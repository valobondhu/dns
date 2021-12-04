​        



Bind DNS sends a notify to all name servers except itself and the primary master listed in the SOA.

- `notify yes;` 
   sends notify to **all name servers** in RR ** (except itself and SOA master)**
- `notify yes; also-notify { x.x.x.x; y.y.y.y; };` 
   sends notify to **x.x.x.x**, **y.y.y.y** and **all name servers** in RR **(except itself and SOA master)**.
- `notify explicit; also-notify { x.x.x.x; y.y.y.y; };` 
   sends notify to just **x.x.x.x**, **y.y.y.y**

----

# BIND 9 and later: NOTIFY syntax    examples

For these examples, assume that the zone transfer agents are 192.0.2.1, 192.0.2.2,      and 192.0.2.3.  See [Restrict zone transfers to the zone transfer agents](https://learn.akamai.com/en-us/webhelp/edge-dns/edge-dns-user-guide/GUID-CFDFF764-E58F-4CB6-82F1-AA5C21CB1B6B.html#GUID-CFDFF764-E58F-4CB6-82F1-AA5C21CB1B6B) to learn how to      subscribe to a list of ZTAs.

For these BIND versions, NOTIFY is enabled using the `notify` and `also-notify` directives.

```
options {
.....
notify yes;
also-notify {192.0.2.1;192.0.2.2;192.0.2.3;};
.....
};
```

Additionally, starting with BIND 9, there is the option of explicit      notification:

```
options {
.....
NOTIFY EXPLICIT;
also-notify {192.0.2.1;192.0.2.2;192.0.2.3;};
.....
};
```

If notify is yes (the default), DNS NOTIFY messages are sent when a zone for which      the server is authoritative changes. The messages are sent to the servers listed in the zone's      name server records (except the master server identified in the start of authority MNAME      field), and to any servers listed in the `also-notify` option.

If notify is explicit, notifies are sent only to servers explicitly listed using        `also-notify`.

If you are only using Akamai’s name servers (and using BIND 9), it is recommended      you use the `explicit notify`      option. If you are running a few of your own authoritative name servers in addition to Akamai’s name servers, you might choose to set `notify` to yes depending on your      current mechanism of propagating zone files to the name servers.

These can also be specified in the zone stanza as follows:

```
zone “example.com” {
.....
notify yes;
also-notify {192.0.2.1;192.0.2.2;192.0.2.3;};
};
zone “example.com” {
NOTIFY EXPLICIT;
also-notify {192.0.2.1;192.0.2.2;192.0.2.3;};
.....
};
```