---
id: 438
title: Building a
date: 2019-06-18T15:42:41-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/436-revision-v1/
permalink: /436-revision-v1/
---
It is always nice to fly.<figure class="wp-block-image">

![](https://lh3.googleusercontent.com/rp4Ha3WgAkt8ObfQQ-0YqAGhPhzTD0hbGbO8LsEyaA_CmM99aqTK5yYS8ufIfiH-TDOnEoeN80BPDEl_UZ1DYYaH6CzS3w-o1NIzKnbjZsj2wiJgUMfgivQmgQDziXWCc4MEmlwhe4si1h5jvcDCX28csWrkvi5rgJzy5TlHZINJyIvq_9oZf4RkIPFM_t5kkMRpPDQ2jgy02-ro74b1WLgtBEH52R3nddNR0-qiTVVwN2C1FnswnFIYswVyurWhG3gnN-th0Ov-RyHTlQXsIpJUauIhncryjgURs-gJ36k_UCEgihnNQFHPGg0a_G9V1LDbpD3OC36PSeAZvxu53Rv0FetLGfWk93oT0NZDlyB7p4n_fey01b4i0EpZhJord832UZO3C7Ot80H20609wPLqb7kqC7WCLrmwdAq36691J_-oiPy12TPtpS8Qny5OFL4aVpis5mOaGnGAEVRP7Q_D_MLdMmZ71BV2ttzkjj_ZVkmPAVMeIuErmLQQsum7W1n8xLXsNqc_X-Q-qSWKwtiZtYxE0nlteSV3NT7Fw0G9d9NdOyHdz5jIFvGnaKU0EBs3JW16MPLs88cAhyCfkXoHDIJKIWU1SuG6o3o87qxPGmnd4vGypDhMbSiwKKzUQB_usTBeUOd669l2mLM75AG1xsQTBVC_=w1198-h898-no) <figcaption>Travelling MDZ to AEP</figcaption></figure> 

My current company is establishing operations on Entre Rios province, where a we are initiating a brand-new ISP service for the towns of Crespo, Libertador San Martin, and Paraná City.

Service is provided with two providers, and BGP sessions must be established with both to announce a /24 prefix of our AS, and probably receive just a default route from the upstream. There is no need to use the full table- yet. Both providers has approximately the same AS-PATH.

We&#8217;ll use a MikroTik CCR1036-8G-2S+ as the border router. Altough it has SFP+ ports to allow 10 Gbps operation, at the moment the links will be negotiated using SFP modules at 1 Gbps.