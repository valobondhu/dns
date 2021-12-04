# Understanding views in BIND 9, with examples                           

------

Views in BIND have a bad reputation, with some  people advocating that they should not be used. It is true that views  add complexity to a BIND configuration, but this article will explain  how that complexity can be managed and allow views to be used  effectively.

One of the first thing to understand with views is that the zones  loaded into memory in each view are completely independent of any other  copy of that zone loaded in any other view.

Caches and views

 Caches are also unique to each view by  default. However, starting with BIND 9.7.0 they can be explicitly shared using the attach-cache option. See [How do I configure multiple views to share the same recursive cache?](https://kb.isc.org/docs/aa-00835) for more details.

Running BIND 9.10.0 or greater?

 Starting with BIND 9.10.0 a new zone option, "in-view", was added that lets multiple views refer to the same  in-memory instance of a zone. This breaks the model presented in this  article for those zones while providing a savings in both memory usage  and configuration complexity. See the Administrator Reference Manual  (ARM) for your version of BIND for more details.

One way to think about multiple views is as if each view were its own instance of BIND, with a fancy multiplexer in front of them all that  determines which instance will receive each incoming request packet.

When you think about each view as its own process, it makes it easy  to see that for secondary zones each view needs to receive its own `NOTIFY` message when zone data changes. It also follows naturally that each  view needs to have a means of requesting zone data from the correct view on the primary (assuming that the primary also has multiple views). All of this applies even when the primary and secondary are located in  different views within the same BIND instance!

The multiplexer in front of the views can use up to three different  pieces of information about an incoming request to determine which view  it should be sent to: the source IP address (with the `match-clients` statement), the destination address (with the `match-destinations` statement), and whether or not the request is signed with a specific key (with both `match-clients` and `match-destinations`).

Let's get started with the examples.

These Examples are Simplified

 In order to more clearly demonstrate the  principles of managing views, these examples do not go out of their way  to also demonstrate other principles of good server management, such as  allowing dynamic updates or good security practices.

#### Example 1 - A single server, no zones in common

In the first example we have, perhaps, the simplest possible setup  with multiple views — a single server with views that don't need to  share any zone data.

![b3d504e4-e1b3-4f0b-9010-2cda9fc50acb.png](https://cdn.document360.io/956e37e2-5ec0-4942-8b27-35533899f099/Images/Documentation/b3d504e4-e1b3-4f0b-9010-2cda9fc50acb.png)

```shell
# named.example01.conf

acl trusted { 192.168.7.0/24; localhost; };
acl guest   { 192.168.8.0/24; };

view trusted {
     match-clients { trusted; };
     
     allow-recursion { any; };
     
     zone "myzone.example" {
          type primary;
          file "db.myzone.example";
     };
     zone "7.168.192.in-addr.arpa" {
          type primary;
          file "db.192.168.7"; 
     };
};
     
view guest {
     
     match-clients { guest; };
     allow-recursion { any; };
};
```

Shell

This setup is something like someone might have in a small office (or maybe at home) with a trusted network for known clients and an  untrusted guest network for visitors. This setup features a local zone  that only the devices on the trusted network can see, only acts as a  resolver for local clients, and does not allow clients on the guest  network to "snoop" in the cache that "trusted" clients use.

How views affect rndc commands

 As long as a zone name is unambiguous you can use it with `rndc` commands the same as when not using views. If the zone exists in  multiple views then it will need to be qualified by both the class  (nearly always `IN` for internet) and the view name.

#### Example 2 - A single server, one common (but different) zone

On the same network as before, suppose that the operator decides that they want to provide names to guests for a few select services that  they offer within the network.

```shell
# named.example02.conf

acl trusted { 192.168.7.0/24; localhost; };
acl guest   { 192.168.8.0/24; };

view trusted {
     match-clients { trusted; };
     
     allow-recursion { any; };
     
     zone "myzone.example" {
          type primary;
          file "trusted/db.myzone.example";
     };
     zone "7.168.192.in-addr.arpa" {
          type primary;
          file "trusted/db.192.168.7";
     };
};

view guest {
     match-clients { guest; };
     
     allow-recursion { any; };
     
     zone "myzone.example" {
          type primary;
          file "guest/db.myzone.example";
     };
};
```

Shell

Note that a subdirectory has been created for each view and that the  zone data files for both views have been placed in their respective  subdirectory. Keeping the zone data files separated this way will help  prevent confusion and problems later.

#### Example 3 - Adding a second server

DNS servers are social creatures and like to have company, so the  operator of this network has decided to add a second DNS server.

![296feee4-b21a-45cf-8802-22882a43380a.png](https://cdn.document360.io/956e37e2-5ec0-4942-8b27-35533899f099/Images/Documentation/296feee4-b21a-45cf-8802-22882a43380a.png)

Thinking about each view as an independent server helps us to  remember that we need to make a way for each view on Server 2 to talk to the same view on Server 1. If we don't do anything explicitly to make  it happen then both views on Server 2 will be talking to the trusted  view on Server 1 because of Server 2's IP address. While we could use  set up additional IPs for this, using TSIG keys gives us the most  flexibility and doesn't require any OS changes to configure the  additional IPs on the servers.

Further information on TSIG

 For information on generating and using  TSIG, see chapter 4 of the BIND 9 Administrator Reference Manual (ARM)  appropriate for your version. The ARM for many BIND versions can be  found by searching in this Knowledgebase. A copy of the ARM is also  included with every BIND 9 source tarball and Windows .zip file  downloaded from ISC.

Version 9.9.0+ required for these examples

 The following configurations require BIND version 9.9.0 or greater. The `also-notify` statement does not allow keys to be specified in any lower versions. Without the ability to specify a key in the `also-notify` statement you have to use other ways to distinguish between views, such as the method described in [My  secondary server for both an internal and an external view has both  views transferred from the same primary view - how to resolve?](https://kb.isc.org/docs/aa-00296).

```shell
# named.example3.server1.conf

key trusted-key {
     algorithm HMAC-MD5;
     secret some-secret;
};

key guest-key {
     algorithm HMAC-MD5;
     secret another-secret;
};

acl trusted { !key guest-key; key trusted-key; 192.168.7.0/24; localhost; };
acl guest   { !key trusted-key; key guest-key; 192.168.8.0/24; };

options {
     notify explicit;
     allow-transfer { none; };
};

view trusted {
     match-clients { trusted; };
     
     allow-recursion { any; };
     
     allow-transfer { key trusted-key; };
     
     zone "myzone.example" {
          type primary;
          file "trusted/db.myzone.example";
          also-notify { 192.168.7.28 key trusted-key; };
     };
     zone "7.168.192.in-addr.arpa" {
          type primary;
          file "trusted/db.192.168.7";
          also-notify { 192.168.7.28 key trusted-key; };
     };
};

view guest {
     match-clients { guest; };
     
     allow-recursion { any; };
     
     allow-transfer { key guest-key; };
     
     zone "myzone.example" {
          type primary; 
          file "guest/db.myzone.example";
          also-notify { 192.168.7.28 key guest-key; };
     };
};
```

Shell

```shell
# named.example3.server2.conf

key trusted-key {
     algorithm HMAC-MD5;
     secret some-secret;
};

key guest-key {
     algorithm HMAC-MD5;
     secret another-secret;
};

acl trusted { !key guest-key; key trusted-key; 192.168.7.0/24; localhost; };
acl guest   { !key trusted-key; key guest-key; 192.168.8.0/24; };

options {
     notify explicit;
     allow-transfer { none; };
};

view trusted {
     match-clients { trusted; };
     
     allow-recursion { any; };
     
     zone "myzone.example" {
          type secondary;
          masters { 192.168.7.27 key trusted-key; };
          file "trusted/db.myzone.example";
     };
     zone "7.168.192.in-addr.arpa" {
          type secondary;
          masters { 192.168.7.27 key trusted-key; };
          file "trusted/db.192.168.7";
     };
};

view guest {
     match-clients { guest; };
     
     allow-recursion { any; };
     
     zone "myzone.example" {
          type secondary;
          masters { 192.168.7.27 key guest-key; };
          file "guest/db.myzone.example";
     };
};
```

Shell

The "`!key ...`" parts of the acls are  important to force signed requests to be rejected from inappropriate  matches before they have a chance to match on the source IP.

Note that, as written, this  example allows anyone, on any network,  with the TSIG key to sign regular DNS requests in order to select which  view they want their answer from and also to request zone transfers.  Depending on who has the key and what their motivation is, this could be considered to be either a serious problem or a nice feature. See [Using Access Control Lists (ACLs) with both addresses and keys](https://kb.isc.org/docs/aa-00723) for more information on how these ACLs could be improved.

Another significant thing that this example demonstrates in the  server1 configuration is that many options can be specified in both a  "global" options section and inside of views, with the value specified  in the view overriding the "global" value.

I've chosen to set the `notify` option to "`explicit`". This disables the default behavior of sending `NOTIFY` messages to every name server listed in the zone data (except for the  primary listed in the SOA, which is already exempted) because those `NOTIFY` messages would be sent unsigned instead of with the proper view identification key.

#### Example 4 - Now with shared zones

Since the last example, the network operator has decided that they're going to create DDNS records for their guest clients in a set of new  zones. While this information will mostly be used by systems on the  trusted network, the operator would like to allow the guest clients to  see their own dynamically-created DNS records, which means making the  same zones available in both views.

The key to making this work is to once again think about the views as independent servers and so to create an explicit relationship between  the two views. We continue to use keys to make sure that the `NOTIFY` messages and transfer requests get sent to the correct view, where the  key used matches the view that we want to receive the message.

Version 9.9.0+ required for these examples

 As with example 3, this example also  requires BIND version 9.9.0 or higher. For information about how to  accomplish this with lower versions of BIND 9, see [How do I share a dynamic zone between multiple views?](https://kb.isc.org/docs/aa-00295).

```shell
named.example4.server1.conf

key trusted-key {
     algorithm HMAC-MD5;
     secret some-secret;
};

key guest-key {
     algorithm HMAC-MD5;
     secret another-secret;
};

key update-key {
     algorithm HMAC-MD5;
     secret yet-another-secret;
};

acl trusted { !key guest-key; key trusted-key; 192.168.7.0/24; localhost; };
acl guest   { !key trusted-key; key guest-key; 192.168.8.0/24; };

options {
     notify explicit;
};

view trusted {
     match-clients { trusted; };
     
     allow-recursion { any; };
     
     allow-transfer { key trusted-key; };
     
     allow-update { key update-key; };
     
     zone "myzone.example" {
          type primary;
          file "trusted/db.myzone.example";
          also-notify { 192.168.7.28 key trusted-key; };
     };
     zone "7.168.192.in-addr.arpa" {
          type primary;
          file "trusted/db.192.168.7";
          also-notify { 192.168.7.28 key trusted-key; };
     };
     zone "guests.example" {
          type primary;
          file "trusted/db.guest.example";
          also-notify {
               192.168.7.28 key trusted-key;
               127.0.0.1    key guest-key;
          };
     };
     zone "8.168.192.in-addr.arpa" {
          type primary;
          file "trusted/db.192.168.8";
          also-notify {
               192.168.7.28 key trusted-key;
               127.0.0.1    key guest-key;
          };
     };
};

view guest {
     match-clients { guest; };
     
     allow-recursion { any; };
     
     allow-transfer { key guest-key; };
     
     zone "myzone.example" {
          type primary;
          file "guest/db.myzone.example";
          also-notify { 192.168.7.28 key guest-key; };
     };
     zone "guests.example" {
          type secondary;
          masters { 127.0.0.1 key trusted-key; };
          file "guest/db.guest.example";
          also-notify { 192.168.7.28 key guest-key; };
     };
     zone "8.168.192.in-addr.arpa" {
          type secondary;
          masters { 127.0.0.1 key trusted-key; };
          file "guest/db.192.168.8";
          also-notify { 192.168.7.28 key guest-key; };
     };
};
```

Shell

```shell
## named.example4.server2.conf

key trusted-key {
     algorithm HMAC-MD5;
     secret some-secret;
};

key guest-key {
     algorithm HMAC-MD5;
     secret another-secret;
};

acl trusted { !key guest-key; key trusted-key; 192.168.7.0/24; localhost; };
acl guest   { !key trusted-key; key guest-key; 192.168.8.0/24; };

view trusted {
     match-clients { trusted; };
     
     allow-recursion { any; };
     
     zone "myzone.example" {
          type secondary;
          masters { 192.168.7.27 key trusted-key; };
          file "trusted/db.myzone.example";
     };
     zone "7.168.192.in-addr.arpa" {
          type secondary;
          masters { 192.168.7.27 key trusted-key; };
          file "trusted/db.192.168.7";
     };
     zone "guests.example" {
          type secondary;
          masters { 192.168.7.27 key trusted-key; };
          file "trusted/db.guest.example";
     };
     zone "8.168.192.in-addr.arpa" {
          type secondary;
          masters { 192.168.7.27 key trusted-key; };
          file "trusted/db.192.168.8";
     };
};

view guest {
     match-clients { guest; };
     
     allow-recursion { any; };
     
     zone "myzone.example" {
          type secondary;
          masters { 192.168.7.27 key guest-key; };
          file "guest/db.myzone.example";
     };
     zone "guests.example" {
          type secondary;
          masters { 192.168.7.27 key guest-key; };
          file "guest/db.guest.example";
     };
     zone "8.168.192.in-addr.arpa" {
          type secondary;
          masters { 192.168.7.27 key guest-key; };
          file "guest/db.192.168.8";
     };
};
```

Shell

​          