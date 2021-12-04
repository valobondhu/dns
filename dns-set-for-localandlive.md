# Two-in-one DNS server with BIND9

### On this page

This tutorial shows you how to configure BIND9 DNS server to serve an internal network and an external network at the same time with  different set of information. To accomplish that goal, a new feature of  BIND9 called view is used. As a tutorial it'll walk you through the  whole set up, but initial knowledge of BIND and DNS is required, there  are plenty of documents that cover that information on the Internet.



## 1 The problem

It is a typical problem in organizations that are growing that they have to resolve two problems at once:



- To have a DNS server for the internal network of the company because long ago there were already too many computers to remember their IPs[1](https://www.howtoforge.com/two_in_one_dns_bind9_views#foot33) and even too many computers to maintain a set of host files[2](https://www.howtoforge.com/two_in_one_dns_bind9_views#foot151).
- To have a DNS server for the external servers, for external clients, etc.

to solve this problems become a bigger problem when the growing organization can't supply more resources than one DNS server[3](https://www.howtoforge.com/two_in_one_dns_bind9_views#foot152). It is a bigger problem because if you just configure your server with all your names, public and private, you'll end up polluting the Internet with private addresses, something that is very bad, and also showing the world part of the topology of your internal network. Something you don't want a possible attacker/cracker to have.

The other part of the problem is that for efficiency you may want to resolve to internal IPs when you are inside and external IPs when you are outside. Here I am taking about computers which have public and private connections.

There are many different solutions to this problem and I remember solving it even with BIND4, but now I am going to use BIND9 to make a solution that is very clean. This was deployed in a Debian GNU/Linux 3.1 server but it should also work for other operating systems that run BIND9, just be sure to change your paths appropriately.



## 2 Initial configuration

Let's imagine the organization I work for makes examples... of what ? I don't know, but you can order them on example.com. Examples Corporation has been assigned the network 192.0.2.0/24 and internally we are using 10.0.0.0/24.

Let's start serving the external names and IPs, we edit /etc/bind/named.conf.local[4](https://www.howtoforge.com/two_in_one_dns_bind9_views#foot153) and add:

```
zone "example.com" {
    type master;
    file "/etc/bind/db.example.com";
};
```

and then we create /etc/bind/db.example.com with the following contents:

```
; example.com
$TTL    604800
@       IN      SOA     ns1.example.com. root.example.com. (
                     2006020201 ; Serial
                         604800 ; Refresh
                          86400 ; Retry
                        2419200 ; Expire
                         604800); Negative Cache TTL
;
@       IN      NS      ns1
        IN      MX      10 mail
        IN      A       192.0.2.1
ns1     IN      A       192.0.2.1
mail    IN      A       192.0.2.128 ; We have our mail server somewhere else.
www     IN      A       192.0.2.1
client1 IN      A       192.0.2.201 ; We connect to client1 very often.
```

As you can see, our start up has one computer to serve all, except mail, it even holds the IP forwarding and a couple of databases.

Now, a good DNS set up has at least one secondary server and in fact, some registrars (where you register domain names) enforce this. Since we don't have a second computer, we go to [XName](http://xname.org), open an account and register example.com as secondary with 192.0.2.1 as IP to transfer from. We now need to let XName's IP do the transfer; we are a small organization but since we want to be a successful start up we try to do everything as smartly as possible. So we use the BIND9 configuration directive acl to define an identifier that aliases to the XName's IP addresses; at the beginning of /etc/bind/named.conf.local we add:

```
acl slaves {
    195.234.42.0/24;    // XName
    193.218.105.144/28; // XName
    193.24.212.232/29;  // XName
};
```

and we change the zone declaration to:

```
zone "example.com" {
    type master;
    file "/etc/bind/db.example.com";
    allow-transfer { slaves; };
};
```

We could have just typed the IPs where we type "slaves".

## 3 Internals and externals

Now that we have a solid base, we can start to thing about serving different contents to the internal and external network, but first, we have to define what is internal and what is external.

On /etc/bind/named.conf.local we add the following definition (at the top or below the definition of slaves):

<iframe style="border: 0px none; vertical-align: bottom;" src="https://1562b57b30d6b86c4455c4f044d8a244.safeframe.googlesyndication.com/safeframe/1-0-38/html/container.html" id="google_ads_iframe_/1254144,1030080/howtoforge_com-box-4_0" title="3rd party ad content" name="" scrolling="no" marginwidth="0" marginheight="0" data-is-safeframe="true" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" role="region" aria-label="Advertisement" tabindex="0" data-google-container-id="9" data-load-complete="true" width="468" height="60" frameborder="0"></iframe>



```
acl internals {
    127.0.0.0/8;
    10.0.0.0/24;
};
```

If we had more internal networks, we could just add them there. We don't define externals because everything that is not internal is external. You may, if you want, define sets of different externals if you want to serve different content to different chunks of the Internet.

We will use a new feature of BIND9 called views. A view let's put a piece of configuration inside a conditional that can depend on a number of things, in this case we'll just depend on internals. We replace the zone declaration at /etc/bind/named.conf.local with:

```
view "internal" {
    match-clients { internals; };
    zone "example.com" {
        type master;
        file "/etc/bind/internals/db.example.com";
    };
};
view "external" {
    match-clients { any; };
    zone "example.com" {
        type master;
        file "/etc/bind/externals/db.example.com";
        allow-transfer { slaves; };
    };
};
```

The match clients configuration directive allow us to conditionally show that view based on a set of IPs, "any" stands for any IP. Internal IPs will be cached by the internal view and the rest will be dropped on the external view. The outside world can't see the internal view, and that includes XName, our secondary DNS provider, but we removed the allow-transfer from the internal view since we don't want anyone to be able to transfer under any circumstances the contents of the internal view.

We also changed the path, we will have to create the directory /etc/bind/externals and /etc/bind/internals and move /etc/bind/db.example.com to /etc/bind/externals/.

On /etc/bind/internals/db.example.com we put a zone file similar to the counterpart on external but holding the internal IPs:Advertisement

<iframe style="border: 0px none; vertical-align: bottom;" src="https://1562b57b30d6b86c4455c4f044d8a244.safeframe.googlesyndication.com/safeframe/1-0-38/html/container.html" id="google_ads_iframe_/1254144,1030080/howtoforge_com-banner-1_0" title="3rd party ad content" name="" scrolling="no" marginwidth="0" marginheight="0" data-is-safeframe="true" sandbox="allow-forms allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" role="region" aria-label="Advertisement" tabindex="0" data-google-container-id="b" data-load-complete="true" width="468" height="60" frameborder="0"></iframe>



```
; example.com
$TTL    604800
@       IN      SOA     ns1.example.com. root.example.com. (
                     2006020201 ; Serial
                         604800 ; Refresh
                          86400 ; Retry
                        2419200 ; Expire
                         604800); Negative Cache TTL
;
@       IN      A       10.0.0.1
boss    IN      A       10.0.0.100
printer IN      A       10.0.0.101
scrtry  IN      A       10.0.0.102
sip01   IN      A       10.0.0.201
lab     IN      A       10.0.0.103
```

Great, we can now ping our boss' computer with

```
ping boss.example.com
```

but trying to reach mail.example.com will disappoint us, what happened ? There's no reference to mail.example.com on the internal zone file and since we are in the internal network we can resolve mail.example.com. Fine, let's just copy the contents of the external zone file to the internal zone file. That'll work.

But we are a small, smart start up, we can do better than copy-paste each modification to the zone file, furthermore, that is very error prone (will you always remember to modify the internal zone file when you modify the external one, or will you forget and spend some days debugging network problems ?).

What we will do is include the external zone file in the internal file this way:

```
$include "/etc/bind/external/db.example.com"
@       IN      A       10.0.0.1
boss    IN      A       10.0.0.100
printer IN      A       10.0.0.101
scrtry  IN      A       10.0.0.102
sip01   IN      A       10.0.0.201
lab     IN      A       10.0.0.103
```

and voila! Finding the $include directive in the current documentation was hard.. Just remember to change the serial of the external zone file whenever you change the internal one, but it is not a big deal, if you forget, nothing bad will happen since you are not likely to have caching servers inside your own small network.Advertisement

<iframe id="google_ads_iframe_/1254144,1030080/howtoforge_com-large-leaderboard-2_0" title="3rd party ad content" name="google_ads_iframe_/1254144,1030080/howtoforge_com-large-leaderboard-2_0" scrolling="no" marginwidth="0" marginheight="0" style="border: 0px none; vertical-align: bottom;" role="region" aria-label="Advertisement" tabindex="0" srcdoc="" data-google-container-id="c" data-load-complete="true" width="728" height="90" frameborder="0"></iframe>



## 4 Security

It is not recommended to use the same DNS server as primary for some domain (in our case example.com) and as caching DNS server, but in our case we are forced to do it. From the outside world 192.0.2.1 is the primary DNS server for example.com, from our own internal network, 192.0.2.1 (or its private address, 10.0.0.1) is our caching name server that should be configured as the nameserver to use on each workstation we have.

One of the problems is that someone might start using our caching nameserver from the outside, there's an attack called cache-poisoning and many other nasty things that can be done and are documented on [[SINS](https://www.howtoforge.com/two_in_one_dns_bind9_views#sins)] (including how to avoid them).

To improve our security a bit, we'll add a couple of directives to the configuration file:

```
view "internal" {    match-clients { internals; };    recursion yes;
    zone "example.com" {
        type master;
        file "/etc/bind/internals/db.example.com";
    };
};
view "external" {
    match-clients { any; };
    recursion no;

    zone "example.com" {
        type master;
        file "/etc/bind/externals/db.example.com";
        allow-transfer { slaves; };
    };
};
```

That will prevent anyone on the dangerous Internet to use our server recursively while we, on our own network, can still do it.

## 5 Configuration files

### 5.1 /etc/bind/named.conf.local

```
acl slaves {
    195.234.42.0/24;    // XName
    193.218.105.144/28; // XName
    193.24.212.232/29;  // XName
};

acl internals {
    127.0.0.0/8;
    10.0.0.0/24;
};

view "internal" {
    match-clients { internals; };
    recursion yes;
    zone "example.com" {
        type master;
        file "/etc/bind/internals/db.example.com";
    };
};
view "external" {
    match-clients { any; };
    recursion no;
    zone "example.com" {
        type master;
        file "/etc/bind/externals/db.example.com";
        allow-transfer { slaves; };
    };
};
```

### 5.2 /etc/bind/externals/db.example.com

```
; example.com
$TTL    604800
@       IN      SOA     ns1.example.com. root.example.com. (
                     2006020201 ; Serial
                         604800 ; Refresh
                          86400 ; Retry
                        2419200 ; Expire
                         604800); Negative Cache TTL
;
@       IN      NS      ns1
        IN      MX      10 mail
        IN      A       192.0.2.1
ns1     IN      A       192.0.2.1
mail    IN      A       192.0.2.128 ; We have our mail server somewhere else.
www     IN      A       192.0.2.1
client1 IN      A       192.0.2.201 ; We connect to client1 very often.
```

### 5.3 /etc/bind/internals/db.example.com

```
$include "/etc/bind/external/db.example.com"
@       IN      A       10.0.0.1
boss    IN      A       10.0.0.100
printer IN      A       10.0.0.101
scrtry  IN      A       10.0.0.102
sip01   IN      A       10.0.0.201
lab     IN      A       10.0.0.103
```

### Bibliography

- SINS

  [Securing an Internet Name Server](http://www.cert.org/archive/pdf/dns.pdf)

### Footnotes

- ... IPs[1](https://www.howtoforge.com/two_in_one_dns_bind9_views#tex2html1)

  For me, two computers already qualify as too many computers to remember their IPs.

- ... files[2](https://www.howtoforge.com/two_in_one_dns_bind9_views#tex2html2)

  The host file resides on /etc/hosts and is a simple mapping for the local computer from names to IP. It was the first way to resolve names to IPs and long ago a central big hosts file was maintained, latter distributed by FTP or similar. It rapidly grow into a problem which was solved by the invention of DNS. Try not to let your hosts file grow into a problem before you turn it into DNS, learn from the pioneers!

- ... server[3](https://www.howtoforge.com/two_in_one_dns_bind9_views#tex2html3)

  Or when you are a perfectionist and believe that this should be doable with only one server.

- .../etc/bind/named.conf.local[4](https://www.howtoforge.com/two_in_one_dns_bind9_views#tex2html4)

  It may be just /etc/bind/named.conf in your operating system if it is not Debian.