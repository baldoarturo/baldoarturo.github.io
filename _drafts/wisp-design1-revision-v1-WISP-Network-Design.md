---
id: 107
title: WISP Network Design
date: 2018-12-30T13:58:17-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/1-revision-v1/
permalink: /1-revision-v1/
---
An Upwork client reached me seeking for a set of suggestion and a brief desing for a brand new WISP network on Wisconsin, USA.

The milestones were:

  * Feasibility calculation of Wireless Links
  * Recommendation of devices
  * Analysis of the the network topology and re-engineering<figure class="wp-block-image">

![Logical Topology](https://www.upwork.com/att/download/portfolio/persons/uid/585797785131323392/profile/projects/files/607601391354241024) </figure> 

Two local ISPs offered dark fiber and MPLS circuits to estabilish the network backbone, but the client declined the offers considering the contract timespan and cost of the lease. An additional link budget was needed, and after considering several vendors, the backbone was built using Ubiquiti Airfiber 24HD radios, which allowed to pass around 500 Mbps on the best conditions, and even 350 Mbps under heavy rain.

Other vendors had backbone solutions on lower frequency bands, but they require a licence to operate, so the non-licensed 24 GHz band was the best selection to avoid further costs.<figure class="wp-block-image">

<img src="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/Topology-Paul-Osterman-1.png?w=640&#038;ssl=1" alt="" class="wp-image-105" srcset="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/Topology-Paul-Osterman-1.png?w=807&ssl=1 807w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/Topology-Paul-Osterman-1.png?resize=300%2C294&ssl=1 300w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/Topology-Paul-Osterman-1.png?resize=768%2C752&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure> 

The last mile was operated using WiMAX gear from Telrad, which gave us a great support and assistance on the initial deployment. The main reasons to select this technology instead of other wireless solutions was the ability to use CPEs that didn&#8217;t require direct line of sight, for indoor use, with limited capabilities, and a set of higher-end outdoor radios with more advanced features.<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/20150204-03-propellant-media-Towers-And-Links.png?resize=640%2C385&#038;ssl=1" alt="" class="wp-image-106" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/20150204-03-propellant-media-Towers-And-Links.png?resize=1024%2C616&ssl=1 1024w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/20150204-03-propellant-media-Towers-And-Links.png?resize=300%2C180&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/20150204-03-propellant-media-Towers-And-Links.png?resize=768%2C462&ssl=1 768w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/20150204-03-propellant-media-Towers-And-Links.png?w=1112&ssl=1 1112w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure>