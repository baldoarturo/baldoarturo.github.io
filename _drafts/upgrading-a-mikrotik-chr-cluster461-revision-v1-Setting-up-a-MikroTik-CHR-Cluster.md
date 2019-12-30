---
id: 466
title: Setting up a MikroTik CHR Cluster
date: 2019-08-19T18:17:20-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/461-revision-v1/
permalink: /461-revision-v1/
---
I built a new cluster with the main objectives of reduce costs, improve network redundancy and provide an easy administration for CHR instances. As explained in previous posts, CHR can be run on many popular hypervisors, and most users are having great success using Hyper-V Failover clusters or vSphere HA to provide highly available routers without depending on VRRP or other gateway redundancy protocols.

These virtual routers will provide two main services:

  * PPPoE concentrator for FTTH users
  * Traffic shaping and policing

#### Server Hardware

For this node, I will use a 32 core Dell R730, with 32 GB of RAM, and a 500 GB RAID 10 storage. On future post, new hosts will be added to the cluster.

#### Network Conectivity

This server comes with a 4 port Gigabit Ethernet NIC, which could be used without any issues with the _[ixgbe](https://www.intel.com/content/www/us/en/support/articles/000005688/network-and-i-o/ethernet-products.html)_ driver. 

First idea was to use two ports in a LACP bundle, and the other two in separate port groups. 

I had previous Netflow analysis where I saw a predictable traffic behavior, where most of the bandwidth usage was going from and to a CDN peer of the ISP network. Customers had a mix of public and private addresses of the Class B segment, and they were being moved to CG-NAT ranges. In other words, traffic from a specific set of addresses were going from and to a specific set of addresses.

Why not configure two port-channels, instead of using separate port groups? I tested and due to the nature of the IP addressing on the customer side of the routers, none of the available hashing modes for LACP allowed to achieve a decent distribution on both links of the port-channel. 

So, for the purposes of this cluster, I added an _[Intel X520](https://www.intel.la/content/www/xl/es/ethernet-products/converged-network-adapters/ethernet-x520-server-adapters-brief.html)_ dual SFP+ card, providing 20 Gbps conectivity to the CHR instances. Peak bandwidth usage was around 4200 Mbps, so this card is more than enough to allow for future grow.<figure class="wp-block-image">

<img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?w=640&#038;ssl=1" alt="" class="wp-image-465" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?w=974&ssl=1 974w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?resize=300%2C219&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?resize=768%2C561&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption>Installing the Intel X520 NIC</figcaption></figure>