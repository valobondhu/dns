[Dns split horizon configure](https://www.slashroot.in/how-to-configure-split-horizon-dns-in-bind)



The general Idea of DNS is to provide an IP address in return for a requested name. The normal regular and  common configuration is to respond with the same answer no matter from  where the request originated. 

 

However, there are requirements and use cases, where you need to configure your  DNS server with different set of addresses for the same domain name.  Lets take an example to understand this. Let's say you have a DNS server and wants to use the same dns server for requests from internet as well as for requests from the private network. It does not make sense to  respond back with the same answer for both the types. Because your  internal private network users should access your services through the  private LAN ip addresses itslef(which will be much better in terms of  performance as well as latency) rather than accessing through the Public facing internet address. 

 

If you are completely new to DNS and its working concepts, i recommend to  go through the below posts to understand the basics first, before going  ahead with this article.

**Read:** [How does a DNS server work?](http://www.slashroot.in/how-dns-works)

**Read:** [Difference between Iterative and Recursive DNS Servers](http://www.slashroot.in/difference-between-iterative-and-recursive-dns-query)

**Read:** [Understanding DNS Root Servers and its Role in the Hierarchy](http://www.slashroot.in/dns-root-servers-most-critical-infrastructure-internet)

**Read:** [DNS ](http://www.slashroot.in/what-dns-zone-file-complete-tutorial-zone-file-and-its-contents)[Zone file Explained](http://www.slashroot.in/what-dns-zone-file-complete-tutorial-zone-file-and-its-contents)

 

> Split horizon DNS is also known as "Split DNS" OR "Split View DNS" etc. Do  not confuse Split horizon DNS with the same term used in networking. 
>
> In the field of Networking, the same term(Split Horizon), is used for a  method that prevents the forming of loops in routing advertisement. 

 

There are different methods to implement this split view or split horizon DNS. Some of them are mentioned below.

 

- The first method is to simply place different DNS servers in the required  networks with different configurations applicable to those network.
- The second method is to run multiple DNS services in the same server. 
- The third one is the built in capability of a DNS server software, and implement this feature in the DNS configuration itself.

 

In this article, we will be concentrating on the third technique from the  above listed ones. We will be using BIND DNS server to demonstrate this  example configuration. The first step is to install bind.

 

**Step 1: Install Bind on your system**

Installing bind is as simple as running an apt-get (if you are on Ubuntu or  debian) or Yum (if you are on RPM based Centos of RHEL) command. Let's  get straight to the installation. 

In Ubuntu, you can run the below command to install bind..

 

```
# apt-get install bind9
```

 

In case of Centos or RHEL, you can install bind using the below yum command.

 

[?](https://www.slashroot.in/how-to-configure-split-horizon-dns-in-bind#)

```
# yum install bind*
```

 

The main primary configuration  file for bind is /etc/bind/named.conf. Generally people do not alter  this file much, but includes pointers to other files that bind needs to  check for configurations. Let's see a typical named.conf file..

 

```
root@localhost:~``# cat /etc/bind/named.conf``include ``"/etc/bind/named.conf.options"``;``include ``"/etc/bind/named.conf.local"``;``include ``"/etc/bind/named.conf.default-zones"``;
```

 

The above shown settings in  named.conf just says bind to look in the files  /etc/bind/named.conf.options, /etc/bind/named.conf.local,  /etc/bind/named.conf.default-zones.



​       

 

/etc/bind/named.conf.options file contains the general option set for bind. Things like the below are mentioned in that file..

- Listen on port and interface
- Allow Query
- Allow transfer to source addresses
- Allow recursion or not
- PID file location 
- Allowed list of IP's to query this DNS server
- Enable/Disable DNSSEC for zone transfer.
- Cache Location...etc

 

[?](https://www.slashroot.in/how-to-configure-split-horizon-dns-in-bind#)

```
root@localhost:~``# cat /etc/bind/named.conf.options``options {``    ``listen-on port 53 { 10.1.136.154; 127.0.0.1;};``    ``directory    ``"/var/cache/bind/"``;``    ``allow-query   { any;};``    ``recursion no;``    ``pid-``file` `"/run/named/named.pid"``;``    ``notify ``yes``;``};
```

 

> The most important  feature of bind that enables this split-view or split horizon  configuration is called "views". Views in bind will let you host a set  of configuration for some, and another set of configuration for others.
>
> The main use case of this is when you want to use the same DNS for internal clients as well as for external ones. 
>
> Well technically, even if you do not configure any views...bind will create a view and that view is internally served to all queries from all  sources.

 

Views in bind is created using the view clause as shown below. 

 

```
view ``"testview"` `{``};
```

 

Views sections are organized beneath the options section that we saw earlier. So let's keep our views in the file /etc/bind/named.conf.options, which we included in named.conf already.

There are two methods to add source addresses that are applicable to your view. The first method is using the statement called "*match-clients*". *match-clients* requires a source address list as argument. An example *view* statement with the *match-clients* substatement is shown below.

 

```
view ``"testview"` `{``    ``match-clients { 10.1.12.0``/24``; };``};
```

 

The second method is to use *acl* statements. Let's see an example *acl* statement and how it can be used inside *views* statement in bind.

 

```
acl ``"testacl"` `{ 10.1.12.0``/24``; };``view ``"testview"` `{``  ``match-clients { ``"testacl"``; };``};
```

 

> Just keep the fact in mind that you need to define acl statement outside of the views statement.

 

## What all options can you place inside the bind's view statement?

You can place almost everything that the *options* statement supports inside the *views* statement. You can define things like recursion, type(ie: whether to act as master or slave), zones, signatures etc.

 

```
acl ``"testacl"` `{ 10.1.12.0``/24``; };``view ``"testview"` `{``  ``match-clients { ``"testacl"``; };``  ``zone ``"test.example.com"` `{``     ``type` `master;``     ``file` `"test.exmple.com.zone"``;``  ``};``};
```

 

> Another perfect use case of views is to allow recursion for only our internal clients and  deny recursion for external clients. This can be easily ahieved using  views. This is because *views* supports *recursion* option to be specified(using which you can allow or deny recursive dns query to a particular source address)
>
> Also *match-clients* statement supports a wild card entry called *any,* which is really usefull in making views that applies to everybody. Like the below shown example..

 

```
acl ``"testacl"` `{ 10.1.12.0``/24``; };``view ``"testview"` `{``  ``match-clients { ``"testacl"``; };``  ``recursion ``yes``;``  ``zone ``"test.example.com"` `{``    ``type` `master;``    ``file` `"test.example.com.trusted.zone"``;``  ``};``};``view ``"outside"` `{``  ``match-clients { any; };``  ``recursion no;``  ``zone ``"test.example.com"` `{``     ``type` `master;``     ``file` `"test.example.com.untrusted.zone"``;``  ``};``};
```

> Its a common security  measure to only allow recursion to known clients. This is because  recursion poses a lot of security risks. The above show bind's view  clause can be used to deny recursion to all anonymous requests, while  allowing it only for our selective views(*allow-recursion* statement can also be used for the same purpose). 