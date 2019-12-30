---
id: 451
title: Building a network on Entre Ríos
date: 2019-06-18T21:41:53-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/436-revision-v1/
permalink: /436-revision-v1/
---
It is always nice to fly. I took two flights, the first one with a little stop at Aeroparque ([AEP](https://www.aa2000.com.ar/aeroparque)), and then a short one to Paraná city ([PRA](https://www.aa2000.com.ar/parana)). The skies were just beautiful.<figure class="wp-block-image">

![](https://lh3.googleusercontent.com/rp4Ha3WgAkt8ObfQQ-0YqAGhPhzTD0hbGbO8LsEyaA_CmM99aqTK5yYS8ufIfiH-TDOnEoeN80BPDEl_UZ1DYYaH6CzS3w-o1NIzKnbjZsj2wiJgUMfgivQmgQDziXWCc4MEmlwhe4si1h5jvcDCX28csWrkvi5rgJzy5TlHZINJyIvq_9oZf4RkIPFM_t5kkMRpPDQ2jgy02-ro74b1WLgtBEH52R3nddNR0-qiTVVwN2C1FnswnFIYswVyurWhG3gnN-th0Ov-RyHTlQXsIpJUauIhncryjgURs-gJ36k_UCEgihnNQFHPGg0a_G9V1LDbpD3OC36PSeAZvxu53Rv0FetLGfWk93oT0NZDlyB7p4n_fey01b4i0EpZhJord832UZO3C7Ot80H20609wPLqb7kqC7WCLrmwdAq36691J_-oiPy12TPtpS8Qny5OFL4aVpis5mOaGnGAEVRP7Q_D_MLdMmZ71BV2ttzkjj_ZVkmPAVMeIuErmLQQsum7W1n8xLXsNqc_X-Q-qSWKwtiZtYxE0nlteSV3NT7Fw0G9d9NdOyHdz5jIFvGnaKU0EBs3JW16MPLs88cAhyCfkXoHDIJKIWU1SuG6o3o87qxPGmnd4vGypDhMbSiwKKzUQB_usTBeUOd669l2mLM75AG1xsQTBVC_=w1198-h898-no) <figcaption>Travelling MDZ to AEP</figcaption></figure> 

My current company is establishing operations on Entre Rios province, where a we are initiating a brand-new ISP service for the towns of [Crespo](https://en.wikipedia.org/wiki/Crespo,_Entre_R%C3%ADos), [Libertador San Martin](https://en.wikipedia.org/wiki/Libertador_San_Mart%C3%ADn), and [Paraná](https://en.wikipedia.org/wiki/Paran%C3%A1,_Entre_R%C3%ADos) City. This was the main task among another small consulting and assistance.<figure class="wp-block-image">

![](https://lh3.googleusercontent.com/DwWysfr7ldGEkO96lxfdRYZefq7DFnuzVbYeTTeSeeNpxAHMAKI1b_LounNa6HOM9ZQOqB6OkW55tdZcq1ZacgiV3EjquQh80l8GYgFVmoa3Hu_3LHr8uyFcJhA5M8NWBm5bfnkMSsZseum83aB32o5EnozdOyJ7eylKKGjvbFx_hiSZP9QaS5wVliYjZwbod-Q3KFxVTS9wsHdHjots69eg8NidZs9C_grvfljuYP6_REQyGjiIV2IpTFlKV2oVJ1sbyGoej-bbrG3z6FJDE3UN32lPpV53j6G8PmzdMpVkXXdVFuVUDF7ajf7QFUs-ooWVS2Lw6i33k4fCALSWzK9k6St4LHfpu7BJrATvbmtnjKpZw5zs6OOFoSgUwOi1v4uXboXtDeeQJpM7OYa5YOcXA5hj3o7KuwhW6dOpaOIQuEnvliVwBGDBswBL-XVwBwASW7RBnGex8EqoXLf8mZ9yLnx7FeoRtxTopNDu9W8ikmRzoEg28Vka7mhj9Tj0-OYuJ_aOhJvN3f_cjVqm31W_OBqCinpwvBUXHC-W5dUt7DuHKMJ1NI8antNfNcPcMeM0exw_EssawTi_lfLYQQnCI9Oa3cNHYtE5cjlecQga6kLf_uX5VbePe2whiI083SiGSrI_Qo3sLSqupPzTIitCQeAMgmY3=w674-h898-no) <figcaption>My first time seeing the mighty Paraná river</figcaption></figure> 

#### Connecting People

Service is provided with two providers, and BGP sessions must be established with both to announce a /24 prefix of our AS, and probably receive just a default route from the upstream. There is no need to use the full table- yet. Both providers has approximately the same AS-PATH.

We&#8217;ll use a [MikroTik CCR1036-8G-2S+](https://mikrotik.com/product/CCR1036-8G-2Splus) as the border router. Although it has SFP+ ports to allow 10 Gbps operation, at the moment the links will be negotiated using SFP modules at 1000 Mbps.

Main customer will be directly connected to this router using copper at 1 Gbps. They are using a [MikroTik CRS326-24G-2S+](https://mikrotik.com/product/CRS326-24G-2SplusRM) for their edge router, which will be enough for their 100 Mbps service. They provide us co-location too, so I installed the core router on their shelter, which is backed up by dual A/C systems and dual UPS-rectifier systems.<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-32.png?w=640&#038;ssl=1" alt="" class="wp-image-440" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-32.png?w=603&ssl=1 603w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-32.png?resize=231%2C300&ssl=1 231w" sizes="(max-width: 603px) 100vw, 603px" data-recalc-dims="1" /> <figcaption>The new router racked and powered up</figcaption></figure> 

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://lh3.googleusercontent.com/ghIpuWo7j8wIGQz-FgrunWSVMGr7YV3by4LTv-BraUSqo2fg8Xjmx7bwItOsbe_2LbdikV61ups2Qv83YPl4u8z7rrSwajeyBmieNDRstteMHAcC9QWdr7xPHuF0i5ZVfu62Mqi08zZk5F1ILlZX08dWlL4FHALc1Qn4VXnjklUVX1O2wuxyRscnE_Mzac88K0KFKvyw5SRL2ZDfLIWSjeu--v2EFBoIH1f6EBMixrONHvYFQUwAuAINdwUvnp_biF1B9IxbjqDXo_4ZOi20cWC2Fd2L8FIzi4SilmA8zbZOL5X0ZSNQLLE1ZbBFsr5usW-jK00NdqhKvNpyg8z7quCJQWAbBGi7ZdaabC8HAU1AtJDB4fyiOPJpsyx-SAyILU7TLubcO6QWxa3PJJNgcCJ_-pYsgnKEDIP3aN-WjaZJgZmBmkf-wuP55YIFM3xkwCsfSqX1E9R-EG1mNkBZtgmTZir5IqJrwqL50i7iO5clOTDylIoa4A6qjHByktgE7nLakqh_jHjac382O-WLxU7XmtCfqP9AcQIaK_vS_43ZrN1duNzd5I7C4l9LO-xXFUqVJb2pCjvvuwokoWznhVRrjwexkMyaWTlCPEIxyteCwleFWr4uW-tHMG7BuWa52WTVe19vL6GGKyFqqsgTE9te9XSkMfab=w506-h898-no" alt="" /><figcaption>We&#8217;ll have some mate while waiting for the upstream provider port to go into <em>no shutdown</em></figcaption></figure>
</div>

Once the upstream was go, I was able to see they were in fact sending us the full BGP table, which we don&#8217;t need yet, so a couple route filters were configured to use put only a default gateway on the _main_ routing table. As the default route was configured as a static one, the route filter policy was as easy as discard all BGP inputs.

<pre class="wp-block-code"><code>[rootmin@ROUTER-EDUC.AR-PARANA] > routing filter export 
# jun/18/2019 16:24:37 by RouterOS 6.42.6
#
/routing filter
add action=discard chain=dynamic-in protocol=bgp</code></pre>

On this site there was also an [Ubiquiti AirFiber 11X](https://www.ui.com/airfiber/airfiber-11fx/) wireless link to reach Libertador San Martín town. Both radios were previously installed but not configured, so I connected to the radio and the site and configured it as _Master._ We traveled to the remote end, configured the radio as _Slave_ and it worked just fine. Ubiquiti is getting up to date with their firmwares and UI, and it has became pretty straight forward to get a link working, even for someone with little or none networking skills. 

¿Do you think this ease-of-use is making the job easier for us, or is it the start point of a madness of wireless spectrum usage?

From this node at Libertador, we installed two single-mode fiber lines, one to connect the town Hospital and another for the town University. [MikroTik CRS326-24G-2S+](https://mikrotik.com/product/CRS326-24G-2SplusRM) switches were installed at each site to be used as CPEs.

All monitoring, reporting and backup systems were previously configured at our NOC, so that was ll for us on the site.

#### Watching cars go by

I also assisted a brand new urban surveillance camera installation on the entrance of the _[Raúl Uranga – Carlos Sylvestre Begnis Subfluvial Tunnel](https://en.wikipedia.org/wiki/Ra%C3%BAl_Uranga_%E2%80%93_Carlos_Sylvestre_Begnis_Subfluvial_Tunnel)_. The objective was to read license plates on this strategic points, which is one of the few exits outside the Paraná river, and the one which has the most vehicle traffic.

Previously we had selected a _[Hikvision DS-2CD4A26FWD-IZHS8/P](https://us.hikvision.com/en/products/cameras/network-camera/smart-series/specialty/license-plate-recognition/2-mp-ultra-low-light-outdoor-lpr-bullet-camera-ds-2cd4a26fwd-izhs8p)_ (yep, that&#8217;s the model name) camera which was already installed by Policía of Entre Ríos technician. This camera was specifically designed for licence-plate recognition (LPR). It supports OCR on hardware and works in very low light conditions, as low as 0.0027 lux.

#### Traces of Paraná City

I stayed at _[Hotel Howard Johnson Plaza Resort & Casino Mayorazgo](https://www.hjmayorazgo.com.ar/wp/)__,_ and I encourage you to visit it. The rooms are lovely and the staff is excellent.<figure class="wp-block-image">

![](https://lh3.googleusercontent.com/pPcsjPaJRjs3MXeKwQLaKOYQ02MnZrdXbTUebV3tO-VowUaSHsojZIe3AV1-8Hw0IAtMQy3IhCfr4jm0ZisA8ZldihmO0USh6LOnu0TkVC9cYNMB9O3StsL3j2jhMyIhY4uB-NzXfb6YvZiqOZMKaSJvdS58vCHq1diJ2iZXbakuPccoY7D7G6sfW0xGXh4fBUd4zD5_Y7YhlwBCWUxE-0Ii7Yljf75Eiabar05zjl9k3luhgoN8OyT2qmWRhSdjAhlFm3Ol0ysMlV7KvtfCF8WmVJ3_AkvWXMpFpFTrYDyVhO82TnhZRzmLagi3RdMnvDSZ0Ii9qjMDMYifPSuZOcqdpVqk6G2Ensw25cRhIkwamKK90DeBf3dHBV559BedoIGS9V3iuZebjD2CXdO65v8shQk39vYNHXMAiUOHOGgfloQnudewSp_nElzPkHetmB0BZI19p_xCqZMqj3GTnl4bx9hv1L0YlHfZWDyIQGEx3HlyQY90yBZtNmxTMDGqvnk3tnQxpCqRCd9P-B-7_C_l2i-MvCvKixjnYc8B1OKOqecuV0h6zLSpQoM5Xf9MA8qgqf1xtB3bf0_yg4Ep8yPOs-lLB0LhcIGmFcVIacr2hVy-aL7Jftmmt6xv6e0DBBOlVMogz-em3MJPppUVzWgDqGoMIKfx=w1597-h899-no) <figcaption>My view from the hotel room</figcaption></figure> 

Be sure to schedule time to walk on the Paraná river borders, [visit](https://www.tripadvisor.com.ar/Attraction_Review-g312800-d5123745-Reviews-Museo_Historico_Provincial_Martiniano_Leguizamon-Parana_Province_of_Entre_Rios_Li.html) [the Martiniano Leguizamón historic town museum](https://es-la.facebook.com/Museohistoricoentreriosmartinianoleguizamon/) and enjoy yourself. This is a beautiful city.

<div class="wp-block-image">
  <figure class="aligncenter"><img src="https://lh3.googleusercontent.com/khQ0CZQBLND9awMeLy2pzwYKxMjHu3TNxX2NbbAq5SqYF9j8ViW1iNVjYAVmts7ekcJXLOlaheXUw_XLDcCiSHS34MpRph-a3d3LdgMK6sbtlOFCJl8PqWC0h__bgSnSKQkpRMlAC8HonVoddcr0oYWrKC8BJ1ClH5Rfxe8P7-hta8WY7ThTyi7yrxUJ2u9QTSWGTET7tWqF4Esw8POyFEtFtLxXD_5tJlj13gYnMVMt1craUWrQYP0OPdqnhiDADBl4I003KFKP3WCL9RbNomSpqg2Q_FE5NrhVl7QacFSHuZXVHr4HabFhJNZh2MRvi1uQYLl1tjXAh1QJYrLwbZJ7yhbbEuFkTWuas2XCwz2RucT5P-Cyx3vieMSmW25hYTAK16uKKFMi6Pb8FVsedxzv93CPcscWxb9dlLbB0BEiNL2RGpBk9unJcdmkIGMu-plAoYRoXOiLnEEzlKPPsPZSfgbCOkjQqhSvXY-VVMvflxmozcTHFV-HfW14gOb7qH6F9ZuN305ciQayb336J40NuGhCeNG13Phn5AyvpZyr8EusrH1RyeqJa6bPVYE7fIKJ86An3FLdUBmDgAMkIQXQx9yF3qrFAi6yxqIuEoFQ4e_7EIkTF7Aoi9TeA4GXBpVvBXZ_559CvC3TW5iGbu0yxaJ5JxPk=w506-h898-no" alt="" /><figcaption>Blue skies at Crespo, Entre Ríos</figcaption></figure>
</div>