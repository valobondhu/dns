# [Bind Security: Transaction Signatures (TSIG) Configuration](https://www.cyberciti.biz/faq/unix-linux-bind-named-configuring-tsig/)

​						Author: Vivek Gite 					Last updated: January 27, 2009 					[17 comments](https://www.cyberciti.biz/faq/unix-linux-bind-named-configuring-tsig/#comments) 				

[![img](data:image/svg+xml;base64,PHN2ZyBoZWlnaHQ9IjU3IiB3aWR0aD0iMTI4IiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZlcnNpb249IjEuMSIvPg==)](https://www.cyberciti.biz/faq/category/bind-dns/)

Q. How do I configure BIND9 name serves with TSIG (Transaction SIGnature)  mechanism to secure server-to-server communication? How do I use secret  key transaction authentication for DNS (bind nameservers)?
 
 A. Transaction signatures (TSIG) is a mechanism used to secure DNS  messages and to provide secure server-to-server communication (usually  between master and slave server, but can be extended for dynamic updates as well). TSIG can protect the following type of transactions between  two DNS servers: 

ADVERTISEMENT

- Zone transfer
- Notify
- Dynamic updates
- Recursive query messages etc

TSIG is available for BIND v8.2 and above. TSIG uses shared secrets  and a one-way hash function to authenticate DNS messages. TSIG is easy  and lightweight for resolvers and named.

## How it works? Each name server adds a TSIG record the data section of a dns server-to-server queries and message. The TSIG record signs the DNS message, proving that the message’s  sender had a cryptographic key shared with the receiver and that the  message wasn’t modified after it left the sender. TSIG uses a one-way hash function to provide authentication and data integrity. 

Our sample setup:

- Master nameserver: ns1.theos.in – 202.54.1.2
- Slave nameserver: ns2.theos.in – 75.55.2.100
- BIND configuration is stored in */etc/bind/* directory.
- Zone data is stored in */etc/bind/named.conf file.* 

### How do I configure TSIG?

Type the following command on master nameserver (ns1.theos.in) to  create the shared keys, using the dnssec-keygen program, which creates  two files, both containing the key generated.
 `# dnssec-keygen -a HMAC-MD5 -b 128 -n HOST rndc-key`









----

# Error

# I'm trying to use TSIG to authenticate dynamic updates or zone transfers but the server is rejecting the TSIG - why?

- If you are sure that the keys are configured  correctly then this may be a clock skew problem. Check that the the  clocks on the client and server are properly synchronized (e.g., using  NTP).

Check your logs for errors. If you are running a recent version of  BIND, you may see error messages similar to these (reported by the  secondary zone server) below:

```shell
25-Jan-2013 13:09:08.048 zone 7.168.192.in-addr.arpa/IN/trusted:
refresh: failure trying master 192.168.7.27#53 (source 0.0.0.0#0):
clocks are unsynchronized
25-Jan-2013 13:09:23.053 zone myzone.example/IN/trusted: refresh:
failure trying master 192.168.7.27#53 (source 0.0.0.0#0): clocks are
unsynchronized
```