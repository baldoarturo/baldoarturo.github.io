---
id: 117
title: Spiceworks Customization
date: 2018-07-18T08:50:49-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=117
permalink: /spiceworks-customization/
categories:
  - Projects
tags:
  - apache
  - spiceworks
  - topland
  - upwork
---
 

Andrew Foster at <a rel="noreferrer noopener" aria-label="Topland Communications (opens in a new tab)" href="https://topland.org/about-topland/" target="_blank">Topland Communications</a> reached me via [Upwork](https://www.upwork.com/freelancers/~01fc71d4c00ed1c159) looking to customize and fine tune a existing Spiceworks installation.

After a quick inspection, I decided to tackle the project by compacting the DB in first place. Spiceworks keeps a lot of logs regarding the system activity, which are located on _C:\Program Files\Spiceworks\Log.&nbsp;_In order to clean them, the first step is to stop Spiceworks service.

Logs are stored in two main locations:

  * _C:\Program Files\Spiceworks\Log_, for the Spiceworks service
  * _C:\Program Files\Spiceworks\httpd\log\, _where the Apache server keeps them

Once the logs are cleaned, I compacted the DB to increase the performance, and I started the service again.

Ticket rules were configured to auto assign support tickets, thus saving time to the support operators.<figure class="wp-block-image">

<img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-1.png?resize=640%2C339&#038;ssl=1" alt="" class="wp-image-118" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-1.png?resize=1024%2C542&ssl=1 1024w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-1.png?resize=300%2C159&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-1.png?resize=768%2C406&ssl=1 768w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-1.png?w=1172&ssl=1 1172w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure> 

And the user portal was customized to match the company colors and logo.<figure class="wp-block-image">

<img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-2.png?resize=640%2C700&#038;ssl=1" alt="" class="wp-image-119" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-2.png?resize=936%2C1024&ssl=1 936w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-2.png?resize=274%2C300&ssl=1 274w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-2.png?resize=768%2C840&ssl=1 768w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-2.png?w=960&ssl=1 960w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure>