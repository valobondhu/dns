









We will edit the` /etc/named.conf.local` file on the primary server `(ns1.computingforgeeks.local) `and add the `allow-transfer` and `also-notify` parameters.

```
sudo vim /etc/bind/named.conf.local
```



```
##Forward zone
zone "computingforgeeks.local" IN { // Domain name
    
      type master; // Primary DNS

     file "/etc/bind/forward.computingforgeeks.local.db"; // Forward lookup file

     allow-update { none; }; // Since this is the primary DNS, it should be none.
     allow-transfer  { 172.16.10.10; }; //Allow Transfer of zone from the master server

     also-notify { 172.16.10.10; }; //Notify slave for zone changes

};

##Reverse zone
zone "10.16.172.in-addr.arpa" IN { //Reverse lookup name, should match your network in reverse order

     type master; // Primary DNS

     file "/etc/bind/reverse.computingforgeeks.local.db"; //Reverse lookup file

     allow-update { none; }; //Since this is the primary DNS, it should be none.

     allow-transfer  { 172.16.10.10; }; //Allow Transfer of zone from the master server

     also-notify { 172.16.10.10; }; //Notify slave for zone changes

};
```

The `allow-transfer `parameter allows transfer of zone files from the master to the slave DNS while the `also-notify` helps notify the slave whenever there is an update on the zone files from the master.







------



--------



# DNS BIND Zone Transfers and Updates

This chapter describes all the statements available in BIND 9.x relating to zone transfers and Updates. [Full list of statements](http://www.zytrax.com/books/dns/ch7/statements.html).

1. [allow-notify](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-notify)
2. [allow-transfer](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-transfer)
3. [allow-update](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-update)
4. [allow-update-forwarding](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-update-forwarding)
5. [also-notify](http://www.zytrax.com/books/dns/ch7/xfer.html#also-notify)
6. [alt-transfer-source[-v6\]](http://www.zytrax.com/books/dns/ch7/xfer.html#alt-transfer-source)
7. [ixfr-from-differences](http://www.zytrax.com/books/dns/ch7/xfer.html#ixfr-from-differences)
8. [max-journal-size](http://www.zytrax.com/books/dns/ch7/xfer.html#max-journal-size)
9. [max-refresh-time, min-refresh-time](http://www.zytrax.com/books/dns/ch7/xfer.html#max-refresh-time)
10. [max-retry-time, min-retry-time](http://www.zytrax.com/books/dns/ch7/xfer.html#max-retry-time)
11. [max-transfer-idle-in](http://www.zytrax.com/books/dns/ch7/xfer.html#max-transfer-idle-in)
12. [max-transfer-idle-out](http://www.zytrax.com/books/dns/ch7/xfer.html#max-transfer-idle-out)
13. [max-transfer-time-in](http://www.zytrax.com/books/dns/ch7/xfer.html#max-transfer-time-in)
14. [max-transfer-time-out](http://www.zytrax.com/books/dns/ch7/xfer.html#max-transfer-time-out)
15. [max-transfer-idle-in](http://www.zytrax.com/books/dns/ch7/xfer.html#max-transfer-idle-in)
16. [multi-master](http://www.zytrax.com/books/dns/ch7/xfer.html#multi-master)
17. [max-transfer-idle-in](http://www.zytrax.com/books/dns/ch7/xfer.html#max-transfer-idle-in)
18. [notify](http://www.zytrax.com/books/dns/ch7/xfer.html#notify)
19. [notify-source](http://www.zytrax.com/books/dns/ch7/xfer.html#notify-source)
20. [notify-source-v6](http://www.zytrax.com/books/dns/ch7/xfer.html#notify-source-v6)
21. [provide-ixfr](http://www.zytrax.com/books/dns/ch7/xfer.html#provide-ixfr)
22. [request-ixfr](http://www.zytrax.com/books/dns/ch7/xfer.html#request-ixfr)
23. [serial-query-rate](http://www.zytrax.com/books/dns/ch7/xfer.html#serial-query-rate)
24. [transfer-format](http://www.zytrax.com/books/dns/ch7/xfer.html#transfer-format)
25. [transfer-source](http://www.zytrax.com/books/dns/ch7/xfer.html#transfer-source)
26. [transfer-source-v6](http://www.zytrax.com/books/dns/ch7/xfer.html#transfer-source-v6)
27. [transfers-in](http://www.zytrax.com/books/dns/ch7/xfer.html#transfers-in)
28. [transfers-out](http://www.zytrax.com/books/dns/ch7/xfer.html#transfers-out)
29. [transfer-per-ns](http://www.zytrax.com/books/dns/ch7/xfer.html#transfer-per-ns)
30. [update-policy](http://www.zytrax.com/books/dns/ch7/xfer.html#update-policy)
31. [use-alt-transfer-source](http://www.zytrax.com/books/dns/ch7/xfer.html#use-alt-transfer-source)

## allow-notify

```
 allow-notify { address_match_list };
```

**allow-notify** applies to slave zones only and defines a [match list](http://www.zytrax.com/books/dns/ch7/address_match_list.html), for example, IP address(es) that are allowed to NOTIFY this server and  implicitly update the zone in addition to those hosts defined in the [masters](http://www.zytrax.com/books/dns/ch7/zone.html#masters) option for the zone. The default behaviour is to allow zone updates only from the [masters](http://www.zytrax.com/books/dns/ch7/zone.html#masters) IP(s). This statement may be used in a [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

```
// allows notify from the defined IPs 
 allow-notify {192.168.0.15; 192.168.0.16; 10.0.0.1;};
 // allows no notifies 
 allow-notify {none;};
```

## allow-transfer

```
 allow-transfer { address_match_list };
 allow-transfer {192.168.0.3;};
```

**allow-transfer** defines a [match list](http://www.zytrax.com/books/dns/ch7/address_match_list.html) e.g. IP address(es) that are allowed to transfer (copy) the zone  information from the server (master or slave for the zone). The default  behaviour is to allow zone transfers to any host. While on its face this may seem an excessively friendly default, DNS data is essentially  public (that's why its there) and the bad guys can get all of it anyway. However if the thought of anyone being able to transfer your precious  zone file is repugnant, or (and this is far more significant) you are  concerned about possible DoS attack initiated by XFER requests, then use the following policy.

```
options {
   ....
   // ban everyone by default
   allow-transfer {"none";};
};
...
zone "example.com" in{
  ....
  // explicity allow the slave(s) in each zone
  allow-transfer {192.168.0.3;};
};
```

This statement may be used in a [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## allow-update

```
allow-update { address_match_list };
allow-update { !172.22.0.0/16;};
```

**allow-update** defines an [address_match_list](http://www.zytrax.com/books/dns/ch7/address_match_list.html) of hosts that are allowed to submit dynamic updates for master zones,  and thus this statement enables Dynamic DNS. The default in BIND 9 is to disallow updates from all hosts, that is, DDNS is disabled by default.  This statement is mutually exclusive with [update-policy](http://www.zytrax.com/books/dns/ch7/xfer.html#update-policy) and applies to master zones only. The example shows DDNS for three  zones: the first disables DDNS explicitly, the second uses an IP-based  list, and the third references a key clause. The allow-update in the  first zone clause could have been omitted since it is the default  behavior. Many people like to be cautious in case the default mode  changes.

```
// named.conf fragment
// key clause is shown only for illustration and would
// normally be included in the named.conf file
key "update-key" {
    ....
};
....
zone "example.net" in{
    type master;
    allow-update {none;}; // no DDNS by default
    ....
};
....
zone "example.com" in{
....type master;
    allow-update {10.0.1.2;}; // DDNS this host only
    ....
};
zone "example.org" in{
    type master;
    allow-update {key "update-key";};
    ....
};
```

In the example.org zone, the reference to the key clause "update-key" implies that the application that performs the update, say nsupdate, is using TSIG and must also have the same shared secret with the same  key-name. This statement may be used in a [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or an [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## allow-update-forwarding

```
 allow-update-forwarding { address_match_list };
```

**allow-update-forwarding** defines a [match list](http://www.zytrax.com/books/dns/ch7/address_match_list.html), for instance, IP address(es) that are allowed to submit dynamic updates to a 'slave' sever for onward transmission to a 'master'. This  statement may be used in [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or an [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## also-notify

The format of **also-notify** changed in BIND9.9 to that shown below. [BIND9.8 (and all prior versions) format](http://www.zytrax.com/books/dns/ch7/xfer.html#also-notify-old).

```
also-notify [port gp-num] [dscp gd-num] { (masters-list|IP-address )[port p-num] [dscp d-num] 
              [key key-name] ; [... ;] };
```

The **also-notify** statement is relevant only with master zones  and defines one or more IP addresses, and optional port numbers, of  servers that will be sent a NOTIFY when the master zone file is  reloaded. The receiving slave controls which port number and which  addresses it will accept NOTIFY messages from using the [allow-notify statement](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-notify) or the [masters statement](http://www.zytrax.com/books/dns/ch7/zone.html#masters). By default BIND9 will send NOTIFY messages to all the target names  (right-hand names) that appear in NS RRs for the zone (this behaviour  can be modified by the [notify statement](http://www.zytrax.com/books/dns/ch7/xfer.html#notify).

**Note:** The **also-notify** statement can appear in a zone file, in which case its scope is the single zone, or in an **options** clause, in which case its scope is all zones, or in a **view** clause, in which case it applies to all zones in the **view**.

The gp-num parameter changes the port number used for NOTIFY for all  the listed servers (the default is port 53). The p-num parameter changes the port number for the specific IP address only. **masters-list** may be used to reference a list of servers (slaves) defined in a [masters clause](http://www.zytrax.com/books/dns/ch7/masters.html) each of which will be sent a NOTIFY. The **key-name** field defines the key to be used to authenticate the NOTIFY when using TSIG and references the name of a [key clause](http://www.zytrax.com/books/dns/ch7/key.html); a corresponding key clause with the same **key-name** must be present in the slave server(s) for the zone. From BIND9.10 the statement also allows the use of a **DiffServ** Differentiated Service Code Point (DSCP) number (range 0 - 95, where  supported by the OS) to be used to identify the traffic classification.  The following example shows an IPv4 name server which will be sent  NOTIFY on port 53 (default) and the second refers to a list of servers  defined in a masters clause each of which will use port 2034:

```
options {
...
};
masters "notify-them" {
...
};
...
zone "example.com" in{
  type master;
  ...
  also-notify {10.0.1.2; "notify-them" port 2034;};
  ...
};
```

#### also-notify Statement (Pre BIND9.9)

```
 also-notify { ip_addr [port ip_port] ; [... ;  ] };
```

**also-notify** defines a list of IP address(es) (and optional  port numbers) that will be sent a NOTIFY when a zone changes (or the  specific zone if the statement is specified in a **zone** clause). These IP(s)s are in addition to those listed in the [NS records](http://www.zytrax.com/books/dns/ch8/ns.html) for the zone. The *also-notify* in a zone is not cumulative with any global *also-notify* statements. If a global [notify](http://www.zytrax.com/books/dns/ch7/xfer.html#notify) statement is 'no' this statement may be used to override it for a specific zone and, conversely, if the global **options** contain a **also-notify** list, setting **notify** 'no' in the zone will override the global option. This statement may be used in normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause. While this statement would normally be used in a zone of type  'master' there is nothing to prevent its use in a 'slave' zone - in  which case the NOTIFY would be triggered following a zone transfer to  the slave.

```
options {
....
    also-notify {10.1.0.15; 172.28.32.7;}; // all zones
....
};
....
zone "example.com" in{
....
    also-notify {10.0.1.2;}; // only this host
....
};
zone "example.net" in{
....
    notify no; // no NOTIFY for zone
....
};
```

## alt-transfer-source[-v6]

```
 alt-transfer-source, alt-transfer-source-v6
 alt-transfer-source ( ipv4_address | * ) [ port ( integer | * )];
```

Applies to slave zones only. Defines an alternative local IP address  to be used for inbound zone transfers by the server if that defined by [transfer-source](http://www.zytrax.com/books/dns/ch7/xfer.html#transfer-source) ([transfer-source-v6](http://www.zytrax.com/books/dns/ch7/xfer.html#transfer-source-v6)) fails and [use-alt-transfer-source](http://www.zytrax.com/books/dns/ch7/xfer.html#use-alt-transfer-source) is enabled. This address must appear in the remote end's [allow-transfer](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-transfer) statement for the zone being transferred. This statement may be used in a [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## ixfr-from-differences

```
 ixfr-from-differences (yes | no);
 ixfr-from-differences yes;
```

Defines how the server calculates incremental zone changes. Normally  incremental zone transfers are only possible when used in conjunction  with Dynamic DNS (DDNS). **ixfr-from-diffrences** allows the slave server to create incremental zone transfers for non-dynamic zones. If set to **yes** when the server receives a new version of a slave file by a  non-incremental zone transfer it will compare the new version to the  previous one and calculate a set of differences. The differences are  then logged in the zone's journal file (.jnl appended to zone file name) such that the changes can be transmitted to downstream slaves as an  incremental zone transfer. This statement saves bandwidth at the expense of increased CPU and memory consumption. This statement may be used in a [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## max-journal-size

```
 max-journal-size size_in_bytes;
 max-journal-size 50k;
```

Sets a maximum size in bytes (may take the case insensitive k or m  shortforms) for each journal file. When the journal file approaches the  specified size, some of the oldest transactions in the journal will be  automatically removed. The default is unlimited. Journal files are used  by Dynamic DNS (DDNS) when modifying the master and when receiving IXFR  changes on slave zones. The journal file is in binary format and its  name is formed by appending the extension .jnl to the name of the  corresponding zone file.

All changes made to a zone using dynamic update are written to the  zone's journal file. The server will periodically flush the complete  contents of the updated zone to its zone file this happens approximately every 15 minutes. When a server is restarted after a shutdown or crash, it will replay the journal file to incorporate into the zone any  updates that took place after the last zone file update.

If changes have to be made manually to a dynamic zone then use the following sequence:

1. Disable dynamic updates to the zone using *rndc freeze zone* which causes the zone file to be updated.
2. Edit the zone file
3. Run *rndc unfreeze zone* to reload the changed zone and re-enable dynamic updates

This statement may be specified in a [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## max-refresh-time, min-refresh-time

```
 max-refresh-time seconds ;
 min-refresh-time seconds ;
```

Only valid for slave or stub zones. The refresh time is normally defined by the [SOA RR *refresh* parameter](http://www.zytrax.com/books/dns/ch8/soa.html). This allows the slave server administrator to override the definition  and substitute the values defined. The values may take the normal [time short-cuts](http://www.zytrax.com/books/dns/apa/time.html). This statement may be specified in a [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) or global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## max-retry-time, min-retry-time

```
 max-retry-time seconds ;
 min-retry-time seconds ;
```

Only valid for slave and stub zones. The retry time is normally defined by the [SOA RR *retry* parameter](http://www.zytrax.com/books/dns/ch8/soa.html). This allows the slave server administrator to override the definition  and substitute the values defined. The values may take the normal [time short-cuts](http://www.zytrax.com/books/dns/apa/time.html). This statement may be specified in normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## max-transfer-idle-in

```
 max-transfer-idle-in minutes ;
```

Only valid for slave zones. Inbound zone transfers making no progress in this many **minutes** will be terminated. The default is 60 minutes (1 hour). The maximum  value is 28 days (40320 minutes). This statement may be specified in  normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## max-transfer-idle-out

```
 max-transfer-idle-out minutes ;
```

Only valid for master zones. Outbound zone transfers running longer than this many **minutes** will be terminated. The default is 120 minutes (2 hours). The maximum  value is 28 days (40320 minutes). This statement may be specified in  normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## max-transfer-time-in

```
 max-transfer-time-in minutes ;
```

Only valid for slave zones. Inbound zone transfers running longer than this many **minutes** will be terminated. The default is 120 minutes (2 hours). The maximum  value is 28 days (40320 minutes). This statement may be specified in  normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## max-transfer-time-out

```
 max-transfer-time-out minutes ;
```

Only valid for 'type master' zones. Outbound zone transfers running longer than this many **minutes** will be terminated. The default is 120 minutes (2 hours). The maximum  value is 28 days (40320 minutes). This statement may be specified in  normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## multi-master

```
 multi-master yes | no ;
```

Relevant only when multiple masters are defined for a slave zone.  Controls whether a log entry will be generated each time the serial  number is *less* than that currently maintained  by the slave (no)  or not (yes). This situation can occur when the zone masters are out of  sync with each other. Default is no. This statement may be specified in  normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## notify

```
 notify yes | no | explicit;
```

**notify** behaviour is applicable to both [master zones](http://www.zytrax.com/books/dns/ch7/zone.html) (with 'type master;') and [slave zones](http://www.zytrax.com/books/dns/ch7/zone.html) (with 'type slave;') and if set to 'yes' (the default) then, when a  zone is loaded or changed, for example, after a zone transfer, NOTIFY  messages are sent to the name servers defined in the [NS records](http://www.zytrax.com/books/dns/ch8/ns.html) for the zone (except itself and the 'Primary Master' name server defined in the [SOA record](http://www.zytrax.com/books/dns/ch8/soa.html)) and to any IPs listed in any [also-notify](http://www.zytrax.com/books/dns/ch7/xfer.html#also-notify) statement.

If set to 'no' NOTIFY messages are not sent.

If set to 'explicit' NOTIFY is only sent to those IP(s) listed in an [**also-notify**](http://www.zytrax.com/books/dns/ch7/xfer.html#also-notify) statement.

If a global **notify** statement is 'no' an [**also-notify**](http://www.zytrax.com/books/dns/ch7/xfer.html#also-notify) statement may be used to override it for a specific zone, and conversely if the global **options** contain an [**also-notify**](http://www.zytrax.com/books/dns/ch7/xfer.html#also-notify) list, setting **notify** 'no' in the zone will override the global option. This statement may be specified in [zone](http://www.zytrax.com/books/dns/ch7/zone.html), [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

```
options {
....
    also-notify {10.1.0.15; 172.28.32.7;}; // all zones
....
};
....
zone "example.com in{
....
    // NS RRs and global also-notify
    notify yes; 
....
};
zone "example.net in{
....
    // no NOTIFY to NS RRs
    // NOTIFY to also-notify IPs above
    notify explicit; 
....
};
```

**Notes:**

1. NOTIFY does not indicate that the zone data has changed, but  rather that the zone data may have changed. The receiver of the NOTIFY  message should query the zone SOA directly from the IP(s) defined in the zone's [masters](http://www.zytrax.com/books/dns/ch7/zone.html#masters) statement.
2. Even if the implementation includes the zone's SOA in the NOTIFY  message (allowed for in the standards) the receiver is mandated NOT to  use this data (by [RFC 1996](http://www.zytrax.com/books/dns/apd/rfc1996.txt)). Instead the receiving server must query the zone's SOA from the IP(s) defined in the [masters](http://www.zytrax.com/books/dns/ch7/zone.html#masters) statement.
3. By default, after a slave has transferred a zone it will also  send out NOTIFY messages to all the zone's NS RRs (except itself  obviously). This behavior can be inhibited by using a 'notify no;'  statement in the slave's zone clause.

## notify-source

```
 notify-source (ip4_addr | *) [port ip_port] ;
```

**notify-source** defines the IPv4 address (and optionally port)  to be used for outgoing NOTIFY operations. The value '*' means the IP of this server (default). This IPv4 address must appear in the [masters](http://www.zytrax.com/books/dns/ch7/zone.html#masters) or [allow-notify](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-notify) statement for the receiving slave name servers. Since neither the  masters nor allow-notify statements take a port parameter if the  optional port value is used a [listen-on](http://www.zytrax.com/books/dns/ch7/location.html#listen-on) or [listen-on-v6](http://www.zytrax.com/books/dns/ch7/location.html#listen-on-v6) statement would be required on the slave. Typically only used on  multi-homed servers. This statement may be specified in normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## notify-source-v6

```
 notify-source-v6 (ip6_addr | *) [port ip_port] ;
```

**notify-source-v6** defines the IPv6 address (and optionally  port) to be used for outgoing NOTIFY operations. The value '*' means the IP of this server (default). This IPv6 address must appear in the [masters](http://www.zytrax.com/books/dns/ch7/zone.html#masters) or [allow-notify](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-notify) option for the receiving slave name servers. Typically only used on  multi-homed servers. This statement may be specified in normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## provide-ixfr

```
 provide-ixfr yes|no ;
```

The **provide-ixfr** option defines whether a master will respond  to an incremental zone transfer request(IXFR) (option = yes) or will  respond with a full zone transfer (AXFR) (option = no). The default is **yes**. This statement may be specified in normal [server](http://www.zytrax.com/books/dns/ch7/server.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## request-ixfr

```
 request-ixfr yes|no ;
```

Applies to slave zones only. The **request-ixfr** option defines  whether a server will request an incremental  zone transfer (IXFR)  (option = yes) or will request a full zone transfer (AXFR) (option =  no). The default is **yes**. This statement may be specified in normal [server](http://www.zytrax.com/books/dns/ch7/server.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## serial-query-rate

```
 serial-query-rate number;
 serial-query-rate 5;
```

Applies to slave zones only and limits the number of simultaneous SOA queries to the **number** per second. The default is 20. This statement may only be used in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## transfer-format

```
 transfer-format ( one-answer | many-answers );
```

Only used by master zones. **transfer-format** determines the  format the server uses to transfer zones. 'one-answer' places a single  record in each message, 'many-answers' packs as many records as possible into a maximum sized message. The default is 'many-answers' which is  ONLY KNOWN TO BE SUPPORTED BY BIND 9, BIND 8 and later BIND 4 releases  so if tranferring to other servers e.g. Windows this statement may be  required. This statement may be specified in [server](http://www.zytrax.com/books/dns/ch7/server.html), [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## transfer-source

```
 transfer-source (ip4_addr | *) [port ip_port] ; ]
```

Only valid for 'type slave' zones. **transfer-source** determines  which local IPv4 address will be bound to TCP connections used to fetch  zones transferred inbound by the server. It also determines the source  IPv4 address, and optionally the UDP port, used for the refresh queries  and forwarded dynamic updates. If not set, it defaults to a BIND  controlled value which will usually be the address of the interface  "closest to" the remote end. This address must appear in the remote  end's [**allow-transfer**](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-transfer)  option for the zone being transferred, if one is specified. This statement may be specified in normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## transfer-source-v6

```
 transfer-source-v6 (ip6_addr | *) [port ip_port] ; ]
```

Only valid for 'type slave' zones. **transfer-source** determines  which local IPv6 address will be bound to TCP connections used to fetch  zones transferred inbound by the server. It also determines the source  IPv4 address, and optionally the UDP port, used for the refresh queries  and forwarded dynamic updates. If not set, it defaults to a BIND  controlled value which will usually be the address of the interface  "closest to" the remote end. This address must appear in the remote  end's [**allow-transfer**](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-transfer)  option for the zone being transferred, if one is specified.  This statement may be specified in normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## transfers-in

```
 transfers-in number ;
```

Only used by slave zones. **transfer-in** determines the number of concurrent inbound zone transfers. Default is 10. This statement may only be defined in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## transfers-out

```
 transfers-out number ;
```

Only used by master zones. **transfers-out** determines the number of concurrent outbound zone transfers. Default is 10. Zone transfer  requests in excess of this limit will be REFUSED. This statement may  only be defined in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## transfers-per-ns

```
 transfers-per-ns number ;
```

Only used by slave zones. **transfer-per-ns** determines the  number of concurrent inbound zone transfers for any zone. Default is 2.  This statement may only be defined in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

## update-policy

```
update-policy ( local | { update-policy-rule; } );
update-policy { grant fred.example.net name example.net MX;};
```

**update-policy** only applies to, and may only appear in, [zone](http://www.zytrax.com/books/dns/ch7/zone.html) clauses. This statement defines the rules by which DDNS updates may be  carried. It may only be used with a key (TSIG or SIG(0)) which is used  to cryptographically sign each update request. It is mutually exclusive  with [ allow-update](http://www.zytrax.com/books/dns/ch7/xfer.html#allow-update) in any single zone clause. The statement may take the keyword **local** or an **update-policy-rule** structure. The keyword **local** is designed to simplify configuration of secure updates using a TSIG  key and limits the update source only to localhost (loopback address,  127.0.0.1 or ::1), thus both [nsupdate](http://www.zytrax.com/books/dns/ch10/nsupdate.html) (or any other application using DDNS) and the name server being updated must reside on the same host.

When BIND encounters an **update-policy local;** statement it  generates a TSIG key (with the algorithm HMAC-SHA256) and a private key  file only in /var/run/named/session.key (location may be modified using  the **session-keyfile** statement) and with a key-name of "local-ddns". (**Note:** The use of the HMAC-SHA256 algorithm means that DHCP cannot use this key.) This session key is also used by **nsupdate** when the -l argument is supplied. The **local** keyword is expanded to an equivalent update-policy-rule as shown:

```
update-policy local;
// expanded by BIND to
update-policy {grant local-ddns zonesub any;};
```

The effect of this statement is to allow any DDNS update signed with  the key-name "local-ddns" to update any RRs with the name of the zone  file or a subzone (any name to the left) of the zone name (as it appears in the zone clause) in which the **update-policy local;** statement appears. Thus, if **update-policy local;** appears in the example.com zone clause then any update from **localhost** is allowed to update RRs with a name of example.com, www.example.com, joe.example.com, and so on. The **update-policy local;** statement may be used in one or more zone clauses, while other zone clauses may use the **update-policy update-policy-rule;** format. See the following section for a full explanation of all the fields in the expanded **update-policy local;** statement.

**update-policy-rule** takes the following format:

```
update-policy-rule permission identity matchtype [tname] [rr]
```

| Parameter  | Description                                                  |
| ---------- | ------------------------------------------------------------ |
|            |                                                              |
| permission | May be either grant or deny.                                 |
| identity   | A key-name as it appears in a key clause for TSIG or the name of a  KEY RR for SIG(0). Can also take the DNS wildcard value * which is  expanded to mean anything matches. |
| matchtype  | Can take any of the following values: **6to4-self:** Only applicable to reverse mapped zones updates.  The RR name to be updated must match the 6to4 (48 bits only) reverse  mapped name of the IPv4 address that intiated this update session. Thus, if the source of the update session is 192.168.2.3 this added to the  IPv6 6to4 prefix (always 2002::/16) to create the address  2002:C0A8:0203::/48 (C0A80203 is the hex value of 192.168.2.3) which  when reversed will yield an RR name of 3.0.2.0.8.A.0.C.2.0.0.2.IP6.ARPA  and thus allow any RR names at this zone apex, such as NS or DNAME, to  be modified or added. **external:** Indicates that bind will call an external application using a UNIXdomain socket address defined in **identity**. The format of the **identity** field in this case is **local:path** where **local** is a keyword indicating a local socket and **path** is the socket address. **krb5-self:** This rule takes a Kerberos machine principal  (host/QDN@REALM) and allows it to update the DNS entry which corresponds to the QDN part of the Principal. The REALM to be matched must exactly  match that specified in **identity**. See [Kerberos/AD note](http://www.zytrax.com/books/dns/ch7/xfer.html#kerberos-ad-note). **krb5-subdomain:** This rule takes a Kerberos machine principal  (host/QDN@REALM) and allows it to update the QDN part of the Principal.  The REALM to be matched must match that specified in **identity** or any subdomain (labels to the left) of **identity**. See [Kerberos/AD note](http://www.zytrax.com/books/dns/ch7/xfer.html#kerberos-ad-note). **ms-self:** This rule takes an AD format principal  (machinename$@REALM) and allows it to update machinename.realm in the  DNS. The REALM to be matched must exactly match that specified in **identity**. See [Kerberos/AD note](http://www.zytrax.com/books/dns/ch7/xfer.html#kerberos-ad-note). **ms-subdomain:** This rule takes an AD format principal  (machinename$@REALM) and allows it to update machinename.realm. The  REALM to be matched must match that specified in **identity** or any subdomain (labels to the left) of **identity**. See [Kerberos/AD note](http://www.zytrax.com/books/dns/ch7/xfer.html#kerberos-ad-note). **name:** The RR name being updated must match the **tname** field exactly. That is, if **tname** is joe.example.com., then this update-policy can only update an RR with the name joe.example.com. **self:** The RR name being updated must match the **identity** field exactly including the DNS wildcard value(*). Thus, if **identity** is * this update-policy will update an RR with any name, if **identity** is example.com then only an RR with the name example.com may be updated The optional **tname** field should be present with the same as **identity**. **selfsub:** The RR name being updated must match the **identity** field or a subdomain (including any label to the left) of **identity**. Thus, if **identity** is example.com then this update-policy will update any RR with the name example.com or joe.example.com and so on. The optional **tname** field should be present with the same name as **identity**. **selfwildcard:** The RR name being updated can only match a subdomain of the **identity** field  Thus, if **identity** is example.com this update-policy can only update RRs with a name of  joe.example.com or sheila.example.com, and so on, but not RRs with a  name of example.com. The optional **tname** field is ignored but should be the same as **identity**. **subdomain:** The RR name being updated matches anything containing (is a subdomain of or has labels to the left of) the **tname** field. Thus, if **tname** is example.com., this update-policy will match any RRs with a name of  bill.example.com, sheila.example.com and so on, as well as example.com.  **tcp-self:** Only applicable to reverse mapped zones updates. The RR name to be updated must match the reverse mapped name of the IP  address (IPv4 or IPv6) that intiated this update session. Thus, if the  source of the update session is 192.168.2.27 and the update-policy  appears in a zone 2.168.192.IN-ADDR.ARPA then the RR name must match 27  which when fully expanded (using ORIGIN substitution) becomes  27.2.168.192.IN-ADDR.ARPA. If the source address is IPv6 then the  reverse mapping occurs in the IP6.ARPA reverse map domain. **wildcard:** The RR name being updated will match the **tname** field after any DNS wildcard expansion has been applied. The **tname** field must contain at least one wildcard (*) and may be a single * in which case this update can apply to any RR name. **zonesub:** The RR name being updated must match anything  containing the zone name (as it appears in the zone clause containing  this update-policy), including subdomains (any labels on the left) of  this zone name. The optional **tname** field must be omitted when using this form. |
| tname      | Optional. The name of the target or part of the target RR name  (depending on the value of matchtype) that will be allowed by this  update-poilicy. Can take the value * which means any RR name. |
| rr         | Optional. Defines the Resource Record types that may be updated  including ANY (all RR types except NSEC/NSEC3). If omitted, the default  allows all RR types except RRSIG, NSEC, NSEC3, SOA, and NS. Multiple  entries may be defined using space-separated entries, for instance, A MX PTR. |

**Note about Kerberos/AD Formats:** [Kerberos V5](http://www.zytrax.com/tech/survival/kerberos.html) (Microsoft's AD DS is based on Kerberos V5 so this note applies also). The Principal name formats accepted by **matchtype** are restricted for **krb5-** and **ms-** types.

When using either **ms-** type the format expected is  machinename$@REALM (where REALM is assumed to be in domain name format). The REALM part is compared with the **identity** field (or any valid subdomain if **ms-subdomain**). If they match (case sensitive compare) then the transaction is allowed  to update a DNS name of machinename.realm (case insesitive). This is  illustrated by the following example:

```
update-policy {grant EXAMPLE.COM ms-self EXAMPLE.COM A AAAA;}; 
```

This will allow an incoming transaction with the principal name of  joe$@EXAMPLE.COM to update (in this case) the A or AAAA RRs with a name  of joe.example.com (case insensitive).

When using either **krb5-** type the format expected is  host/QDN@REALM (where REALM is assumed to be in domain name format, and  QDN is assumed to be a qualified domain name - without the terminating  dot). The REALM part is compared with the **identity** field (or any valid subdomain if **ms-subdomain**). If they match (case sensitive compare) and the keyword **host** appears in the Principal name then the transaction is allowed to update a DNS name of QDN (case insesitive). This is illustrated by the  following example:

```
update-policy {grant EXAMPLE.COM krb5-self EXAMPLE.COM A AAAA;}; 
```

This will allow an incoming transaction with the principal name of  host/joe.example.com@EXAMPLE.COM to update (in this case) the A or AAAA  RRs with a name of joe.example.com (case insensitive). RFC 4120 Section  6.2.1 suggests that only a subset of any server Principal names should  use the keyword **host** which suggests this format may be excessively restrictive for non-MS Kerberos.

The following example shows the use of update-policy whereby each host can update its own A RR but no others:

```
zone "example.com" in {
    type master;
     ....
    update-policy { grant * self * A;};
};
```

The policy says that any KEY RR name, or key-name as it appears in a  key clause, (the first *) with the same name (self) as the A RR it is  trying to update (the second *) will be allowed (grant) to do so.

The next example shows mixed use of the local and update-policy-rule formats:

```
zone "example.com" in {
    type master;
     ....
    update-policy local;  // allow updates to any RR but only from localhost
};
zone "example.net" in {
    type master;
     ....
    update-policy { grant "remote-key" name example.com MX;};

};
```

The first zone clause allows DDNS updates to any RR in the zone but  only from localhost. The second (example.net) zone allows updates from  any TSIG signed transaction with the key-name of "remote-key" (there  must be a key clause with the name "remote-key" in this named.conf) but  only to the MX RR at the zone apex.

## use-alt-transfer-source

```
 use-alt-transfer-source yes | no;
```

Use [alt-transfer-source[-v6\]](http://www.zytrax.com/books/dns/ch7/xfer.html#alt-transfer-source) (yes) statements or not (no). If views are specified this defaults to **no** otherwise it defaults to **yes** (for BIND 8 compatibility). This statement may be specified in normal [zone](http://www.zytrax.com/books/dns/ch7/zone.html) or [view](http://www.zytrax.com/books/dns/ch7/view.html) clauses or in a global [options](http://www.zytrax.com/books/dns/ch7/options.html) clause.

------

