---
id: 354
title: Using The Dude on MikroTik CHR
date: 2019-06-04T13:52:37-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=354
permalink: /using-the-dude-on-mikrotik-chr/
categories:
  - MikroTik
  - Networking
  - Projects
---
The Dude network monitor is RouterOS package intended to manage a network environment. It automatically scan all devices within specified subnets, draw and layout a network maps, monitor services, and alert you in case of problems.

Previous versions of _The Dude_ were developed as Windows x86 software, but later versions went through a full rebuild, and now it is distributed as a RouterOS package. This comes handy as the same RouterOS instance can be linked to the network, eliminating the the need for additional VPNs on servers or gateways. Instead, all tunneling can be done inside the CHR instance.

The Windows versions also had a web GUI which was, awful. For all the new editions, you&#8217;ll need a software client available on <https://download.mikrotik.com/routeros/6.43.14/dude-install-6.43.14.exe>

It will update itself whenever you connect to a newer RouterOS version. Just be sure to run it as administrator on W10.

#### Installing

Get the CHR package from [https://download.mikrotik.com/routeros/6.43.14/dude-6.43.14.npk.](https://download.mikrotik.com/routeros/6.43.14/dude-6.43.14.npk)

Once downloaded, upload it to the CHR instance via Winbox drag-and-drop, FTP client, or just download it from inside chr:<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-28.png?w=640&#038;ssl=1" alt="" class="wp-image-355" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-28.png?w=831&ssl=1 831w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-28.png?resize=300%2C157&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-28.png?resize=768%2C401&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption>Downloading from CHR  
</figcaption></figure> 

Reboot the CHR instance, and you will find the new _Dude_ menu inside Winbox.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-29.png?w=640&#038;ssl=1" alt="" class="wp-image-383" srcset="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-29.png?w=489&ssl=1 489w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-29.png?resize=300%2C203&ssl=1 300w" sizes="(max-width: 489px) 100vw, 489px" data-recalc-dims="1" /><figcaption>New <em>Dude</em> menu</figcaption></figure>
</div>

Head to _Dude > Settings_ and tick _Enabled_ to enable the server. A few folders will be created on the filesystem, and the server will be ready to accept connections on port 8291. The previous x86 based versions of Dude used port TCP/2210 or TCP/2211, but on this new integrated RouterOS package, all the management is handled on the same port as Winbox.

If you still don&#8217;t have the client, get it on <https://download.mikrotik.com/routeros/6.43.14/dude-install-6.43.14.exe>.

One you connect, the following window should appear by default. You can run a discover for multiple networks and let Dude map your network for you, but it will only disconver layer 3 adyancencies. In order to have complete control over the monitoring, I suggest to build your backbone manually and let the autodiscovery handle your management VLANs/VRFs.<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-31.png?w=640&#038;ssl=1" alt="" class="wp-image-385" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-31.png?w=761&ssl=1 761w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-31.png?resize=300%2C206&ssl=1 300w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure>