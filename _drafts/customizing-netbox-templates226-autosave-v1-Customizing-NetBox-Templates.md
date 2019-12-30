---
id: 260
title: Customizing NetBox Templates
date: 2018-12-31T13:39:23-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/226-autosave-v1/
permalink: /226-autosave-v1/
---
[NetBox](https://arturobaldo.com.ar/running-netbox-in-docker/) is an IP address management (IPAM) and data center infrastructure management (DCIM) tool. Initially conceived by the network engineering team at DigitalOcean, NetBox was developed specifically to address the needs of network and infrastructure engineers. <figure class="wp-block-image">

<img src="https://i1.wp.com/media.packetlife.net/media/blog/attachments/723/netbox2.png?w=640" alt="Image result for netbox device types" data-recalc-dims="1" /> </figure> 

When I started using <span class="wsc-spelling-problem" data-spelling-word="NetBox" data-wsc-lang="en_US">NetBox</span> on my daily job, I planned to use it as a replacement for all the spreadsheets I had for switch configurations, IP address management, secrets, and VLAN assignments. <span class="wsc-spelling-problem" data-spelling-word="NetBox" data-wsc-lang="en_US">NetBox</span> can handle all of this and more, but the interface didn&#8217;t suit my needs.

<span class="wsc-spelling-problem" data-spelling-word="NetBox" data-wsc-lang="en_US">NetBox</span> is built using the Python <span class="wsc-spelling-problem" data-spelling-word="Django" data-wsc-lang="en_US">Django</span> framework, which I have used for another projects. I used Visual Studio Code to clone the repository and debug, as it has native support for the <span class="wsc-spelling-problem" data-spelling-word="Django" data-wsc-lang="en_US">Django</span> template language. 

I keep a copy of the repository on my local machine for ease of modifications. Prior, I have set _DEBUG=TRUE_ on <span class="wsc-spelling-problem" data-spelling-word="netbox" data-wsc-lang="en_US"><em>netbox</em></span>_/configuration.py_, and allowed <span class="wsc-spelling-problem" data-spelling-word="localhost" data-wsc-lang="en_US"><em>localhost</em></span> __and my local network to access the development server. Also, I set the correct settings to connect to the existing <span class="wsc-spelling-problem" data-spelling-word="postgresql" data-wsc-lang="en_US">postgresql</span> database. <figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-10.png?w=640&#038;ssl=1" alt="" class="wp-image-265" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-10.png?w=983&ssl=1 983w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-10.png?resize=300%2C117&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-10.png?resize=768%2C300&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption>Connecting the existing DB to my local development server</figcaption></figure> 

This environment works for test purposes, but the best you can do is to set up separated development and production environments, and commit your changes to production once everything is tested.<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-5.png?resize=640%2C350&#038;ssl=1" alt="" class="wp-image-258" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-5.png?resize=1024%2C560&ssl=1 1024w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-5.png?resize=300%2C164&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-5.png?resize=768%2C420&ssl=1 768w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-5.png?w=1920&ssl=1 1920w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-5.png?w=1280&ssl=1 1280w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption>Using <span class="wsc-spelling-problem" data-spelling-word="VSCode" data-wsc-lang="en_US">VSCode</span> to debug Django</figcaption></figure> 

The URL definition for the single device view is around line #147 of the <span class="wsc-spelling-problem" data-spelling-word="netbox" data-wsc-lang="en_US"><em>netbox</em></span>_/_<span class="wsc-spelling-problem" data-spelling-word="dcim" data-wsc-lang="en_US"><em>dcim</em></span>_/urls.py_ file, and it looks like this.

<pre class="wp-block-code"><code> url(r'^devices/(?P&lt;pk>\d+)/$', views.DeviceView.as_view(), name='device'),</code></pre>

Heading to the <span class="wsc-spelling-problem" data-spelling-word="DeviceView" data-wsc-lang="en_US"><em>DeviceView</em></span> view, I put a breakpoint on the interfaces  
QuerySet of the view definition, and launched the debugger. The default location is at http://<span class="wsc-spelling-problem" data-spelling-word="localhost" data-wsc-lang="en_US">localhost</span>:8000.<figure class="wp-block-image">

<img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-6.png?resize=640%2C350&#038;ssl=1" alt="" class="wp-image-261" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-6.png?resize=1024%2C560&ssl=1 1024w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-6.png?resize=300%2C164&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-6.png?resize=768%2C420&ssl=1 768w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-6.png?w=1920&ssl=1 1920w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-6.png?w=1280&ssl=1 1280w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption>Setting up the debugger</figcaption></figure> <figure class="wp-block-image"><img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-8.png?resize=640%2C350&#038;ssl=1" alt="" class="wp-image-263" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-8.png?resize=1024%2C560&ssl=1 1024w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-8.png?resize=300%2C164&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-8.png?resize=768%2C420&ssl=1 768w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-8.png?w=1920&ssl=1 1920w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-8.png?w=1280&ssl=1 1280w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>Breakpoints</figcaption></figure> 

I headed to http://<span class="wsc-spelling-problem" data-spelling-word="localhost" data-wsc-lang="en_US">localhost</span>:8000/<span class="wsc-spelling-problem" data-spelling-word="dcim" data-wsc-lang="en_US">dcim</span>/devices/570/, where I had defined a switch with several <span class="wsc-spelling-problem" data-spelling-word="VLANs" data-wsc-lang="en_US">VLANs</span>, to hit the breakpoint and find out if the  
QuerySet had information about the <span class="wsc-spelling-problem" data-spelling-word="VLANs" data-wsc-lang="en_US">VLANs</span>, or if they were queried in a per-interface basis, on the interface view.<figure class="wp-block-image">

<img src="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-9.png?resize=640%2C350&#038;ssl=1" alt="" class="wp-image-264" srcset="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-9.png?resize=1024%2C560&ssl=1 1024w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-9.png?resize=300%2C164&ssl=1 300w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-9.png?resize=768%2C420&ssl=1 768w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-9.png?w=1920&ssl=1 1920w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-9.png?w=1280&ssl=1 1280w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption><span class="wsc-spelling-problem" data-spelling-word="QuerySet" data-wsc-lang="en_US">QuerySet</span> returns this</figcaption></figure> 

Lucky me, the QuerySet recovered all the information I needed, and it is passed to the template via a _render() _call.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-11.png?w=640&#038;ssl=1" alt="" class="wp-image-266" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-11.png?w=492&ssl=1 492w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-11.png?resize=300%2C208&ssl=1 300w" sizes="(max-width: 492px) 100vw, 492px" data-recalc-dims="1" /></figure>
</div>

All the information I want is rendered on this table. This is the power of the <span class="wsc-spelling-problem" data-spelling-word="Django" data-wsc-lang="en_US">Django</span> framework. I added line #513 as an additional header for the <span class="wsc-spelling-problem" data-spelling-word="VLANs" data-wsc-lang="en_US"><em>VLANs</em></span> column.<figure class="wp-block-image">

<img src="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-12.png?w=640&#038;ssl=1" alt="" class="wp-image-267" srcset="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-12.png?w=954&ssl=1 954w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-12.png?resize=300%2C140&ssl=1 300w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-12.png?resize=768%2C357&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure> 

This table has a for loop which iterates for each interface of the device, so I edited the included template file at <span class="wsc-spelling-problem" data-spelling-word="dcim" data-wsc-lang="en_US"><em>dcim</em></span>_/_<span class="wsc-spelling-problem" data-spelling-word="inc" data-wsc-lang="en_US"><em>inc</em></span>_/interface.html._

Both tagged and untagged <span class="wsc-spelling-problem" data-spelling-word="VLANs" data-wsc-lang="en_US">VLANs</span> groups have a bolded title, and the VID and VLAN name is shown after it. I used the <span class="wsc-spelling-problem" data-spelling-word="dictsort" data-wsc-lang="en_US">dictsort</span> filter, which is part of the <span class="wsc-spelling-problem" data-spelling-word="Django" data-wsc-lang="en_US">Django</span> framework, to sort all the <span class="wsc-spelling-problem" data-spelling-word="VLANs" data-wsc-lang="en_US">VLANs</span> by their VID.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-14.png?w=640&#038;ssl=1" alt="" class="wp-image-269" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-14.png?w=624&ssl=1 624w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-14.png?resize=300%2C204&ssl=1 300w" sizes="(max-width: 624px) 100vw, 624px" data-recalc-dims="1" /></figure>
</div>