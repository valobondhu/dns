## What is a DNS record?

[DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) records (aka zone files) are instructions that live in authoritative [DNS servers](https://www.cloudflare.com/learning/dns/dns-server-types/) and provide information about a domain including what [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) is associated with that domain and how to handle requests for that  domain. These records consist of a series of text files written in what  is known as DNS syntax. DNS syntax is just a string of characters used  as commands that tell the DNS server what to do. All DNS records also  have a ‘[TTL](https://www.cloudflare.com/learning/cdn/glossary/time-to-live-ttl/)’, which stands for time-to-live, and indicates how often a DNS server will refresh that record.

You can think of a set of DNS records like a business listing on  Yelp. That listing will give you a bunch of useful information about a  business such as their location, hours, services offered, etc. All  domains are required to have at least a few essential DNS records for a  user to be able to access their website using a [domain name](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/), and there are several optional records that serve additional purposes.

## What are the most common types of DNS record?

- **A record** - The record that holds the IP address of a domain. [Learn more about the A record.](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/)
- **AAAA record** - The record that contains the IPv6 address for a domain (as opposed to A records, which list the IPv4 address). [Learn more about the AAAA record.](https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/)
- **CNAME record** - Forwards one domain or subdomain to another domain, does NOT provide an IP address. [Learn more about the CNAME record.](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/)
- **MX record** - Directs mail to an email server. [Learn more about the MX record.](https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/)
- **TXT record** - Lets an admin store text notes in the record. These records are often used for email security. [Learn more about the TXT record.](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/)
- **NS record** - Stores the name server for a DNS entry. [Learn more about the NS record.](https://www.cloudflare.com/learning/dns/dns-records/dns-ns-record/)
- **SOA record** - Stores admin information about a domain. [Learn more about the SOA record.](https://www.cloudflare.com/learning/dns/dns-records/dns-soa-record/)
- **SRV record** - Specifies a port for specific services. [Learn more about the SRV record.](https://www.cloudflare.com/learning/dns/dns-records/dns-srv-record/)
- **PTR record** - Provides a domain name in reverse-lookups. [Learn more about the PTR record.](https://www.cloudflare.com/learning/dns/dns-records/dns-ptr-record/)

## What are some of the less commonly used DNS records?

- **AFSDB record** - This record is used for clients of the Andrew File System (AFS) developed by Carnegie Melon. The AFSDB  record functions to find other AFS cells.
- **APL record** - The ‘address prefix list’ is an experiment record that specifies lists of address ranges.
- **CAA record** - This is the ‘certification authority authorization’ record, it allows domain owners state which certificate  authorities can issue certificates for that domain. If no CAA record  exists, then anyone can issue a certificate for the domain. These  records are also inherited by subdomains.
- **DNSKEY record** - The ‘[DNS Key Record](https://www.cloudflare.com/learning/dns/dns-records/dnskey-ds-records/)’ contains a [public key](https://www.cloudflare.com/learning/ssl/how-does-public-key-encryption-work/) used to verify [Domain Name System Security Extension (DNSSEC)](https://www.cloudflare.com/learning/dns/dns-security/) signatures.
- **CDNSKEY record** - This is a child copy of the DNSKEY record, meant to be transferred to a parent.
- **CERT record** - The ‘certificate record’ stores public key certificates.
- **DCHID record** - The ‘DHCP Identifier’ stores info  for the Dynamic Host Configuration Protocol (DHCP), a standardized  network protocol used on IP networks.
- **DNAME record** - The ‘delegation name’ record  creates a domain alias, just like CNAME, but this alias will redirect  all subdomains as well. For instance if the owner of ‘example.com’  bought the domain ‘website.net’ and gave it a DNAME record that points  to ‘example.com’, then that pointer would also extend to  ‘blog.website.net’ and any other subdomains.
- **HIP record** - This record uses ‘Host identity  protocol’, a way to separate the roles of an IP address; this record is  used most often in mobile computing.
- **IPSECKEY record** - The ‘IPSEC key’ record works with the [Internet Protocol Security (IPSEC)](https://www.cloudflare.com/learning/network-layer/what-is-ipsec/), an end-to-end security protocol framework and part of the Internet Protocol Suite [(TCP/IP)](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/).
- **LOC record** - The ‘location’ record contains geographical information for a domain in the form of longitude and latitude coordinates.
- **NAPTR record** - The ‘name authority pointer’ record can be combined with an [SRV record](https://www.cloudflare.com/learning/dns/dns-records/dns-srv-record/) to dynamically create URI’s to point to based on a regular expression.
- **NSEC record** - The ‘next secure record’ is part of DNSSEC, and it’s used to prove that a requested DNS resource record does not exist.
- **RRSIG record** - The ‘resource record signature’ is a record to store digital signatures used to authenticate records in  accordance with DNSSEC.
- **RP record** - This is the ‘responsible person’ record and it stores the email address of the person responsible for the domain.
- **SSHFP record** - This record stores the ‘SSH public key fingerprints’; SSH stands for Secure Shell and it’s a cryptographic networking protocol for secure communication over an unsecure network.

Cloudflare DNS supports a wide variety of DNS records. Learn about Cloudflare's [authoritative DNS service](https://www.cloudflare.com/dns/), or about [managing DNS records in Cloudflare](https://support.cloudflare.com/hc/en-us/articles/360019093151-Managing-DNS-records-in-Cloudflare).

---

## What is a DNS A record?

The "A" stands for "address" and this is the most fundamental type of [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) record: it indicates the [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) of a given [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/). For example, if you pull the DNS records of cloudflare.com, the A record currently returns an IP address of: 104.17.210.9.

A records only hold IPv4 addresses. If a website has an IPv6 address, it will instead use an ["AAAA" record](https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/).

Here is an example of an A record:

| example.com | record type: | value:    | TTL   |
| ----------- | ------------ | --------- | ----- |
| @           | A            | 192.0.2.1 | 14400 |

The "@" symbol in this example indicates that this is a record for the root domain, and the "14400" value is the [TTL (time to live)](https://www.cloudflare.com/learning/cdn/glossary/time-to-live-ttl/), listed in seconds. The default TTL for A records is 14,400 seconds.  This means that if an A record gets updated, it takes 240 minutes  (14,400 seconds) to take effect.

The vast majority of websites only have one A record, but it is  possible to have several. Some higher profile websites will have several different A records as part of a technique called [round robin load balancing](https://www.cloudflare.com/learning/dns/glossary/round-robin-dns/), which can distribute request traffic to one of several IP addresses, each hosting identical content.

## When are DNS A records used?

The most common usage of A records is IP address lookups: matching a  domain name (like "cloudflare.com") to an IPv4 address. This enables a  user's device to connect with and load a website, without the user  memorizing and typing in the actual IP address. The user's web browser  automatically carries this out by sending a query to a [DNS resolver](https://www.cloudflare.com/learning/dns/dns-server-types/).

DNS A records are also used for operating a Domain Name System-based  Blackhole List (DNSBL). DNSBLs can help mail servers identify and block  email messages from known spammer domains.

If you want to learn more about DNS A records, you can see the  original 1987 RFC where A records and several other DNS record types are defined [here](https://tools.ietf.org/html/rfc1035). To learn more about how the Domain Name System works, see [What is DNS?](https://www.cloudflare.com/learning/dns/what-is-dns/)

---

## What is a DNS AAAA record?

DNS AAAA records match a domain name to an IPv6 address. DNS AAAA records are exactly like [DNS A records](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/), except that they store a [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/)'s IPv6 address instead of its IPv4 address.

IPv6 is the latest version of the [Internet Protocol (IP)](https://www.cloudflare.com/learning/network-layer/internet-protocol/). One of the important differences between IPv6 and IPv4 is that IPv6  addresses are longer than IPv4 addresses. The Internet is running out of IPv4 addresses, just as there are only so many possible phone numbers  for a given area code. But IPv6 addresses offer exponentially more  permutations and thus far more possible [IP addresses](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/).

As an example of the difference between IPv4 and IPv6 addresses, Cloudflare offers a [public DNS resolver](https://www.cloudflare.com/learning/dns/what-is-1.1.1.1/) that anyone can use by setting their device's DNS to 1.1.1.1 and  1.0.0.1. These are the IPv4 addresses. The IPv6 addresses for this  service are 2606:4700:4700::1111 and 2606:4700:4700::1001.

#### DNS AAAA record example

Here is an example of an AAAA record:

| example.com | record type: | value:                                   | TTL   |
| ----------- | ------------ | ---------------------------------------- | ----- |
| @           | AAAA         | 2001:0db8:85a3:0000: 0000:8a2e:0370:7334 | 14400 |

## When are AAAA records used?

Like A records, AAAA records enable client devices to learn the IP  address for a domain name. The client device can then connect with and  load the website.

AAAA records are only used when a domain has an IPv6 address in  addition to an IPv4 address, and when the client device in question is  configured to use IPv6. While all domains have one or more IPv4  addresses and accompanying A records, not all domains have IPv6  addresses, and not all user devices are configured to use IPv6.

However, IPv6 is growing in adoption. This will likely continue to be the case because the number of available IPv4 addresses is rapidly  diminishing, often forcing multiple devices to share an IPv4 address. To combat this, Cloudflare began turning on IPv6 for all customers [in 2016](https://blog.cloudflare.com/98-percent-ipv6/).

It is probable that in the future, all domains will have AAAA records.

Read more about [DNS records](https://www.cloudflare.com/learning/dns/dns-records/).

---

## What is a DNS CNAME record?

The ‘canonical name’ (CNAME) record is used in lieu of an [A record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/), when a [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) or subdomain is an alias of another domain. All CNAME records must point to a domain, never to an [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/). Imagine a scavenger hunt where each clue points to another clue, and  the final clue points to the treasure. A domain with a CNAME record is  like a clue that can point you to another clue (another domain with a  CNAME record) or to the treasure (a domain with an A record).

For example, suppose blog.example.com has a CNAME record with a value of ‘example.com’ (without the ‘blog’). This means when a [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) server hits the [DNS records](https://www.cloudflare.com/learning/dns/dns-records/) for blog.example.com, it actually triggers another DNS lookup to  example.com, returning example.com’s IP address via its A record. In  this case we would say that example.com is the canonical name (or true  name) of blog.example.com.

Oftentimes, when sites have subdomains such as blog.example.com or  shop.example.com, those subdomains will have CNAME records that point to a root domain (example.com). This way if the IP address of the host  changes, only the DNS A record for the root domain needs to be updated  and all the CNAME records will follow along with whatever changes are  made to the root.

A frequent misconception is that a CNAME record must always resolve  to the same website as the domain it points to, but this is not the  case. The CNAME record only points the client to the same IP address as  the root domain. Once the client hits that IP address, the web server  will still handle the URL accordingly. So for instance, blog.example.com might have a CNAME that points to example.com, directing the client to  example.com’s IP address. But when the client actually connects to that  IP address, the web server will look at the URL, see that it is  blog.example.com, and deliver the blog page rather than the home page.

Example of a CNAME record:

| blog.example.com | record type: | value:                     | TTL   |
| ---------------- | ------------ | -------------------------- | ----- |
| @                | CNAME        | is an alias of example.com | 32600 |

In this example you can see that blog.example.com points to example.com, and assuming it is based on our [example A record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/) we know that it will eventually resolve to the IP address 192.0.2.1.

## Can a CNAME record point to another CNAME record?

Pointing a CNAME record to another CNAME record is inefficient  because it requires multiple DNS lookups before the domain can be loaded — which slows down the user experience — but it is possible. For  example, blog.example.com could have a CNAME record that pointed to  www.example.com's CNAME record, which then pointed to example.com's A  record.

CNAME for blog.example.com:

| blog.example.com | record type: | value:                         | TTL   |
| ---------------- | ------------ | ------------------------------ | ----- |
| @                | CNAME        | is an alias of www.example.com | 32600 |

Which points to a CNAME for www.example.com:

| www.example.com | record type: | value:                     | TTL   |
| --------------- | ------------ | -------------------------- | ----- |
| @               | CNAME        | is an alias of example.com | 32600 |

This configuration adds an extra step to the DNS lookup process and  should be avoided if possible. Instead, the CNAME records for both  blog.example.com and www.example.com should point directly to  example.com.

## What restrictions are there on using CNAME records?

MX and NS records cannot point to a CNAME record; they have to point to an A record (for IPv4) or an [AAAA record](https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/) (for IPv6). An MX record is a mail exchange record that directs email  to a mail server. An NS record is a 'name server' record and indicates  which DNS server is authoritative for that domain.

---

## What is a DNS MX record?

A DNS 'mail exchange' (MX) record directs email to a mail server. The MX record indicates how email messages should be routed in accordance  with the Simple Mail Transfer Protocol (SMTP, the standard protocol for  all email). Like [CNAME records](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/), an MX record must always point to another [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/).

Example of an MX record:

| example.com | record type: | priority: | value:                | TTL   |
| ----------- | ------------ | --------- | --------------------- | ----- |
| @           | MX           | 10        | mailhost1.example.com | 45000 |
| @           | MX           | 20        | mailhost2.example.com | 45000 |

The 'priority' numbers before the domains for these MX records  indicate preference; the lower 'priority' value is preferred. The server will always try mailhost1 first because 10 is lower than 20. In the  result of a message send failure, the server will default to mailhost2.

The email service could also configure this MX record so that both  servers have equal priority and receive an equal amount of mail:

| example.com | record type: | priority: | value:                | TTL   |
| ----------- | ------------ | --------- | --------------------- | ----- |
| @           | MX           | 10        | mailhost1.example.com | 45000 |
| @           | MX           | 10        | mailhost2.example.com | 45000 |

This configuration enables the email provider to equally [balance the load](https://www.cloudflare.com/load-balancing/) between the two servers.

## What is the process of querying an MX record?

Message transfer agent (MTA) software is responsible for querying MX  records. When a user sends an email, the MTA sends a DNS query to  identify the mail servers for the email recipients. The MTA establishes  an SMTP connection with those mail servers, starting with the  prioritized domains (in the first example above, mailhost1).

## What is a backup MX record?

A backup MX record is just an MX record for a mail server with a  higher 'priority' value (which means a lower priority), so that under  normal circumstances mail will go to the more prioritized servers. In  the first example above, mailhost2 would be the 'backup' server because  email traffic will be handled by mailhost1 as long as it is up and  running.

## Can MX records point to a CNAME?

A CNAME record is used for referencing a domain's alias instead of its actual name. CNAME records typically point to an [A record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/) (in IPv4) or AAAA record (in IPv6) for that domain. However, MX records have to point directly to a server's A record or [AAAA record](https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/). Pointing to a CNAME is forbidden by the [RFC documents that define](https://tools.ietf.org/html/rfc2181) how MX records function.

Learn more about the uses for [CNAME records](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/).

---

## What is a DNS TXT record?

The DNS ‘text’ (TXT) record lets a [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) administrator enter text into the [Domain Name System (DNS)](https://www.cloudflare.com/learning/dns/what-is-dns/). The TXT record was originally intended as a place for human-readable  notes. However, now it is also possible to put some machine-readable  data into TXT records. One domain can have many TXT records.

Example of a TXT record:

| example.com | record type: | value:                                            | TTL   |
| ----------- | ------------ | ------------------------------------------------- | ----- |
| @           | TXT          | This is an awesome domain! Definitely not spammy. | 32600 |

Today, two of the most important uses for DNS TXT records are email  spam prevention and domain ownership verification, although TXT records  were not designed for these uses originally.

## What kind of data can go in a TXT record?

The [original RFC](https://tools.ietf.org/html/rfc1035) only indicates that 'text strings' go in the 'value' field of a TXT  record. This could be any text that an administrator wants to associate  with their domain.

Most [DNS servers](https://www.cloudflare.com/learning/dns/dns-server-types/) will put a limit on how big TXT records can be and how many records  they can store, so administrators cannot use TXT records for large  amounts of data.

## What is the official format for storing data in a TXT record?

In 1993, the Internet Engineering Task Force (IETF) defined a format  for storing attributes and their corresponding values within the 'value' field of TXT records. The format was simply the attribute and the value contained within quotation marks (") and separated by an equal sign  (=), such as:

```
"attribute=value"
```

[RFC 1464](https://tools.ietf.org/html/rfc1464), the 1993 document that defines this format, includes these examples:

| host.widgets.com | record type: | value:         |
| ---------------- | ------------ | -------------- |
| @                | TXT          | "printer=lpr5" |



| sam.widgets.com | record type: | value:                        |
| --------------- | ------------ | ----------------------------- |
| @               | TXT          | "favorite drink=orange juice" |

However, this definition was considered experimental, and in practice it is not often adopted. Some DNS administrators follow their own  formats within TXT records, if they make use of TXT records at all. TXT  records may also be formatted in a specific way for certain uses  described below — for instance, DMARC policies have to be formatted in a standardized way.

## How do TXT records help prevent email spam?

Spammers often try to fake or forge the domains from which they send  their email messages. TXT records are a key component of several  different email authentication methods that help an email server  determine if a message is from a trusted source.

Common email authentication methods include Domain Keys Identified  Mail (DKIM), Sender Policy Framework (SPF), and Domain-based Message  Authentication, Reporting & Conformance (DMARC). By configuring  these records, domain operators can make it more difficult for spammers  to [spoof their domains](https://www.cloudflare.com/learning/ssl/what-is-domain-spoofing/) and can track attempts to do so.

[SPF records](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/): SPF TXT records list all the servers that are authorized to send email messages from a domain.

[DKIM records](https://www.cloudflare.com/learning/dns/dns-records/dns-dkim-record/): DKIM works by digitally signing each email using a public-private key  pair. This helps verify that the email is actually from the domain it  claims to be from. The public key is hosted in a TXT record associated  with the domain. (Learn more about [public key encryption](https://www.cloudflare.com/learning/ssl/how-does-public-key-encryption-work/).)

[DMARC records](https://www.cloudflare.com/learning/dns/dns-records/dns-dmarc-record/): A DMARC TXT record references the domain's SPF and DKIM policies. It  should be stored under the title _dmarc.example.com. with 'example.com'  replaced with the actual domain name. The 'value' of the record is the  domain's DMARC policy (a guide to creating one can be found [here](https://dmarcguide.globalcyberalliance.org/#/dmarc/)).

## How do TXT records help verify domain ownership?

While domain ownership verification was not initially a feature of  TXT records, this approach has been adopted by some webmaster tools and [cloud](https://www.cloudflare.com/learning/cloud/what-is-the-cloud/) providers.

By uploading a new TXT record with specific information included, or  editing the current TXT record, an administrator can prove they control  that domain. The tool or cloud provider can check the TXT record and see that it has been changed as requested. This is somewhat like when a  user confirms their email address by opening and clicking a link sent to that email, proving they own the address.

Learn more about the different types of [DNS records](https://www.cloudflare.com/learning/dns/dns-records/).

----

## What is a DNS NS record?

NS stands for ‘nameserver,’ and the nameserver record indicates which [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) server is authoritative for that [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) (i.e. which server contains the actual [DNS records](https://www.cloudflare.com/learning/dns/dns-records/)). Basically, NS records tell the Internet where to go to find out a domain's [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/). A domain often has multiple NS records which can indicate primary and  backup nameservers for that domain. Without properly configured NS  records, users will be unable to load a website or application.

Here is an example of an NS record:

| example.com | record type: | value:                | TTL   |
| ----------- | ------------ | --------------------- | ----- |
| @           | NS           | ns1.exampleserver.com | 21600 |

Note that NS records can never point to a [canonical name (CNAME) record](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/).

## What is a nameserver?

A nameserver is a type of DNS server. It is the server that stores all DNS records for a domain, including [A records](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/), [MX records](https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/), or CNAME records.

Almost all domains rely on multiple nameservers to increase  reliability: if one nameserver goes down or is unavailable, DNS queries  can go to another one. Typically there is one primary nameserver and  several secondary nameservers, which store exact copies of the DNS  records in the primary server. Updating the primary nameserver will  trigger an update of the secondary nameservers as well.

When multiple nameservers are used (as in most cases), NS records should list more than one server. Learn more about [DNS servers](https://www.cloudflare.com/learning/dns/dns-server-types/).

## When should NS records be updated or changed?

Domain administrators should update their NS records when they need  to change their domain's nameservers. For instance, some cloud providers provide nameservers and require their customers to point to them.

Admins may also wish to update their NS records if they want a  subdomain to use different nameservers. In the example above, the  nameserver for example.com is ns1.exampleserver.com. If the example.com  admin wanted blog.example.com to resolve via a ns2.exampleserver.com  instead, they could set this up by updating the NS record.

When NS records are updated, it may take several hours for the changes to be replicated throughout the DNS.

---

## What is a DNS SOA record?

The [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) ‘start of authority’ (SOA) record stores important information about a [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) or [zone](https://www.cloudflare.com/learning/dns/glossary/dns-zone/) such as the email address of the administrator, when the domain was  last updated, and how long the server should wait between refreshes.

All DNS zones need an SOA record in order to conform to IETF standards. SOA records are also important for zone transfers.

Example of an SOA record:

| name        | example.com          |
| ----------- | -------------------- |
| record type | SOA                  |
| MNAME       | ns.primaryserver.com |
| RNAME       | admin.example.com    |
| SERIAL      | 1111111111           |
| REFRESH     | 86400                |
| RETRY       | 7200                 |
| EXPIRE      | 4000000              |
| TTL         | 11200                |

The 'RNAME' value here represents the administrator's email address,  which can be confusing because it is missing the ‘@’ sign, but in an SOA record admin.example.com is the equivalent of admin@example.com.

## What is a zone serial number?

In the DNS, a 'zone' is an area of control over namespace. A zone can include a single domain name, one domain and many subdomains, or many  domain names. In some cases, 'zone' is essentially equivalent with  'domain,' but this is not always true.

A zone serial number is a version number for the SOA record. In the  example above, the serial number is listed next to 'SERIAL.' When the  serial number changes in a zone file, this alerts secondary nameservers  that they should update their copies of the zone file via a zone  transfer.

## What are the other parts of an SOA record?

- MNAME: This is the name of the primary nameserver for the zone. Secondary servers that maintain duplicates of the zone's [DNS records](https://www.cloudflare.com/learning/dns/dns-records/)  receive updates to the zone from this primary server.
- REFRESH: The length of time (in seconds) secondary servers should  wait before asking primary servers for the SOA record to see if it has  been updated.
- RETRY: The length of time a server should wait for asking an unresponsive primary nameserver for an update again.
- EXPIRE: If a secondary server does not get a response from the  primary server for this amount of time, it should stop responding to  queries for the zone.

## What is a zone transfer?

A DNS zone transfer is the process of sending DNS record data from a [primary](https://www.cloudflare.com/learning/dns/glossary/primary-secondary-dns/) nameserver to a secondary nameserver. The SOA record is transferred  first. The serial number tells the secondary server if its version needs to be updated. Zone transfers take place over the [TCP protocol](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/).

Learn more about various [DNS records](https://www.cloudflare.com/learning/dns/dns-records/).

---

## What is a DNS SRV record?

The [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) "service" (SRV) record specifies a host and port for specific services such as [voice over IP (VoIP)](https://www.cloudflare.com/learning/video/what-is-voip/), instant messaging, and so on. Most other [DNS records](https://www.cloudflare.com/learning/dns/dns-records/) only specify a server or an [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/), but SRV records include a port at that IP address as well. Some [Internet protocols](https://www.cloudflare.com/learning/network-layer/internet-protocol/) require the use of SRV records in order to function.

## What is a port?

In networking, [ports](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/) are virtual places that designate what processes network traffic goes  to within a computer. Ports allow computers to easily differentiate  between different kinds of traffic: VoIP streams go to a different port  than email messages, for instance, even though both reach a computer  over the same Internet connection. Much like IP addresses, all ports are assigned a number.

Certain Internet protocols, such as IMAP, SIP, and XMPP, need to  connect to a specific port in addition to connecting with a specific  server. SRV records are how a port can be specified within the DNS.

## What goes in an SRV record?

An SRV record contains the following information. Here, we list example values for each field.

| service  | XMPP               |
| -------- | ------------------ |
| proto*   | TCP                |
| name**   | example.com        |
| TTL      | 86400              |
| class    | IN                 |
| type     | SRV                |
| priority | 10                 |
| weight   | 5                  |
| port     | 5223               |
| target   | server.example.com |

**Short for "protocol," as in transport protocol.  
  **[Domain name](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/).*

However, SRV records are actually formatted in this way:

```
_service._proto.name. TTL class type of record priority weight port target.
```

So our example SRV record would actually look like:

```
_xmpp._tcp.example.com. 86400 IN SRV 10 5 5223 server.example.com.
```

In the above example, "_xmpp" indicates the type of service (the XMPP protocol) and "_tcp" indicates the [TCP](https://www.cloudflare.com/learning/ddos/glossary/tcp-ip/) transport protocol, while "example.com" is the host, or the domain  name. "Server.example.com" is the target server and "5223" indicates the port within that server.

SRV records must point to an [A record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/) (in IPv4) or an [AAAA record](https://www.cloudflare.com/learning/dns/dns-records/dns-aaaa-record/) (in IPv6). The server name they list cannot be a [CNAME](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/). So "server.example.com" must lead directly to an A or AAAA record under that name.

## What is the difference between priority and weight in SRV records?

SRV records indicate the "priority" and "weight" of the various  servers they list. The "priority" value in an SRV record enables  administrators to prioritize one server that supports the given service  over another. A server with a lower priority value will receive more  traffic than other servers. However, the "weight" value is similar: a  server with a higher weight will receive more traffic than other servers with the same priority.

The main difference between them is that priority is looked at first. If there are three servers, Server A, Server B, and Server C, and they  have respective priorities of 10, 20, and 30, then their "weight" does  not matter. The service will always query Server A first.

But suppose Servers A, B, and C all have a priority of 10 — how will a service choose between them? This is where weight becomes a factor: if  Server A has a "weight" value of 5 and Servers B and C have a "weight"  value of 3 and 2, Server A will receive the most traffic, Server B will  receive the second-most traffic, and Server C the third-most.

---

## What is a DNS PTR record?

The [Domain Name System, or DNS](https://www.cloudflare.com/learning/dns/what-is-dns/), correlates [domain names](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) with [IP addresses](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/). A DNS pointer record (PTR for short) provides the domain name  associated with an IP address. A DNS PTR record is exactly the opposite  of the ['A' record](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/), which provides the IP address associated with a domain name.

DNS PTR records are used in [reverse DNS lookups](https://www.cloudflare.com/learning/dns/glossary/reverse-dns/). When a user attempts to reach a domain name in their browser, a DNS  lookup occurs, matching the domain name to the IP address. A reverse DNS lookup is the opposite of this process: it is a query that starts with  the IP address and looks up the domain name.

## How are DNS PTR records stored?

*In IPv4:*

While DNS A records are stored under the given domain name, DNS PTR  records are stored under the IP address — reversed, and with  ".in-addr.arpa" added. For example, the PTR record for the IP address  192.0.2.255 would be stored under "255.2.0.192.in-addr.arpa".

"in-addr.arpa" has to be added because PTR records are stored within  the .arpa top-level domain in the DNS. .arpa is a domain used mostly for managing network infrastructure, and it was [the first](https://tools.ietf.org/html/rfc881) top-level domain name defined for the Internet. (The name "arpa" dates  back to the earliest days of the Internet: it takes its name from the  Advanced Research Projects Agency (ARPA), which created ARPANET, an  important precursor to the Internet.)

in-addr.arpa is the namespace within .arpa for reverse DNS lookups in IPv4.

*In IPv6:*

IPv6 addresses are constructed differently from IPv4 addresses, and  IPv6 PTR records exist in a different namespace within .arpa. IPv6 PTR  records are stored under the IPv6 address, reversed and converted into  four-bit sections (as opposed to 8-bit sections, as in IPv4), plus  ".ip6.arpa".

## What are some of the main uses for PTR records?

PTR records are used in reverse DNS lookups; common uses for reverse DNS include:

**Anti-spam:** Some email anti-spam filters use reverse  DNS to check the domain names of email addresses and see if the  associated IP addresses are likely to be used by legitimate email  servers.

**Troubleshooting email delivery issues:** Because  anti-spam filters perform these checks, email delivery problems can  result from a misconfigured or missing PTR record. If a domain has no  PTR record, or if the PTR record contains the wrong domain, email  services may block all emails from that domain.

**Logging:** System logs typically record only IP  addresses; a reverse DNS lookup can convert these into domain names for  logs that are more human-readable.

---

## What are the DNSKEY and DS records?

The [Domain Name System (DNS)](https://www.cloudflare.com/learning/dns/what-is-dns/) is the phonebook of the Internet, but it was not designed with security in mind. For this reason, an optional security protocol called [DNSSEC](https://www.cloudflare.com/dns/dnssec/how-dnssec-works/) was created so that owners of web properties could better secure their  applications. DNSSEC increases security by adding cryptographic  signatures to DNS records; these signatures can be checked to verify  that a record came from the correct DNS server.

For the implementation of these cryptographic signatures, two new [DNS record types](https://www.cloudflare.com/learning/dns/dns-records/) were created: DNSKEY and DS. The DNSKEY record contains a public  signing key, and the DS record contains a hash* of a DNSKEY record.

Each DNSSEC zone is assigned a set of [zone](https://www.cloudflare.com/learning/dns/glossary/dns-zone/) signing keys (ZSK). This set includes a private and public ZSK. The  private ZSK is used to sign the DNS records in that zone, and the public ZSK is used to verify the private one.

The public ZSK is published in a DNSSEC record, which is how it is  provided to a DNSSEC resolver; the resolver will use the public ZSK to  ensure the records from that zone are authentic. As an added layer of  security, DNSSEC zones contain a second DNSKEY record containing a key  signing key (KSK), which verifies the authenticity of the public ZSK.

The DS record is used to verify the authenticity of child zones** of  DNSSEC zones. The DS key record on a parent zone contains a hash of the  KSK in a child zone. A DNSSEC resolver can therefore verify the  authenticity of the child zone by hashing its KSK record, and comparing  that to what is in the parent zone's DS record.

**A cryptographic hash is a one-way scrambling of alphanumeric  input; hashes are often used for storing sensitive information like  passwords on servers. For example, a hash of the input ‘cantguessthis’  is 18fe9934cf77a759eb2471f2b304708a. Every time ‘cantguessthis’ is put  through the hashing function, it outputs the same hash. But there is no  way to get the original input using just the hash. The hash on its own  is essentially useless.*

***A child zone is a delegated subdomain of another zone. For  instance, a URL of example.com could have child zones with domains like  blog.example.com and mail.example.com.*

----

## What is a DNS SPF record?

A sender policy framework (SPF) record is a type of [DNS TXT record](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/) that lists all the servers authorized to send emails from a particular domain.

A DNS TXT (“text”) record lets a [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) administrator enter arbitrary text into the [Domain Name System (DNS)](https://www.cloudflare.com/learning/dns/what-is-dns/). TXT records were initially created for the purpose of including  important notices regarding the domain, but have since evolved to serve  other purposes. 

SPF records were originally created because the standard protocol used for email — the Simple Mail Transfer Protocol ([SMTP](https://datatracker.ietf.org/doc/html/rfc5321)) — does not inherently authenticate the “from” address in an email. This means that without SPF or other authentication records, an attacker can easily impersonate a sender and trick the recipient into taking action  or sharing information they otherwise would not. 

Think of SPF records like a guest list that is managed by a door  attendant. If someone is not on the list, the door attendant will not  let them in. Similarly, if an SPF record does not have a sender’s IP  address or domain on its list, the receiving server (door attendant)  will either not deliver those emails or mark them as spam.

SPF records are just one of many DNS-based mechanisms that can help  email servers confirm whether an email comes from a trusted source.  Domain-based Message Authentication Reporting and Conformance ([DMARC](https://www.cloudflare.com/learning/dns/dns-records/dns-dmarc-record/)) and DomainKeys Identified Mail ([DKIM](https://www.cloudflare.com/learning/dns/dns-records/dns-dkim-record/)) are two other mechanisms used for email authentication. 

It is worth noting that, at one point, SPF records had a dedicated DNS record type. The dedicated record type has since been [deprecated](https://datatracker.ietf.org/doc/html/rfc7208#section-3.1) and only TXT records are to be used.  

## How does a mail server check an SPF record?

Mail servers go through a relatively simple process when checking an SPF record:

- Server One sends an email. Its IP address is *192.0.2.0* and the return-path the email uses is email@returnpath.com. (A  return-path address is different from the “from” address and is used  specifically for collecting and processing bounced messages.)
- The mail server that is receiving the message (Server Two) takes the return-path domain and searches for its SPF record.
- If Server Two finds an SPF record for the return-path’s domain, it  searches the SPF record for Server One’s IP address in its list of  authorized senders. If the IP address is listed in the SPF record, the  SPF check passes and the email will go through. If the IP address is not listed in the SPF record, the SPF check fails. In this case, the email  will be rejected or marked as spam. 

## What does an SPF record look like?

SPF records must follow certain standards in order for the server to  understand how to interpret its contents. Here is an example of the core components of an SPF record:

```
v=spf1 ip4=192.0.2.0 ip4=192.0.2.1 include:examplesender.email -all
```

This example lets the server know what type of record this is, states the approved IP addresses and a third-party for this domain, and tells  the server what to do with non-compliant emails. Let’s break down how  the individual components accomplish this: 

- `v=spf1` tells the server that this contains an SPF record. Every SPF record must begin with this string.

- Then comes the “guest list” portion of the SPF record or the list of authorized IP addresses. In this example, the SPF record is telling the server that `ip4=192.0.2.0` and `ip4=192.0.2.1` are authorized to send emails on behalf of the domain. 

- `include:examplesender.net` is an example of the include  tag, which tells the server what third-party organizations are  authorized to send emails on behalf of the domain. This tag signals that the content of the SPF record for the included domain  (examplesender.net) should be checked and the IP addresses it contains  should also be considered authorized. Multiple domains can be included  within an SPF record but this tag will only work for valid domains. 

- Finally, `-all` tells the server that addresses not listed in the SPF record are not authorized to send emails and should be rejected. 

- - Alternative options here include `~all`, which states that unlisted emails will be marked as insecure or spam but still accepted, and, less commonly, `+all`, which signifies that any server can send emails on behalf of your domain. 

While the example used in this article is fairly straightforward, SPF records can certainly be more complex. Here are just a few things to  keep in mind to ensure SPF records are valid:

- There cannot be more than one SPF record associated with a domain. 
- The record must end with the `all` component or include a `redirect:` component (which indicates that the SPF record is hosted by another domain). 
- An SPF record cannot contain uppercase characters.

Check out the [official SPF record documentation](https://datatracker.ietf.org/doc/html/rfc7208) for more information.

## Why are SPF records used?

There are many reasons domain operators use SPF records: 

- **Preventing attacks:** If emails are not authenticated, companies and email recipients are at risk for [phishing](https://www.cloudflare.com/learning/access-management/phishing-attack/) attacks, spam emails, and email spoofing. With SPF records, it is  harder for attackers to imitate a domain, reducing the likelihood of  these attacks. 
- **Improving email deliverability:** Domains without a  published SPF record may have their emails bounce or be marked as spam.  Over time, bounced emails or emails marked as spam can hurt a domain’s  ability to reach their audience’s inboxes, compromising efforts to  communicate with customers, employees, and other entities.
- **DMARC compliance:** DMARC is an email validation  system that helps ensure that emails are sent only by authorized users.  DMARC policies dictate what servers should do with emails that fail SPF  and DKIM checks. Based on the DMARC policy instructions, those emails  will either be marked as spam, rejected, or delivered as normal. Domain  administrators receive reports about their email activity that help them make adjustments to their policy. 

The Cloudflare Email Security DNS Wizard makes it simple to set up  the correct DNS TXT records and block spammers from using a domain. [Read more about the Wizard here](https://blog.cloudflare.com/tackling-email-spoofing/).

Learn more about DNS records for email:

- [DMARC record](https://www.cloudflare.com/learning/dns/dns-records/dns-dmarc-record/)

- [DNS DKIM record](https://www.cloudflare.com/learning/dns/dns-records/dns-dkim-record/)

- [DNS MX record](https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/)

- [DNS TXT record](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/)

  ---

  ## What is DKIM?

  DomainKeys Identified Mail (DKIM) is a method of email authentication that helps prevent spammers and other malicious parties from [impersonating a legitimate domain](https://www.cloudflare.com/learning/ssl/what-is-domain-spoofing/).

  All email addresses have a [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) — the part of the address after the "@" symbol. Spammers and attackers  may try to impersonate a domain when sending emails to carry out [phishing attacks](https://www.cloudflare.com/learning/access-management/phishing-attack/) or other scams.

  Suppose Chuck wants to trick Alice, who works for example.com, into  sending him confidential company information. He could send her an email that seems to be coming from "bob@example.com" to fool her into  thinking he also works for example.com.

  DKIM, along with [Sender Policy Framework (SPF)](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/) and [Domain-based Message Authentication Reporting and Conformance (DMARC)](https://www.cloudflare.com/learning/dns/dns-records/dns-dmarc-record/), makes it much more difficult for attackers to impersonate domains in  this way. Emails that do not pass DKIM and SPF get marked as "spam" or  are not delivered by email servers. If example.com has DKIM, SPF, and  DMARC set up for their domain, then Alice will probably never even see  Chuck's malicious email because it will either go to her spam folder or  be rejected by the email server altogether.

  ## How does DKIM work?

  There are two main aspects of DKIM: the DKIM record, which is stored in the [Domain Name System (DNS)](https://www.cloudflare.com/learning/dns/what-is-dns/) records for the domain, and the DKIM header, which is attached to all emails from the domain.

  DKIM uses digital signature schemes based on [public key cryptography](https://www.cloudflare.com/learning/ssl/how-does-public-key-encryption-work/) to authenticate where an email came from, that it actually came from a  server that sends emails from that domain. A pair of cryptographic keys  are used: a private key for the sender to sign messages, and a public  key for the receiver to verify signatures. A receiver cannot use the  public key to sign messages, and vice versa.

  The email provider generates the public key and private key. They  give the public key to the domain owner, who stores the public key in a  publicly available DNS record — the DKIM record.

  All emails sent from that domain include a DKIM header, which  contains a section of data that is signed with the private key: this is  called a "digital signature." An email server can check the DKIM DNS  record, obtain the public key, and use the public key to verify the  digital signature.

  This process also ensures that the email has not been changed in  transit. The digital signature will not be verified if email headers or  the email body have been altered — like a tamper-proof seal on a  canister of medicine.

  ## What is a DKIM record?

  A DKIM record stores the DKIM public key — a randomized string of  characters that is used to verify anything signed with the private key.  Email servers query the domain's DNS records to see the DKIM record and  view the public key.

  A DKIM record is really a [DNS TXT ("text") record](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/) . TXT records can be used to store any text that a domain administrator wants to associate with their domain. DKIM is one of many uses for this type of DNS record.

  Here is an example of a DKIM DNS TXT record:

  | Name                               | Type  | Content                                                      | TTL    |
  | ---------------------------------- | ----- | ------------------------------------------------------------ | ------ |
  | `big-email._domainkey.example.com` | `TXT` | `v=DKIM1; p=76E629F05F709EF665853333EEC3F5ADE69A2362BECE40658267AB2FC3CB6CBE` | `6000` |

  #### Name

  Unlike most DNS TXT records, DKIM records are stored under a  specialized name, not just the name of the domain. DKIM record names  follow this format:

  ```
  [selector]._domainkey.[domain]
  ```

  The `selector` is a specialized value issued by the email  service provider used by the domain. It is included in the DKIM header  to enable an email server to perform the required DKIM lookup in the  DNS. The `domain` is the email domain name. `._domainkey.` is included in all DKIM record names.

  To look up the DKIM record, email servers use the DKIM selector  provided by the email service provider, not just the domain name.  Suppose example.com uses Big Email as their email service provider, and  suppose Big Email uses the DKIM selector `big-email`. Most of example.com's DNS records would be named `example.com`, but their DKIM DNS record would be under the name `big-email._domainkey.example.com`, which is listed in the example above.

  #### Content

  This is the part of the DKIM DNS record that lists the public key. In the example above, `v=DKIM1` indicates that this TXT record should be interpreted as DKIM, and the public key is everything after `p=`.

  #### Record type and TTL

  These are standard fields in DNS records. `TXT` indicates  that this is a DNS TXT record. "TTL" stands for time to live (measured  in seconds), and it indicates how long this record should be considered  valid before it needs to be refreshed. DKIM records generally have a TTL of several minutes.

  ## What is a DKIM header? How does a DKIM signature work?

  The sending email server creates its digital signature using email  headers, the email body (actually a hash of the email body — read more  below), and its private key. This digital signature is attached to the  email as part of the DKIM header.

  The DKIM header is one of many headers that are attached to an email. Most email applications do not show the header when displaying an email unless the user selects certain options. In Gmail, for example, users  can view an email's header by clicking on the three vertical dots in the upper right of the email, then clicking "Show original."

  Here is an example of a DKIM header:

  ```
  v=1; a=rsa-sha256; 
          d=example.com; s=big-email;
          h=from:to:subject;
        bh=uMixy0BsCqhbru4fqPZQdeZY5Pq865sNAnOAxNgUS0s=;
    b=LiIvJeRyqMo0gngiCygwpiKphJjYezb5kXBKCNj8DqRVcCk7obK6OUg4o+EufEbB
  tRYQfQhgIkx5m70IqA6dP+DBZUcsJyS9C+vm2xRK7qyHi2hUFpYS5pkeiNVoQk/Wk4w
  ZG4tu/g+OA49mS7VX+64FXr79MPwOMRRmJ3lNwJU=
  ```

  - `v=` shows which version of DKIM is in use.
  - `d=` is the domain name of the sender.
  - `s=` is the selector that the receiving server should use to look up the DNS record.
  - `h=` lists the header fields that are used to create the digital signature, or `b`. In this case, the from, to, and subject headers are used. If Bob sent  an email to Alice using the example.com domain and the subject line was  "Recipe for cheesecake," the content used here would be  "bob@example.com" + "alice@example.com" + "Recipe for cheesecake". (This content would also be canonicalized — put into a standardized format.)
  - `bh=` is the hash of the email body. A hash is the  result of a specialized mathematical function called a hash function.  This is included so that the receiving email server can compute the  signature before the entire email body loads, since email bodies can be  any length and loading it may take a long time in some cases.
  - `a=` is the algorithm used to compute the digital signature, or `b`, as well as generate the hash of the email body, or `bh`. In this example, RSA-SHA-256 is in use (RSA using SHA-256 as the hash  function for the digital signature, and SHA-256 for the body hash).
  - `b=` is the **digital signature**, generated from `h` and `bh` and signed with the private key.

  The digital signature (`b=`) allows the receiving server  to 1. authenticate the sending server and 2. ensure integrity — that the email has not been tampered with.

  The receiving server does this by taking the same content that is listed in `h=` plus the body hash (`bh=`) and using the public key from the DKIM record to check if the digital  signature is valid. If the correct private key was used and if the  content (headers and body) has not been altered, the email passes the  DKIM check.

  ## How does DKIM relate to DMARC?

  DMARC is an email authentication method built on top of DKIM and SPF. DMARC describes what to do with an email that fails SPF and DKIM.  Together, SPF, DKIM, and DMARC help prevent email spam and email  spoofing. Like DKIM records, DMARC policies are stored as DNS TXT  records.

  Cloudflare offers an [Email Security DNS Wizard](https://blog.cloudflare.com/tackling-email-spoofing/) that allows users to quickly set up email authentication DNS TXT  records, helping domain administrators stop malicious parties from  impersonating their domain.

  Learn more about DNS records for email:

  - [DNS SPF record](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/)
  - [DNS DMARC record](https://www.cloudflare.com/learning/dns/dns-records/dns-dmarc-record/)
  - [DNS MX record](https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/)
  - [DNS TXT record](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/)

  To learn more about DKIM, see [RFC 6376](https://datatracker.ietf.org/doc/html/rfc6376/).

  ---

  ## What is DMARC?

  Domain-based Message Authentication Reporting and Conformance (DMARC) is a method of authenticating email messages. A DMARC policy tells a  receiving email server what to do after checking a domain's [Sender Policy Framework (SPF)](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/) and [DomainKeys Identified Mail (DKIM)](https://www.cloudflare.com/learning/dns/dns-records/dns-dkim-record/) records, which are additional email authentication methods.

  DMARC and other email authentication methods are necessary in order to prevent email [spoofing](https://www.cloudflare.com/learning/ssl/what-is-domain-spoofing/). Every email address has a [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/), which is the portion of the address that comes after the "@" symbol.  Malicious parties and spammers sometimes try to send emails from a  domain that they are not authorized to use — like someone writing the  wrong return address on a letter. They may do this to try to trick users (as in a [phishing attack](https://www.cloudflare.com/learning/access-management/phishing-attack/)), among other reasons.

  Together, DMARC, DKIM, and SPF function like a background check on  email senders, to make sure they really are who they say they are.

  For example, imagine a spammer sends an email from the address  "trustworthy@example.com," despite the fact that they are not authorized to send email from the "example.com" domain. The spammer would do this  by replacing the "From" header in the email with  "trustworthy@example.com" — they would not send an email from the actual example.com email server. Email servers that receive this email can use DMARC, SPF, and DKIM to discover that this is an unauthorized email,  and they can mark the email message as spam or refuse to deliver it.

  ## What is a DMARC policy?

  A DMARC policy determines what happens to an email after it is  checked against SPF and DKIM records. An email either passes or fails  SPF and DKIM. The DMARC policy determines if failure results in the  email being marked as spam, getting blocked, or being delivered to its  intended recipient. (Email servers may still mark emails as spam if  there is no DMARC record, but DMARC provides clearer instructions on  when to do so.)

  Example.com's domain policy could be:

  "If an email fails the DKIM and SPF tests, mark it as spam."

  These policies are not recorded as human-readable sentences, but  rather as machine-readable commands so that email services can interpret them automatically. That DMARC policy would actually look like:

  ```
  v=DMARC1; p=quarantine; adkim=s; aspf=s;
  ```

  What does this mean?

  - `v=DMARC1` indicates that this TXT record contains a DMARC policy and should be interpreted as such by email servers.
  - `p=quarantine` indicates that email servers should  "quarantine" emails that fail DKIM and SPF — considering them to be  potentially spam. Other possible settings for this include `p=none`, which allows emails that fail to still go through, and `p=reject`, which instructs email servers to block emails that fail.
  - `adkim=s` means that DKIM checks are "strict." This can also be set to "relaxed" by changing the `s` to an `r`, like `adkim=r`.
  - `aspf=s` is the same as `adkim=s`, but for SPF.
  - Note that `aspf` and `adkim` are optional settings. The `p=` attribute is what indicates what email servers should do with emails that fail SPF and DKIM.

  If the example.com administrator wanted to make this policy even  stricter and signal more strongly to email servers to consider  unauthorized messages spam, they would adjust the "p=" attribute like  so:

  ```
  v=DMARC1; p=reject; adkim=s; aspf=s;
  ```

  Essentially, this says: "If an email fails the DKIM and SPF tests, do not deliver it."

  ## What is a DMARC report?

  DMARC policies can contain instructions to send reports about emails  that pass or fail DKIM or SPF. Typically, administrators set up reports  to be sent to a third-party service that boils them down to a more  digestible form, so administrators are not overwhelmed with information. DMARC reports are extremely important because they give administrators  the information they need to decide how to adjust their DMARC policies — for instance, if their legitimate emails are failing SPF and DKIM, or  if a spammer is trying to send illegitimate emails.

  The example.com administrator would add the `rua` part of  this policy to send their DMARC reports to a third-party service (with  an email address of "example@third-party-example.com"):

  ```
  v=DMARC1; p=reject; adkim=s; aspf=s; rua=mailto:example@third-party-example.com;
  ```

  ## What is a DMARC record?

  A DMARC record stores a domain's DMARC policy. DMARC records are stored in the [Domain Name System (DNS)](https://www.cloudflare.com/learning/dns/what-is-dns/) as [DNS TXT records](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/). A DNS TXT record can contain almost any text a domain administrator  wants to associate with their domain. One of the ways DNS TXT records  are used is to store DMARC policies.

  (Note that a DMARC record is a DNS TXT record that contains a DMARC policy, not a specialized type of [DNS record](https://www.cloudflare.com/learning/dns/dns-records/).)

  Example.com's DMARC policy might look like this:

  | Name          | Type  | Content                                                      | TTL     |
  | ------------- | ----- | ------------------------------------------------------------ | ------- |
  | `example.com` | `TXT` | `v=DMARC1; p=quarantine; adkim=r; aspf=r; rua=mailto:example@third-party-example.com;` | `32600` |

  Within this TXT record, the DMARC policy is contained in the "Content" field.

  ## What about domains that do not send emails?

  Domains that do not send emails should still have a DMARC record in  order to prevent spammers from using the domain. The DMARC record should have a DMARC policy that rejects all emails that fail SPF and DKIM —  which should be all emails sent by that domain.

  In other words, if example.com was not configured to send email, all emails would fail SPF and DKIM and be rejected.

  The Cloudflare Email Security DNS Wizard makes it simple to set up  the correct DNS TXT records and block spammers from using a domain. [Read about it here](https://blog.cloudflare.com/tackling-email-spoofing/).

  Learn more about DNS records for email:

  - [DNS SPF record](https://www.cloudflare.com/learning/dns/dns-records/dns-spf-record/)
  - [DNS DKIM record](https://www.cloudflare.com/learning/dns/dns-records/dns-dkim-record/)
  - [DNS MX record](https://www.cloudflare.com/learning/dns/dns-records/dns-mx-record/)
  - [DNS TXT record](https://www.cloudflare.com/learning/dns/dns-records/dns-txt-record/)

  DMARC is described further in [RFC 7489](https://datatracker.ietf.org/doc/html/rfc7489).