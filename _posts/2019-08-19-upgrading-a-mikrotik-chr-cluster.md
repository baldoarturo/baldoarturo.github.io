---
id: 461
title: Upgrading a MikroTik CHR Cluster
date: 2019-08-19T18:39:06-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=461
permalink: /upgrading-a-mikrotik-chr-cluster/
categories:
  - MikroTik
  - Networking
  - Projects
tags:
  - chr
  - cluster
  - intel
  - networking
  - sfp
  - vmware
---
I upgraded a CHR cluster with the main objectives of reduce costs, improve network redundancy and provide an easy administration for CHR instances. As explained in previous posts, CHR can be run on many popular hypervisors, and most users are having great success using Hyper-V Failover clusters or vSphere HA to provide highly available routers without depending on VRRP or other gateway redundancy protocols.

These virtual routers currently provide two main services besides routing for ISP customers. They act as PPPoE concentrator for FTTH users, and provide traffic shaping and policing depending on the customer service plan. 

#### Server Hardware

For this node, I will use a 32 core Dell R730, with 32 GB of RAM, and a 500 GB RAID 10 storage. On future post, new hosts will be added to the cluster.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-33.png?w=640&#038;ssl=1" alt="" class="wp-image-467" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-33.png?w=700&ssl=1 700w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-33.png?resize=300%2C233&ssl=1 300w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>Unracking the server</figcaption></figure>
</div>

#### Network Conectivity

This server comes with a 4 port Gigabit Ethernet NIC, which could be used without any issues with the _[ixgbe](https://www.intel.com/content/www/us/en/support/articles/000005688/network-and-i-o/ethernet-products.html)_ driver. 

First idea was to use two ports in a LACP bundle, and the other two in separate port groups. 

I had previous Netflow analysis where I saw a predictable traffic behavior, where most of the bandwidth usage was going from and to a CDN peer of the ISP network. Customers had a mix of public and private addresses of the Class B segment, and they were being moved to CG-NAT ranges. In other words, traffic from a specific set of addresses were going from and to a specific set of addresses.

Why not configure two port-channels, instead of using separate port groups? I tested and due to the nature of the IP addressing on the customer side of the routers, none of the available hashing modes for LACP allowed to achieve a decent distribution on both links of the port-channel. 

So, for the purposes of this cluster, I added an _[Intel X520](https://www.intel.la/content/www/xl/es/ethernet-products/converged-network-adapters/ethernet-x520-server-adapters-brief.html)_ dual SFP+ card, providing 20 Gbps conectivity to the CHR instances. Peak bandwidth usage was around 4200 Mbps, so this card is more than enough to allow for future grow.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?w=640&#038;ssl=1" alt="" class="wp-image-465" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?w=974&ssl=1 974w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?resize=300%2C219&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/imageedit_6_3219751644.jpg?resize=768%2C561&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>Installing the Intel X520 NIC</figcaption></figure>
</div>

The Intel X520 only supports Intel branded SFP modules, and this behavior can be tuned configuring the kernel module. However, for this particular scenario, where both ports will be connected to a top of rack [Dell Force10 S4048-ON](https://www.dell.com/en-us/work/shop/povw/networking-s-series-10gbe) switch, I choosed to use DAC cables to keep things simple.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-34.png?w=640&#038;ssl=1" alt="" class="wp-image-468" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-34.png?w=583&ssl=1 583w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-34.png?resize=300%2C244&ssl=1 300w" sizes="(max-width: 583px) 100vw, 583px" data-recalc-dims="1" /><figcaption>DAC cables on the switch</figcaption></figure>
</div>

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/e597f6d2-756f-40bd-9844-9f49f48105bd.jpg?fit=576%2C1024&ssl=1" alt="" class="wp-image-469" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/e597f6d2-756f-40bd-9844-9f49f48105bd.jpg?w=585&ssl=1 585w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/e597f6d2-756f-40bd-9844-9f49f48105bd.jpg?resize=169%2C300&ssl=1 169w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/e597f6d2-756f-40bd-9844-9f49f48105bd.jpg?resize=576%2C1024&ssl=1 576w" sizes="(max-width: 585px) 100vw, 585px" /><figcaption>Connecting the server</figcaption></figure>
</div>

The server is using ESXi 6.5 for the hypervisor. After booting, I noticed the NICs were being recognized as _vmnic5_ and _vmnic6_, but they were using the _ixgbe_ driver and only establishing links at 1 Gbps.

I downloaded the _ixgben_ driver which is provided by VMware itself [here](https://www.vmware.com/resources/compatibility/detail.php?deviceCategory=io&productid=13444) and uploaded it to ESXi via SFTP. 

For all my SFTP needs, my tool of choice always is [Bitwise SSH client](https://www.bitvise.com/ssh-client-download).

Once uploaded, I installed the **offline bundle** with the following command line. 

<pre class="wp-block-code"><code>[root@esxi] esxcli software vib install -d "/complete/path/to/the/driver/bundle"</code></pre>

Then I followed the [KB article](https://kb.vmware.com/s/article/2147565) to disable the native _ixgbe_ driver and use the new one. First, I placed the host on maintenance mode, and then I executedthe following to disable the driver.

<pre class="wp-block-code"><code>[root@esxi] esxcli system module set --enabled=false --module=ixgbe</code></pre>

After a reboot, the new _ixgben_ driver was loaded, and the NICs were establishing links at 10 Gbps.

I added the new NICs to the previously created virtual switches, checked the correct assignments of port groups, and then migrated the VMs to this host.