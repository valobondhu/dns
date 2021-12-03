# How Dns work

Hi all![smiley](http://slashroot.in/sites/all/libraries/ckeditor/plugins/smiley/images/regular_smile.gif)...in this post we will be discussing the most important and inevitable  resource in the world of internet, which each and everyone of us uses  knowingly or unknowingly. 

Each and every machine in a network is identified by a numerical address.  This address can be used by other machines in the network to communicate with each other. But without a relevant name associated with that  numerical address, it will be very difficult to memorize the numerical  address of all the machines in the network. Even for a handful of hosts  in a local network, it will be very difficult to memorize all the  numerical address of each of them, so forget about memorizing numerical  addresses of more than 400 million hosts in the internet.

initially the concept of host file was born to solve the problem, each and every  machine in the network used to have a host file, /etc/hosts where the  name to address mapping was done.. But with the passage of time,problems like the below emerged.

1.each and every machine needed to update the the newly added entries themselves.

2.there was no kind of notification available for clients to know a new entry has been added.

3.by the passage of time, a single file became large and very large, making it difficult to handle.

During the mid 1970's the concept of name servers came into place. the basic  idea behind this name servers was that, people find it easy to remember  names rather than numbers, especially when that name describes some  attributes of a resource. 

This main problem of converting names to numbers in networking is as old as computer networking itself.

When a name server is present in a network the machines in the network only  needed to know, the numerical address of the name server and the name of the destination machine or a website. With these information in hand  the machines in the network can ask the name server in the network for  the numerical address (IP address) of the destination.

a centralized server for the name server was much better than /etc/hosts  file solution. because now with a central server dedicated for name to  address mapping, the machines in the network only needed to know the  numerical address of the name server, and the name server will return  the numerical address of a name, whenever asked by the clients.

the major advantage of having a central name server was that the numerical  address or the IP address of the server, can be changed without the  clients being aware of the change. In such situations the name server  just needed to be updated or modified with the new IP address.

But there were some drawbacks of a central name server also, like what if  the central name server is not available? Hence came the idea of  multiple name servers, in the network, one acted like a master or  primary, and the other a slave or secondary. If master is not available  then the secondary name server of the slave name server, is queried for  the answer.

There were some main drawbacks of even this kind of an implementation (primary and secondary name servers). they are as follows.

1.As the names in the network goes on increasing, it becomes too much  difficult for a name server to retrieve an information from millions of  entries. So we needed a method to organize the names.

2.Imagine a single name server getting lot and lots of queries per second, in  such cases the load on the name server increases. So we need to find a  method to spread the load.

3.We needed a mechanism to separate the administration of the entries in the name server, as many different administrators used to add entires.

### **The Domain Name System of the Internet.**

the complete DNS functionality is explained in the following RFC's

RFC 1034

RFC 1035

The domain name system of the internet works in a inverted tree  structure.At the top of the tree is the root name server(don't worry, i  will explain whats a root server).The root server is followed by TLD's  or Top Level Domains,and then TLD's are followed by SLD's or Second  Level Domains. All of these are seperated by dots.

Understanding the above explained thing which is underlined is very much important in understanding the concept of DNS.

The root server is represented by a .(a dot).

TLD's are split into two types as follows.

![types of TLD(top level domains)](https://www.slashroot.in/sites/default/files/types%20of%20tld.png)

Generic Top Level Domains(gTLD's) are TLD's like .com,.net,.org,.edu etc.



​       

Country Code Top Level Domains are domains such as .in,.us,.uk etc.

Now when we call www.slashroot.in a domain name, this domain name is a  combination of gTLD,SLD(Secondry Level Domain) and the host name.We will come back to this in some time.

When we normally call a domain like google.com its the combination of TLD,SLD.

![TLD and SLD in a domain](https://www.slashroot.in/sites/default/files/tld%20and%20sld%20in%20a%20domain.png)

Each and every node in this Domain Name system is assigned to an authority  or organization for its administration. And that organization resposible for a particular node is authoritative for that node.The term authoritative will be used many times in DNS system.

Now the authority of the .(root name server) which is at the top of the heirarchy lies with an organization named  ICANN(Internet Corporation for Assigned Names And Numbers.).

gTLD's like (.com,.net) and others are also administered by ICANN and are also delegated to ICANN accredited registrars. ccTLD's are accredited to  different countries for administration by ICANN.

Delegation in DNS is an important concept...I will keep another dedicated post only for delegation.

It is very much important to understand the fact that, the left most part  (www) in any address, like for example www.slashroot.in, is the  hostname. WWW is used by websites only by convention, there is no rule  to use www for a website. A web site can also be named xyz.example.com.

 

### **what happens when I type www.example.com in the address bar of the browser?**

the root name server(.) is the most important resource in the name server  heirarchy. when any name server is asked for an information which it  does not have, the first thing that name server does is asking one of  the (.)root name server.

there are 13 root name servers as follows.

a.root-servers.net.
b.root-servers.net.
c.root-servers.net.
d.root-servers.net.
e.root-servers.net.
f.root-servers.net.
g.root-servers.net.
h.root-servers.net.
i.root-servers.net.
j.root-servers.net.
k.root-servers.net.
l.root-servers.net.
m.root-servers.net.

Now the ip address of all the root servers mentioned above are known to all the DNS software packages, by default. Which means all the DNS servers  can reach these root servers without any other DNS server.

**Step1:** the client types www.example.com in his browser

**Step2:** the operating system looks at /etc/host file,first for the ip address of  www.example.com(this can be changed from /etc/nsswitch), then looks  /etc/resolv.conf for the DNS server IP for that machine

**Step3:** the dns server will search its database for the name www.example.com,  if it finds it will give that back, if not it will query the root  server(.) for the information.

**Step4:** root server will return a referral to the .com TLD name server(these  TLD name servers knows the address of name servers of all SLD's).In our case we searched for www.example.com so root server will give us referral to .com TLD servers.

If it was www.example.net then root server will give, .net TLD servers refferal.

**Step5:** Now One of the TLD servers of .com will give us the referral to the DNS server resposible for example.com domain.

**Step6:** the dns server for example.com domain will now give the client the ip address of www host(www is the host name.)

Now lets practically have a look at how this process works.

> [root@myvm1 ~]# dig +trace www.google.com
>
> ; <<>> DiG 9.3.4-P1 <<>> +trace www.google.com
> ;; global options: printcmd
> .            5    IN   NS   a.root-servers.net.
> .            5    IN   NS   b.root-servers.net.
> .            5    IN   NS   c.root-servers.net.
> .            5    IN   NS   d.root-servers.net.
> .            5    IN   NS   e.root-servers.net.
> .            5    IN   NS   f.root-servers.net.
> .            5    IN   NS   g.root-servers.net.
> .            5    IN   NS   h.root-servers.net.
> .            5    IN   NS   i.root-servers.net.
> .            5    IN   NS   j.root-servers.net.
> .            5    IN   NS   k.root-servers.net.
> .            5    IN   NS   l.root-servers.net.
> .            5    IN   NS   m.root-servers.net.
> ;; Received 228 bytes from 192.168.159.2#53(192.168.159.2) in 49 ms
>
> com.          172800 IN   NS   a.gtld-servers.net.
> com.          172800 IN   NS   b.gtld-servers.net.
> com.          172800 IN   NS   c.gtld-servers.net.
> com.          172800 IN   NS   d.gtld-servers.net.
> com.          172800 IN   NS   e.gtld-servers.net.
> com.          172800 IN   NS   f.gtld-servers.net.
> com.          172800 IN   NS   g.gtld-servers.net.
> com.          172800 IN   NS   h.gtld-servers.net.
> com.          172800 IN   NS   i.gtld-servers.net.
> com.          172800 IN   NS   j.gtld-servers.net.
> com.          172800 IN   NS   k.gtld-servers.net.
> com.          172800 IN   NS   l.gtld-servers.net.
> com.          172800 IN   NS   m.gtld-servers.net.
> ;; Received 504 bytes from 198.41.0.4#53(a.root-servers.net) in 153 ms
>
> google.com.       172800 IN   NS   ns2.google.com.
> google.com.       172800 IN   NS   ns1.google.com.
> google.com.       172800 IN   NS   ns3.google.com.
> google.com.       172800 IN   NS   ns4.google.com.
> ;; Received 168 bytes from 192.33.14.30#53(b.gtld-servers.net) in 12 ms
>
> www.google.com.     300   IN   A    74.125.236.48
> www.google.com.     300   IN   A    74.125.236.50
> www.google.com.     300   IN   A    74.125.236.51
> www.google.com.     300   IN   A    74.125.236.49
> www.google.com.     300   IN   A    74.125.236.52
> ;; Received 112 bytes from 216.239.34.10#53(ns2.google.com) in 108 ms
>  


Now you can clearly see from the dig with trace output that, the request first went to root servers. a.root-servers.net replied me with the addresses of all .com gtld servers, and b.gtld-servers.net gave me the name servers for google.com and finally ns2.google.com replied me with the ip address of www.google.com 

Hope you guys enjoyed the post...!!



----

----

## What is DNS?

The Domain Name System (DNS) is the phonebook of the Internet. Humans access information online through [domain names](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/), like nytimes.com or espn.com. Web browsers interact through [Internet Protocol (IP)](https://www.cloudflare.com/learning/network-layer/internet-protocol/) addresses. DNS translates domain names to [IP addresses](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) so browsers can load Internet resources.

Each device connected to the Internet has a unique IP address which  other machines use to find the device. DNS servers eliminate the need  for humans to memorize IP addresses such as 192.168.1.1 (in IPv4), or  more complex newer alphanumeric IP addresses such as  2400:cb00:2048:1::c629:d7a2 (in IPv6).

![DNS](https://www.cloudflare.com/img/learning/dns/what-is-dns/theinternet-dns.svg)

## How does DNS work?

The process of DNS resolution involves converting a hostname (such as www.example.com) into a computer-friendly IP address (such as  192.168.1.1). An IP address is given to each device on the Internet, and that address is necessary to find the appropriate Internet device -  like a street address is used to find a particular home.  When a user  wants to load a webpage, a translation must occur between what a user  types into their web browser (example.com) and the machine-friendly  address necessary to locate the example.com webpage.

In order to understand the process behind the DNS resolution, it’s  important to learn about the different hardware components a DNS query  must pass between. For the web browser, the DNS lookup occurs "behind  the scenes"  and requires no interaction from the user’s computer apart  from the initial request.

## There are 4 DNS servers involved in loading a webpage:

- **[DNS recursor](https://www.cloudflare.com/learning/dns/dns-server-types#recursive-resolver)** - The recursor can be thought of as a librarian who is asked to go find a particular book somewhere in a library. The DNS recursor is a server  designed to receive queries from client machines through applications  such as web browsers. Typically the recursor is then responsible for  making additional requests in order to satisfy the client’s DNS query.
- **Root nameserver** - The [ root server](https://www.cloudflare.com/learning/dns/glossary/dns-root-server/) is the first step in translating (resolving) human readable host names  into IP addresses. It can be thought of like an index in a library that  points to different racks of books -  typically it serves as a reference to other more specific locations.
- **[TLD nameserver](https://www.cloudflare.com/learning/dns/dns-server-types#tld-nameserver)** - The top level domain server ([TLD](https://www.cloudflare.com/learning/dns/top-level-domain/)) can be thought of as a specific rack of books in a library. This  nameserver is the next step in the search for a specific IP address, and it hosts the last portion of a hostname (In example.com, the TLD server is “com”).
- **[Authoritative nameserver](https://www.cloudflare.com/learning/dns/dns-server-types#authoritative-nameserver)** - This final nameserver can be thought of as a dictionary on a rack of  books, in which a specific name can be translated into its definition.  The authoritative nameserver is the last stop in the nameserver query.  If the authoritative name server has access to the requested record, it  will return the IP address for the requested hostname back to the DNS  Recursor (the librarian) that made the initial request.

## What's the difference between an authoritative DNS server and a recursive DNS resolver?

Both concepts refer to servers (groups of servers) that are integral  to the DNS infrastructure, but each performs a different role and lives  in different locations inside the pipeline of a DNS query. One way to  think about the difference is the [recursive](https://www.cloudflare.com/learning/dns/what-is-recursive-dns/) resolver is at the beginning of the DNS query and the authoritative nameserver is at the end.

#### Recursive DNS resolver

The recursive resolver is the computer that responds to a recursive request from a client and takes the time to track down the [DNS record](https://www.cloudflare.com/learning/dns/dns-records/). It does this by making a series of requests until it reaches the  authoritative DNS nameserver for the requested record (or times out or  returns an error if no record is found). Luckily, recursive DNS  resolvers do not always need to make multiple requests in order to track down the records needed to respond to a client; [caching](https://www.cloudflare.com/learning/cdn/what-is-caching/) is a data persistence process that helps short-circuit the necessary  requests by serving the requested resource record earlier in the DNS  lookup.

![How DNS works - the 10 steps in a DNS query](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-1.png)

#### Authoritative DNS server

Put simply, an authoritative DNS server is a server that actually  holds, and is responsible for, DNS resource records. This is the server  at the bottom of the DNS lookup chain that will respond with the queried resource record, ultimately allowing the web browser making the request to reach the IP address needed to access a website or other web  resources. An authoritative nameserver can satisfy queries from its own  data without needing to query another source, as it is the final source  of truth for certain DNS records.

![DNS query diagram](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-2.png)

It’s worth mentioning that in instances where the query is for a subdomain such as foo.example.com or [blog.cloudflare.com](https://blog.cloudflare.com/), an additional nameserver will be added to the sequence after the  authoritative nameserver, which is responsible for storing the  subdomain’s [CNAME record](https://www.cloudflare.com/learning/dns/dns-records/dns-cname-record/).

![DNS query diagram](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-3.png)

There is a key difference between many DNS services and the one that  Cloudflare provides.  Different DNS recursive resolvers such as Google  DNS, OpenDNS, and providers like Comcast all  maintain data center  installations of DNS recursive resolvers. These resolvers allow for  quick and easy queries through optimized clusters of DNS-optimized  computer systems, but they are fundamentally different than the  nameservers hosted by Cloudflare.

Cloudflare maintains infrastructure-level nameservers that are  integral to the functioning of the Internet. One key example is the [f-root server network](https://blog.cloudflare.com/f-root/) which Cloudflare is partially responsible for hosting. The F-root is  one of the root level DNS nameserver infrastructure components  responsible for the billions of Internet requests per day. Our [Anycast network](https://www.cloudflare.com/learning/cdn/glossary/anycast-network/) puts us in a unique position to handle large volumes of DNS traffic without service interruption.

## What are the steps in a DNS lookup?

For most situations, DNS is concerned with a domain name being  translated into the appropriate IP address. To learn how this process  works, it helps to follow the path of a DNS lookup as it travels from a  web browser, through the DNS lookup process, and back again. Let's take a look at the steps.

Note: Often DNS lookup information will be cached either locally  inside the querying computer or remotely in the DNS infrastructure.  There are typically 8 steps in a DNS lookup. When DNS information is  cached, steps are skipped from the DNS lookup process which makes it  quicker. The example below outlines all 8 steps when nothing is cached.

####  The 8 steps in a DNS lookup:

1. A user types ‘example.com’ into a web browser and the query travels  into the Internet and is received by a DNS recursive resolver.
2. The resolver then queries a DNS root nameserver (.).
3. The root server then responds to the resolver with the address of a  Top Level Domain (TLD) DNS server (such as .com or .net), which stores  the information for its domains. When searching for example.com, our  request is pointed toward the .com TLD.
4. The resolver then makes a request to the .com TLD.
5. The TLD server then responds with the IP address of the domain’s nameserver, example.com.
6. Lastly, the recursive resolver sends a query to the domain’s nameserver.
7. The IP address for example.com is then returned to the resolver from the nameserver.
8. The DNS resolver then responds to the web browser with the IP address of the domain requested initially.
9. Once the 8 steps of the DNS lookup have returned the IP address for  example.com, the browser is able to make the request for the web page:

10. The browser makes a [HTTP](https://www.cloudflare.com/learning/ddos/glossary/hypertext-transfer-protocol-http/) request to the IP address.
11. The server at that IP returns the webpage to be rendered in the browser (step 10).

![DNS query diagram](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-lookup-diagram.png)

## What is a DNS resolver?

The DNS resolver is the first stop in the DNS lookup, and it is  responsible for dealing with the client that made the initial request.  The resolver starts the sequence of queries that ultimately leads to a  URL being translated into the necessary IP address.

Note: A typical uncached DNS lookup will involve both recursive and iterative queries.

It's important to differentiate between a [recursive DNS](https://www.cloudflare.com/learning/dns/what-is-recursive-dns/) query and a recursive DNS resolver. The query refers to the request  made to a DNS resolver requiring the resolution of the query. A DNS  recursive resolver is the computer that accepts a recursive query and  processes the response by making the necessary requests.

![DNS query diagram](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-recursive-query.png)

## What are the types of DNS queries? 

In a typical DNS lookup three types of queries occur. By using a  combination of these queries, an optimized process for DNS resolution  can result in a reduction of distance traveled. In an ideal situation  cached record data will be available, allowing a DNS name server to  return a non-recursive query.

#### 3 types of DNS queries:

1. **Recursive query** - In a recursive query, a DNS  client requires that a DNS server (typically a DNS recursive resolver)  will respond to the client with either the requested resource record or  an error message if the resolver can't find the record. 
2. **Iterative query** - in this situation the DNS client  will allow a DNS server to return the best answer it can. If the queried DNS server does not have a match for the query name, it will return a  referral to a DNS server authoritative for a lower level of the domain  namespace. The DNS client will then make a query to the referral  address. This process continues with additional DNS servers down the  query chain until either an error or timeout occurs.
3. **Non-recursive query** - typically this will occur  when a DNS resolver client queries a DNS server for a record that it has access to either because it's authoritative for the record or the  record exists inside of its cache. Typically, a DNS server will cache  DNS records to prevent additional bandwidth consumption and load on  upstream servers.

## What is DNS caching? Where does DNS caching occur? 

The purpose of caching is to temporarily stored data in a location  that results in improvements in performance and reliability for data  requests. DNS caching involves storing data closer to the requesting  client so that the DNS query can be resolved earlier and additional  queries further down the DNS lookup chain can be avoided, thereby  improving load times and reducing bandwidth/CPU consumption. DNS data  can be cached in a variety of locations, each of which will store DNS  records for a set amount of time determined by a [time-to-live (TTL)](https://www.cloudflare.com/learning/cdn/glossary/time-to-live-ttl/).

#### Browser DNS caching

Modern web browsers are designed by default to cache DNS records for a set amount of time. The purpose here is obvious; the closer the DNS  caching occurs to the web browser, the fewer processing steps must be  taken in order to check the cache and make the correct requests to an IP address. When a request is made for a DNS record, the browser cache is  the first location checked for the requested record.

In Chrome, you can see the status of your DNS cache by going to chrome://net-internals/#dns.

#### Operating system (OS) level DNS caching

The operating system level DNS resolver is the second and last local  stop before a DNS query leaves your machine. The process inside your  operating system that is designed to handle this query is commonly  called a “stub resolver” or DNS client. When a stub resolver gets a  request from an application, it first checks its own cache to see if it  has the record. If it does not, it then sends a DNS query (with a  recursive flag set), outside the local network to a DNS recursive  resolver inside the Internet service provider (ISP).

When the recursive resolver inside the ISP receives a DNS query, like all previous steps, it will also check to see if the requested  host-to-IP-address translation is already stored inside its local  persistence layer.

The recursive resolver also has additional functionality depending on the types of records it has in its cache:

1. If the resolver does not have the [A records](https://www.cloudflare.com/learning/dns/dns-records/dns-a-record/), but does have the [NS records](https://www.cloudflare.com/learning/dns/dns-records/dns-ns-record/) for the authoritative nameservers, it will query those name servers  directly, bypassing several steps in the DNS query. This shortcut  prevents lookups from the root and .com nameservers (in our search for  example.com) and helps the resolution of the DNS query occur more  quickly.
2. If the resolver does not have the NS records, it will send a query  to the TLD servers (.com in our case), skipping the root server.
3. In the unlikely event that the resolver does not have records  pointing to the TLD servers, it will then query the root servers. This  event typically occurs after a DNS cache has been purged. 

Learn about what differentiates [Cloudflare DNS](https://www.cloudflare.com/dns/) from other DNS providers.