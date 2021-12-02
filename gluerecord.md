In normal DNS resolution, when a resolver attempts to resolve a  domain name, it first queries the root, which provides the top-level  domain. Next, it queries the top-level domain servers, which provide the domain’s authoritative nameservers. Finally, it queries the  authoritative nameservers for the domain to resolve the domain name. If  the nameservers for a domain exist inside the domain itself, a glue  record is needed to resolve the domain name.

## What is a Glue Record?

Glue records are DNS records created at the domain’s registrar. The  record provides a complete answer when the TLD nameserver returns a  reference for an authoritative nameserver for a domain. For example, the domain name “example.net” has nameservers “ns1.example.net”  “ns2.example.net”. To resolve the domain name, the DNS would query in  order: root, TLD nameserver, and authoritative nameserver. However, by  having the authoritative nameservers inside the domain itself, these  nameservers cannot be found without outside assistance. This is called a ‘circular reference’. Creating a glue record, an A record served by the TLD nameserver, avoids circular references and allows for both DNS name resolution and listing the nameservers inside the domain itself.

Glue records can only be created at the domain registrar as the  registrar controls the DNS settings for a given domain’s delegation.  Every nameserver on the internet has its own glue record created by the  domain’s owner. For example, NS1’s nameservers at ‘dnsx.p0y.nsone.net’  all have glue records created at our registrar.

## When do I need a Glue Record?

You need to create a glue record when you host your own authoritative servers. If a 3rd party, such as a [managed DNS provider](https://ns1.com/products/managed-dns) hosts your authoritative nameservers, then the provider takes care of setting up the glue record.

When you host your own authoritative servers, you need to set up the glue records with the domain registrar.

## Dedicated DNS needs Glue Records

One instance of this scenario is [Dedicated DNS](https://ns1.com/products/dedicated-dns). NS1 strongly recommends that a separate domain name is registered for  the Dedicated DNS servers, ensuring that there are two separate  namespaces in delegation. 

NS1 recommends this as a best practice to easily identify between the Dedicated DNS instances and the DNS setup for the customer’s standard  domain. For instance, the company “Example Company” with a domain  “example.net” may register the domain “exampledns.net” to use with our  Dedicated DNS services.

Once the Dedicated DNS service has been provisioned, the nameservers’ IP addresses will need to be added at the registrar as glue records.  Here is an example of four nameservers for the domain “exampledns.net”  being configured in Google Domains:

![img](https://ns1.com/writable/images/gluerecordsongoogledomains.png)

Once the glue records have been added at the registrar, delegate both the Dedicated DNS servers and Managed DNS servers to complete the  setup. The glue records at the registrar will make sure that the  Dedicated DNS setup will work properly.

...

...

...

# Glue Records

## What are Glue Records

Glue records are needed to prevent circular references. Circular references exist where the name servers for a domain can't be resolved without resolving the domain they're responsible for.

For example, if the name servers for yourdomain.com are ns1.yourdomain.com and ns2.yourdomain.com, the DNS client would not be able to get to either name server without knowing where yourdomain.com is. But this information is held by those name servers!

This is where glue records are used. A glue record is a hint that is provided by the parent DNS server. In the case of yourdomain.com, the .com GLTD (Global Top Level Domain) servers would provide the glue records. The glue records are simply additional A records that are returned with the DNS response. These additional A records allow the DNS client to locate the name servers.

## How do I add Glue Records

If you are using someone else's name servers (eg. your ISP's), you won't need to worry about glue records. You only need to worry about glue records when you are configuring your own name servers where a circular reference exists.

To add a glue record for yourdomain.com you will need to contact your name registrar, as the glue records need to be created on the parent name servers. In the case of yourdomain.com the parent name servers are the .com GTLD (Global Top Level Domain) servers.

...

...

...

[Glue record chck](http://www.webdnstools.com/dnstools/domain_check)

...

....

...

## What is a Glue Record?

As we learned in our article [What Are Domains?](https://www.liquidweb.com/kb/what-are-domains/), a domain is associated with an IP address. That IP directs visitors to  the correct location on the Internet that hosts your website and its  contents. In the same way, glue records or nameserver glue records, link a nameserver on the internet to an IP address. When a DNS request is  made for an IP address of a specific domain, it's queried at the  registrar. The registrar will provide any information that it has for  the DNS. If there is a glue record, it is presented as the place to look for any DNS zones. 

A glue record binds the IP address to a static cache, so visitors can  always locate your site without issue. This record also avoids  dependencies issues for that DNS zone. Typically, your registrar  maintains the glue records for a set of nameservers. This allows traffic to be directed without using the typical [lookup process of DNS](https://www.liquidweb.com/kb/dns-an-overview/). You will often see a glue record being used for nameservers. It can  also occasionally be utilized in other records, depending on the  circumstances.

## How is a Glue Record Different From Other DNS Records?

Let’s review [how DNS works](https://www.liquidweb.com/kb/how-to-demystify-the-dns-process/). Your computer normally has no idea how to locate a website on the  internet from a domain name we type into our browser. As an example, If  we said, “Call Bob's house”, we inherently would not know how to contact them. This request must be made to an intermediary first. That go  between information service is called the Domain Name System or DNS. DNS basically acts as the phone book of the internet and associated domain  names with IP addresses. In this case, Bob's house would equate to an IP like 192.168.1.1. 

Normally, we leave this behind the scenes translation of names to IPs to the DNS  nameservers. Even though we may be able to discover the nameserver for a domain, that nameserver is still a domain name. Nameservers (like  ns1.liquidweb.com) must be turned into an IP address before it can be  accessed. To look up the ns1.liquidweb.com A record, you will need to  know the IP address for liquidweb.com! At first, this may seem like a  catch-22 situation, right? This is where the nameserver glue records  come into play.

The ultimate authority for a fully qualified domain name (or FQDN) is the [domains' registrar](https://www.liquidweb.com/kb/where-is-my-dns-hosted/). The registrar is where domains are purchased and contain a list of the  nameservers related to a particular domain name. Similarly, we can  translate a nameserver name into an IP. Thatway  we can contact the  nameserver to get DNS records for the domain it controls.

## Example DNS Query

As an example, let’s use liquidweb.com again. We start out on our browser, not knowing anything about the domain’s IP, or even what nameservers it uses. Our first step will search our local DNS cache and hosts file, to see if we have visited the domain before, and if we have, a local  record is cached. In this example, we have not visited liquidweb.com  before. 

Next, our browser checks for the domain at our Internet Service Provider  (ISP) to see if they have a cached nameserver record. Let’s say *they* don’t; we then have to move up the chain to the next nameserver  provider which may be an internet provider like Level 5 or Cogent. If  they do not have a nameserver record, we proceed to one of the 13  worldwide root nameservers. If the domain is registered correctly and  resolves to an IP, the root nameservers will have a record stored. We  use a public service to determine the registrar, and subsequently, the  nameserver names. You can do the same thing by running the linux *whois* command:

```
root@host:~# whois liquidweb.com
   Domain Name: LIQUIDWEB.COM
   Registry Domain ID: 1458046_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.networksolutions.com
   Registrar URL: http://networksolutions.com
   Updated Date: 2020-08-04T12:02:49Z
   Creation Date: 1997-08-05T04:00:00Z
   Registry Expiry Date: 2030-08-04T04:00:00Z
   Registrar: Network Solutions, LLC
   Registrar IANA ID: 2
   Registrar Abuse Contact Email: abuse@web.com
   Registrar Abuse Contact Phone: +1.8003337680
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Name Server: NS.LIQUIDWEB.COM
   Name Server: NS1.LIQUIDWEB.COM
   DNSSEC: unsigned
```

This tells us the domain registrar (network solutions) and the authoritative nameservers (ns.liquidweb.com and ns1.liquidweb.com) along with a  significant amount of other useful information.

## Search for Nameserver DNS

Now that we have the [nameservers for liquidweb.com](https://www.liquidweb.com/kb/using-liquid-webs-nameservers/), and know whom to contact to get the IP address for the domain. If we  still cannot contact those nameservers; we can’t do another DNS lookup  for them, since the records are on the nameservers themselves! So, we  ask the registrar for the IP addresses of the nameservers as well. You  can test this query using the *whois* command.

```
root@host:~# whois ns.liquidweb.com
  Server Name: NS.LIQUIDWEB.COM
  IP Address: 69.16.222.254
  Registrar: Network Solutions, LLC
  Registrar WHOIS Server: whois.networksolutions.com
  Registrar URL: http://networksolutions.com
>>> Last update of whois database: 2021-02-26T17:11:01Z <<<

root@host:~# whois ns1.liquidweb.com
   Server Name: NS1.LIQUIDWEB.COM
   IP Address: 69.16.223.254
   Registrar: Network Solutions, LLC
   Registrar WHOIS Server: whois.networksolutions.com
   Registrar URL: http://networksolutions.com
>>> Last update of whois database: 2021-02-26T17:11:01Z <<<
```

In our example, we get 69.16.222.254 and 69.16.223.254 as the nameserver  IPs. Now that we have the IP addresses, we can ask ns.liquidweb.com and  ns1.liquidweb.com about the IP address of liquidweb.com, and the browser can carry on with its query for the web page. We can also use the [dig command](https://www.liquidweb.com/kb/how-to-use-dig/) to locate DNS information. You can see that without glue records set up at the registrar, we would never be able to contact the nameservers,  and no one would be able to go to liquidweb.com!

## Are Glue Records Needed?

Anyone who uses a shared set of nameservers, like ns.liquidweb.com and  ns1.liquidweb.com or a service like CloudFlare, will probably not need  to worry about glue records, since these are already set up. But, if we  are using [custom nameservers](https://www.liquidweb.com/kb/setting-up-private-nameservers-in-whmcpanel/), like ones based around our domain name, or if we are setting up a new  set of nameservers for a client, or if we are moving our nameservers  from one set of IPs to another during a domain migration, we will need  to make sure our glue records are set up properly.

## How To Set Up Nameserver Glue Records

Every domain registrar has different steps for setting up the [nameserver glue records](https://www.liquidweb.com/kb/how-to-decode-dns-zone-types/) for a domain. But, you will need to know a few things in advance to be successful.

1. We will need the login information to our registrar where the domain was  purchased. This is where we will set up our nameservers information.
2. Next, we need to know or choose the names of our nameservers. Most clients  select something similar to ns1 and ns2 or some variation of that. But,  any designation can be used.
3. Finally, we need to have an IP  addresses assigned for each of our nameservers. Some registrars are ok  with using the same IP address for both nameservers, however, best  practice dictates we use a different IP for each nameserver used.  Additionally, have geographically disparate nameservers is a bonus in  case the DNS service goes down on the main server. This allows for the  remote nameserver to still direct traffic to the main server.

## Setup Nameservers in WHM

For [cPanel servers](https://www.liquidweb.com/kb/setting-up-private-nameservers-in-whmcpanel/), and most other servers running the Berkeley Internet Name Domain (or  BIND) nameserver software, all the IPs on the machine are set up to  listen for DNS requests. This means that we can use any of our IPs for  any of your nameservers. But, we should ensure that the actual A records for the nameservers also match the glue records, to keep everything  resolving properly. Also, nameserver software (like BIND) and webserver  software (like Apache or Nginx) listen on different network ports, so  you can use the same IP for your nameservers as you do for apache  without any issues.

If we do not have direct access to the registrar, we can ask our host or  domain name reseller to set up glue records for us. If you purchased  your domain name through Liquid Web, simply open a support ticket or  chat with us, and we ensure your [glue records](https://www.liquidweb.com/kb/should-i-host-my-dns-or-use-liquid-webs/) are set up correctly. 

////

///

///

A lot has been  written and discussed regarding Domain Name System (DNS) in the past few months. The DDoS attacks on one of the major managed DNS providers a  while ago just made us all take DNS issues seriously once again.

So why so much emphasis on getting DNS, right? We at  Catchpoint, like a lot of other people in this ecosystem, strongly  believe that DNS is not just a metric but a lifeline; a backbone for our online systems. It is extremely important to the Internet, as it lays  the foundation for the WWW (World Wide Web).

DNS, in simple terms, translates hostnames to IP addresses.  Though the objective of DNS seems straightforward and simple, yet in  real life, it has grown to become one of the most complex systems we  have today.

- Domain registries
- Global top-level domains (gTLDs)
- Numerous country code top-level domains (ccTLDs).
- An ever-growing list of all the new TLDs (.space, .photography etc.).

All these add more complexity to an already complex system.

Since DNS is not restricted to a single machine (being a  distributed, coherent, and hierarchical database) and involves multiple  hierarchies and entities, ensuring that every hierarchy and entity  involved in managing the system is working efficiently becomes crucial.  At the top of the hierarchy is the

- **Root(.)**.
- **gTLD servers**.
- **Authoritative name servers** for domains.

Every level in this hierarchy has an important role to play in the resolution process of a domain name**.** 

- **The registries** (Verisign managing .COM and .NET).
- **Registrars** (GoDaddy and Namecheap).
- **Registrants** (we who register a domain name).
- **ISPs**.
- **Managed DNS Service Providers**.

We all are a part of this system and it becomes extremely  important for us, as registrants, to keep an eye on how these multiple  components are functioning to ensure that we have a stable and  well-functioning system.

In this article, we will be focusing more on a very important concept in DNS: additional records, or glue records.

## **Additional Records (Glue Records)**

In simplest of terms, glue records are A records or IP  addresses that are assigned or mapped to a domain name or a subdomain.  Glue records become extremely important when the name servers for a  domain name are the subdomains of the domain name itself.

The glue records can be seen under the additional section of a DNS response.

Let’s take an example to understand how glue records work; assume you have a domain name called *yourdomain.com* for which you are using the following set of nameservers:

- *ns1. yourdomain.com*.
- *ns2. yourdomain.com*.

In the DNS Resolution process, the authoritative name servers for *yourdomain.com* are *ns1.yourdomain.com* and *ns2.yourdomain.com*. The DNS resolution for *ns1.yourdomain.com* would first require the resolution of *yourdomain.com* which in turn returns the authoritative name servers as *ns1* and *ns2.yourdomain*.

As you may have already noticed, this creates a circular  dependency — or in simple terms, a loop — and the resolution never  succeeds.

Glue records help in breaking this dependency by providing the IP Addresses for *ns1.yourdomain.com* and *ns2.yourdomain.com* in the lookup process, this breaks the loop from getting created, as we no longer need to resolve the name servers for the IP addresses; these  addresses are already provided in the form of glue records.

[![img](http://blogstage.catchpoint.com/wp-content/uploads/2017/04/Image2.png)](http://blog.catchpoint.com/wp-content/uploads/2017/04/Image2.png)

In the example above, we see that glue records helped remove the circular dependency by providing the A records for *ns1.ctrls.in* and *ns2.ctrls.in* that were returned as the authoritative name servers for the domain *ctrls.in*. If this were not the case, the DNS Lookup would have failed because of a circular dependency.

For domain names, which do not use subdomains of the same  domain as authoritative nameservers, glue records help in reducing the  number of lookups by providing the IP addresses for the authoritative  name servers. Here is an example for *Wikipedia.com*.

[![img](http://blogstage.catchpoint.com/wp-content/uploads/2017/04/Image1.png)](http://blog.catchpoint.com/wp-content/uploads/2017/04/Image1.png)

In this case, *Wikipedia.org* returned *ns1.wikimedia.org*, *ns2.wikimedia.org*, and *ns3.wikimedia.org* as the authoritative name servers for the domain. This would have required an additional level of DNS lookup for *Wikimedia.org* to get the A/AAAA record for the domain name initially queried for *Wikipedia.org*.

One of our customers, a leading CDN provider headquartered  in China, reached out to us a while ago complaining that the A records  being returned for 2 of their name servers were incorrect (old IPs).

When investigating this case, we observed that when doing a  DNS Experience test for the Nameservers, the IPs being returned by the  authoritative name servers were correct. However, when running a DNS  direct test to the name servers of the domain using any of the gTLDs (*a-m.gtld-servers.net.*), the IPs returned were the incorrect IPs.

Digs to the domain name using the command `dig “domain name here” @a.root-servers.net` returned the same response as Catchpoint’s DNS tests.

Further investigation led us to believe that this was one of those cases where the changes to the GLUE/additional record at the  domain registrar’s end was not pushed to the gTLD servers.

| **Catchpoint DNS Monitors ** |                                                              |
| ---------------------------- | ------------------------------------------------------------ |
| Experience DNS Test          | For DNS tests that use the experience monitor,  Catchpoint randomly selects a server from each level of the DNS route  and queries it for the domain. |
| Direct DNS Test              | This test provides the complete query and  response from the DNS server specified for the test along with the  length of time it took to complete the test and any errors received  during testing. |

## **What Fixed This Issue?**

Based on our recommendations, our client reached out to the  domain registrar for the domain and got the glue records updated for the domain. The change made was pushed to all the gTLD servers and the  issue was resolved.

This incident emphasizes the importance of monitoring each  level as well as each component of this amazingly vast system we know as DNS. Having a monitoring strategy focused around DNS is not just  recommended but is crucial to discover issues that may be under our  control or out of our control.



# Answer From stack over flow

----------------------------------------

-----------------------------

--------

 A glue record is a term for a record that's served by a DNS server  that's not authoritative for the zone, to avoid a condition of  impossible dependencies for a DNS zone.

Say I own a DNS zone for `example.com`.  I want to have  DNS servers that're hosting the authoritative zone for this domain so  that I can actually use it - adding records for the root of the domain, `www`, `mail`, etc.  So, I put the name servers in the registration to delegate to them - those are always names, so we'll put in `ns1.example.com` and `ns2.example.com`.

There's the trick.  The TLD's servers will delegate to the DNS servers in the whois record - but they're within `example.com`.  They try to find `ns1.example.com`, ask the `.com` servers, and get referred back to... `ns1.example.com`.

What glue records do is to allow the TLD's servers to send extra information in their response to the query for the `example.com` zone - to send the IP address that's configured for the name servers,  too.  It's not authoritative, but it's a pointer to the authoritative  servers, allowing for the loop to be resolved.

---

---

---

​                    



I requested that this answer be merged in from a duplicate question, as the existing answers did not explain the role of the `ADDITIONAL` section.

To see how it works, type this: `dig +trace +additional google.com SOA`

This will trace the nameserver authority starting from the root servers (`+trace`). Adding `+additional` will also show you the `ADDITIONAL` section of each DNS server response. Normally most people think of DNS in terms of the `QUESTION` and the `ANSWER` sections, but `ADDITIONAL` also plays an important role: if the nameserver knows the answers to  any queries that are related to the answer, it can pre-emptively supply  those answers in the `ADDITIONAL` section without requiring additional queries from your client.

Note that the authoritative nameservers for `google.com` are rooted under the domain they're authoritative for. (`ns1.google.com`, `ns2.google.com`, etc.)

When you ask a nameserver to supply the list of nameservers for a domain, they will often supply a list of `A`-type records (IP addresses) in the `ADDITIONAL` section, not just the `NS`-type answers: these are called [glue records](http://en.wikipedia.org/wiki/Domain_Name_System#Circular_dependencies_and_glue_records), used to prevent circular dependencies. In this case, those `A` records are served from the TLD (.com, .org, etc.) nameservers based on the IP addresses that someone supplied the DNS registrar responsible  for the domain. They can usually be changed by logging into the admin  web interface they supply you. 

*(disclaimer: `AAAA` records containing IPV6 addresses can also be supplied as part of the glue, but I left this out for simplicity's sake.)*