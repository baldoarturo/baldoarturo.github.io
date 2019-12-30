---
id: 169
title: Running NetBox in Docker
date: 2018-09-06T10:50:13-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=169
permalink: /running-netbox-in-docker/
categories:
  - Projects
tags:
  - docker
  - netbox
  - portainer
  - traefik
---
 

[NetBox](https://github.com/digitalocean/netbox) is an IP address management (IPAM) and data center infrastructure management (DCIM) tool. Initially conceived by the network engineering team at <span class="wsc-spelling-problem" data-spelling-word="DigitalOcean" data-wsc-lang="en_US">DigitalOcean</span>, <span class="wsc-spelling-problem" data-spelling-word="NetBox" data-wsc-lang="en_US">NetBox</span> was developed specifically to address the needs of network and infrastructure engineers.

A quick way to get it working is to use the Docker stack provided at [https://github.com/](https://github.com/ninech/netbox-docker)<span class="wsc-spelling-problem" data-spelling-word="ninech" data-wsc-lang="en_US"><a href="https://github.com/ninech/netbox-docker">ninech</a></span>[/netbox-docker](https://github.com/ninech/netbox-docker). 

<p class="has-medium-font-size">
  <strong>Installing</strong>
</p>

First, I cloned the repository.

<pre class="wp-block-code"><code>$ git clone -b master https://github.com/ninech/netbox-docker.git
$ cd netbox-docker</code></pre>

Once cloned, I used docker-compose to pull the images

<pre class="wp-block-code"><code>$ docker-compose pull</code></pre>

And then I started the stack with

<pre class="wp-block-code"><code>$ docker-compose up -d</code></pre>

The service will be up and running after a few minutes. Once ready, you need to find where to connect to with

<pre class="wp-block-code"><code>$ docker-compose port nginx 8080</code></pre>

Or use this snippet

<pre class="wp-block-code"><code>$ echo "http://$(docker-compose port nginx 8080)/"</code></pre>

Here I use <span class="wsc-spelling-problem" data-spelling-word="Portainer" data-wsc-lang="en_US">Portainer</span> as a <span class="wsc-spelling-problem" data-spelling-word="gui" data-wsc-lang="en_US">gui</span> to manage Docker, and <span class="wsc-spelling-problem" data-spelling-word="Traefik" data-wsc-lang="en_US">Traefik</span> as a reverse proxy to enable FQDN access to the services behind. I added an entry on my DNS to route <span class="wsc-spelling-problem" data-spelling-word="netbox" data-wsc-lang="en_US">netbox</span>.arturo.local to the Docker IP address, on the exposed port for <span class="wsc-spelling-problem" data-spelling-word="Nginx" data-wsc-lang="en_US">Nginx</span>.<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-4.png?resize=640%2C350&#038;ssl=1" alt="" class="wp-image-221" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-4.png?resize=1024%2C560&ssl=1 1024w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-4.png?resize=300%2C164&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-4.png?resize=768%2C420&ssl=1 768w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-4.png?w=1920&ssl=1 1920w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-4.png?w=1280&ssl=1 1280w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure>