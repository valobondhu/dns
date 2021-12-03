





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

---

----

## What is a top-level domain (TLD)?

In the [DNS](https://www.cloudflare.com/learning/ddos/glossary/domain-name-system-dns/) hierarchy, a top-level domain (TLD) represents the first stop after the root zone. In simpler terms, a TLD is everything that follows the final dot of a [domain name](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/). For example, in the domain name ‘google.com’, ‘.com’ is the TLD. Some other popular TLDs include ‘.org’, ‘.uk’, and ‘.edu’.

TLDs play an important role in the DNS lookup process. For all  uncached requests, when a user enters a domain name like ‘google.com’  into their browser window, the [DNS resolvers](https://www.cloudflare.com/learning/dns/dns-server-types/#recursive-resolver) start the search by communicating with the [TLD server](https://www.cloudflare.com/learning/dns/dns-server-types/). In this case, the TLD is ‘.com’, so the resolver will contact the TLD  DNS server, which will then provide the resolver with the IP address of  Google’s [origin server](https://www.cloudflare.com/learning/cdn/glossary/origin-server/).

The Internet Corporation for Assigned Names and Numbers (ICANN) has  authority over all TLDs used on the Internet, and it delegates the  responsibility of these TLDs to various organizations. For example, a  U.S. company called VeriSign operates all ‘.com’ and ‘.net’ TLDs.

Another purpose of TLDs is to help classify and communicate the  purpose of domain names. Every TLD will tell you something about the  domain that precedes it; let’s look at some examples:

- **’.com’** is intended for commercial businesses.
- **’.gov’** is for U.S. government entities.
- **’.uk’** is for domains from the United Kingdom.

TLDs themselves are also classified into one of several groups.

## What are the different types of TLDs?

- **Generic TLDs:** Generic TLDs (gTLDs) encompass some  of the more common domain names seen on the web, such as ‘.com’, ‘.net’, and ‘.org’. The Internet Corporation for Assigned Names and Numbers  (ICANN) used to heavily restrict the creation of new gTLDs, but in 2010  these restrictions were relaxed. Now there are hundreds of lesser-known  gTLDs, such as ‘.top’, ‘.xyz’, and ‘.loan’.
- **Country-code TLDs:** Country-code TLDs (ccTLDs) are  reserved for use by countries, sovereign states, and territories. Some  examples are ‘.uk’, ‘.au’ (Australia), and ‘.jp’ (Japan). The Internet  Assigned Numbers Authority (IANA), which is run by ICANN, is in charge  of picking appropriate organizations in each location to manage ccTLDs.
- **Sponsored TLDs:** These TLDs typically represent  professional, ethnic, or geographical communities. Each sponsored TLD  (sTLD) has a delegated sponsor that represents that community. For  example, ‘.app’ is a TLD intended for the developer community, and it is sponsored by Google. Similarly, ‘.gov’ is intended for use by the U.S.  government, and is sponsored by the General Services Administration.
- **Infrastructural TLDs:** This category only contains a single TLD: ‘.arpa’. Named for DARPA, the U.S. military research  organization that helped pioneer the modern Internet, ‘.arpa’ was the  first TLD ever created and is now reserved for infrastructural duties,  such as facilitating [reverse DNS](https://www.cloudflare.com/learning/cdn/glossary/origin-server/) lookups.
- **Reserved TLDs:** Some TLDs are on a reserved list,  which means they are permanently unavailable for use. For example,  ‘.localhost’ is reserved for local computer environments, and ‘.example’ is reserved for use in example demonstrations.

## Do TLDs Matter?

There are now so many TLD options available that the choice can be  overwhelming for someone trying to register a new domain name. For years ‘.com’ was seen as the only option for businesses that want to be taken seriously. But experts predict that as the supply of ‘.com’ domains  dwindles and some of the newer TLDs continue to pick up steam, we will  see a major shift in the perception of alternative TLDs. With big  companies like Twitter and Apple starting to adopt alternative TLDs for  their products (t.co and itun.es, respectively) we are already seeing  that shift in action, so it may be better to create a clever and  memorable domain name using an alternative TLD, than to insist on a  ‘.com’ domain.

---

---

## What is a DNS zone?

[The DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) is broken up into many different zones. These zones differentiate  between distinctly managed areas in the DNS namespace. A DNS zone is a  portion of the DNS namespace that is managed by a specific organization  or administrator. A DNS zone is an administrative space which allows for more granular control of DNS components, such as [authoritative nameservers](https://www.cloudflare.com/learning/dns/dns-server-types/#authoritative-nameserver). The [domain](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) name space is a hierarchical tree, with the DNS root domain at the top. A DNS zone starts at a domain within the tree and can also extend down  into subdomains so that multiple subdomains can be managed by one  entity.

A common mistake is to associate a DNS zone with a domain name or a single [DNS server](https://www.cloudflare.com/learning/dns/dns-server-types/). In fact, a DNS zone can contain multiple subdomains and multiple zones  can exist on the same server. DNS zones are not necessarily physically  separated from one another, zones are strictly used for delegating  control.

For example, imagine a hypothetical zone for the cloudflare.com  domain and three of its subdomains: support.cloudflare.com,  community.cloudflare.com, and blog.cloudflare.com. Suppose the blog is a robust, independent site that needs separate administration, but the  support and community pages are more closely associated with  cloudflare.com and can be managed in the same zone as the primary  domain. In this case, cloudflare.com as well as the support and  community sites would all be in one zone, while blog.cloudflare.com  would exist in its own zone. 

![DNS Zone](https://www.cloudflare.com/img/learning/dns/glossary/dns-zone/dns-zone.png)

All of the information for a zone is stored in what’s called a DNS  zone file, which is the key to understanding how a DNS zone operates.

## What is a DNS zone file?

A zone file is a plain text file stored in a DNS server that  contains an actual representation of the zone and contains all the  records for every domain within the zone. Zone files must always start  with a [Start of Authority (SOA) record](https://www.cloudflare.com/learning/dns/dns-records/dns-soa-record/), which contains important information including contact information for the zone administrator.

## What is a Reverse Lookup Zone?

 A [reverse lookup zone](https://www.cloudflare.com/learning/dns/glossary/reverse-dns/) contains mapping from an [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) to the host (the opposite function of most DNS zones). These zones are used for troubleshooting, spam filtering, and [bot](https://www.cloudflare.com/learning/bots/what-is-a-bot/) detection.

Learn more about [Cloudflare's DNS](https://www.cloudflare.com/dns/)



---

---

## What is reverse DNS?

A reverse [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) lookup is a DNS query for the [domain name](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) associated with a given [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/). This accomplishes the opposite of the more commonly used forward DNS  lookup, in which the DNS system is queried to return an IP address.

Standards from the Internet Engineering Task Force (IETF) [suggest](https://tools.ietf.org/html/rfc1912) that every domain should be capable of reverse DNS lookup, but as  reverse lookups are not critical to the normal function of the Internet, they are not a hard requirement. As such, reverse DNS lookups are not  universally adopted.

## How does reverse DNS work?

Reverse DNS lookups query DNS servers for a PTR (pointer) record; if the server does not have a [PTR record](https://www.cloudflare.com/learning/dns/dns-records/dns-ptr-record/), it cannot resolve a reverse lookup. PTR records store IP addresses with their segments reversed, and they append ".in-addr.arpa" to that. For  example if a domain has an IP address of 192.0.2.1, the PTR record will  store the domain's information under 1.2.0.192.in-addr.arpa.

In IPv6, the latest version of the Internet Protocol, PTR records  are stored within the ".ip6.arpa" domain instead of ".in-addr.arpa."

## What are reverse DNS lookups used for?

Reverse lookups are commonly used by email servers. Email servers  check and see if an email message came from a valid server before  bringing it onto their network. Many email servers will reject messages  from any server that does not support reverse lookups or from a server  that is highly unlikely to be legitimate. Spammers often use IP  addresses from hijacked machines, which means there will be no PTR  record. Or, they may use dynamically assigned IP addresses that lead to  server domains with highly generic names.

Logging software also employs reverse lookups in order to provide  users with human-readable domains in their log data, as opposed to a  bunch of numeric IP addresses.

---

---

## What is recursive DNS?

A recursive [DNS](https://www.cloudflare.com/learning/dns/what-is-dns/) lookup is where one [DNS server](https://www.cloudflare.com/learning/dns/dns-server-types/) communicates with several other DNS servers to hunt down an [IP address](https://www.cloudflare.com/learning/dns/glossary/what-is-my-ip-address/) and return it to the client. This is in contrast to an iterative DNS  query, where the client communicates directly with each DNS server  involved in the lookup. While this is a very technical definition, a  closer look at the DNS system and the difference between recursion and  iteration should help clear things up.

## What is a DNS server?

Whenever a user types a [domain name](https://www.cloudflare.com/learning/dns/glossary/what-is-a-domain-name/) (such as ‘cloudflare.com’) into their browser window, this triggers a  DNS lookup. A series of remote computers known as DNS servers then find  the IP address for that domain and return it to the user’s computer so  that they can access the correct website.

Several different types of DNS servers must work in conjunction to  complete a DNS lookup. A DNS resolver, DNS root server, DNS TLD server,  and DNS authoritative nameserver must all provide information to  complete the lookup. In the case of [caching](https://www.cloudflare.com/learning/cdn/what-is-caching/), one of these servers may have saved the answer to a query during a previous lookup, and can then deliver it from memory.

For more on how a DNS lookup works, see [What is a DNS Server?](https://www.cloudflare.com/learning/dns/dns-server-types/)

## What is the difference between recursion and iteration?

Recursion and iteration are computer science terms that describe two  different methods to solve a problem. In recursion, a program repeatedly calls itself until a condition is met, while in iteration, a set of  instructions is repeated until a condition is met. This subtle  difference is hard to illustrate without getting into code, but the key  takeaway is that recursion is a solution that repeatedly calls upon  itself.

For example, imagine that Jim lost his keys at home and is looking  for a systematic way to find them. A recursive solution would be for Jim to keep looking for his keys until he finds them. Jim will start  looking, and if he doesn’t find his keys, he will return to his original instruction to keep looking until he finds them. An iterative solution  would be for Jim to to search one room for five minutes, then go back to his instructions and search the next room for five minutes, and  continue this cycle until he either finds his keys or has gone through  the entire list of rooms to search.

A deep understanding of recursion and iteration isn’t necessary to  comprehend the difference between recursive and iterative DNS lookups:  In a recursive lookup, a DNS server does the recursion and continues  querying other DNS servers until it has an IP address to return to the  client (often a user’s operating system). In an iterative DNS query,  each DNS query responds directly to the client with an address for  another DNS server to ask, and the client continues querying DNS servers until one of them responds with the correct IP address for the given  domain.

Put another way, the client does a form of delegation in a recursive  DNS query. It tells the DNS resolver, “Hey, I need the IP address for  this domain, please hunt it down and don’t get back to me until you have it.” Meanwhile, in an iterative query, the client tells the DNS  resolver, “Hey, I need the IP address for this domain. Please let me  know the address of the next DNS server in the lookup process so I can  look it up myself.”

## What are the advantages of recursive DNS?

Recursive DNS queries generally tend to resolve faster than iterative queries. This is due to caching. A recursive DNS server caches the  final answer to every query it performs and saves that final answer for a certain amount of time (known as the [Time-To-Live](https://www.cloudflare.com/learning/cdn/glossary/time-to-live-ttl/)).

When a recursive resolver receives a query for an IP address it  already has in its cache, it can rapidly provide the cached answer to  the client without communicating with any other DNS servers. Quickly  serving responses from the cache is very likely if a) the DNS server  serves a lot of clients and/or b) the requested website is very popular.

## What are the disadvantages of recursive DNS?

Unfortunately, allowing recursive DNS queries on open DNS servers  creates a security vulnerability, as this  configuration can enable  attackers to perform [DNS amplification attacks](https://www.cloudflare.com/learning/ddos/dns-amplification-ddos-attack/) and [DNS cache poisoning](https://www.cloudflare.com/learning/dns/dns-cache-poisoning/).

#### Recursive DNS servers and DNS amplification attacks

In a DNS amplification attack, an attacker typically uses a group of machines (known as a [botnet](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-botnet/)) to send a high volume of DNS queries using a [spoofed IP](https://www.cloudflare.com/learning/ddos/glossary/ip-spoofing/) address. A spoofed IP address is like a forged return address; the  attacker is sending requests from their own IP, but asking for the  responses to go to the victim. In order to exacerbate the attack, the  attacker also uses a technique called amplification, in which the  spoofed request asks for a very long response. The victimized service  will receive a flood of lengthy and unwanted DNS responses that can  disrupt or even take down their servers. This is a type of [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/).

This is kind of like a group of teenage pranksters calling a pizza  place and each ordering a dozen pizzas. Instead of giving their own  address for delivery, they give the address of an unsuspecting neighbor.  The victim, who then receives a stream of large and unwanted pizza  deliveries, will likely experience a lot of disruption in their day.

A DNS server that accepts recursive queries is needed to carry out  this kind of attack, because the amplified DNS packets are responses to  recursive DNS queries.

#### Recursive DNS servers and DNS cache poisoning attacks

In a DNS cache poisoning attack, when a recursive DNS server requests an IP address from another DNS server, an attacker intercepts the  request and gives a fake response, which is often the IP address for a  malicious website. Not only does the recursive DNS server send the  original client this IP address, but the server will also save the  response in its cache. Any  user that requests an IP for the same domain name will be sent to the malicious website. If it’s a popular domain  name and a popular DNS resolver, this attack could affect thousands of  users.

In an iterative DNS query, the client directly asks each DNS server  for the answer.  Even  if an attacker is able to send a forged response  to the query, it will only affect a single client, which is generally  not worth the attacker’s time.

## How to support DNS queries that are both fast and secure

[Cloudflare’s managed DNS](https://www.cloudflare.com/dns/) service generally resolves DNS queries faster than competitors' DNS or  public DNS. Additionally, Cloudflare DNS supports DNSSEC, a stringent  security protocol designed to keep DNS servers and queries safe.



---

---

