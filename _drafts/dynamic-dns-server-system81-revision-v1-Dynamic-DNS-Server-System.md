---
id: 101
title: Dynamic DNS Server System
date: 2018-12-30T13:55:53-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/81-revision-v1/
permalink: /81-revision-v1/
---
After a couple of successful jobs with my client [Visual Link Internet LLC](https://www.visuallink.com/), they reached me to set up a service similar to _dyndns.net_. I had already developed another value added services for their customers, like web filtering and firewalling, so I found this project very interesting and fun to do.

_Cool, but what is DNS ?_

DNS stands for Domain Name Systems. Yep, domains like google.com.

It is based on a distributed database that takes some time to update globally. When DNS was first introduced, the database was small and could be easily maintained by hand. As the system grew this task became difficult for any one site to handle, and a new management structure was introduced to spread out the updates among many domain name registrars.

Due to the distributed nature of the DNS systems and its registrars, updates to the global DNS system may take hours to distribute. Thus DNS is only suitable for services that do not change their IP address very often, but not for servers being run with dynamic addresses, which are likely to change their IP address over very short periods of time.

_Ok, but my ISP gives dynamic addresses, and I want to access services on my network. What can i do?_

Dynamic DNS is a system that addresses the problem of rapid updates. The term is used in two ways, which, while technically similar, have very different purposes and user populations. The first is &#8220;standards-based DNS updates&#8221;, which uses an extension of the DNS protocol to ask for an update. The second is usually a web-based protocol, normally a single HTTP fetch with username and password which then updates some DNS records (by some unspecified method).

Many providers offer commercial or free Dynamic DNS service for this scenario. The automatic reconfiguration is generally implemented in the user&#8217;s router or computer, which runs software to update the DDNS service. The communication between the user&#8217;s equipment and the provider is not standardized, although a few standard web-based methods of updating have emerged over time.

_Yeah, but those free services are now paid, and some have even disappeared_

I know, I know. But this service can be built in-house. Using open source software, there are no fees, and the company domain name can be used to keep things professional.

This is what my client wanted, so I deployed a solution on a that allowed to offer added value services to customers, and provide easy remote access. Using a open source solution based on PHP (<https://github.com/nicokaiser/Dyndns>) and some custom Bash scripts I was able to deliver a stable system in a short amount of time. 

The main techonologies I used are Apache 2 and PHP 7 for the HTTP requests and update system, and BIND9 for the DNS service.<figure class="wp-block-image">

<img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/asd.png?w=640&#038;ssl=1" alt="" class="wp-image-100" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/asd.png?w=773&ssl=1 773w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/asd.png?resize=300%2C88&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/asd.png?resize=768%2C225&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure> 

The solution used the standard URL schema of DynDNS, so it is compatible with any device with support for it. Also, because most CPEs of the client&#8217;s network were MikroTik based, I also wrote a RouterOS script to call the update.