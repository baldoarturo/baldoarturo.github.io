---
id: 617
title: Ansible and Juniper Junos – Routing Protocols
date: 2019-11-27T17:01:34-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/615-revision-v1/
permalink: /615-revision-v1/
---
As seen on previous posts, Ansible has a decent catalog of modules for Junos, allowing various operations like:

  * Manage layer 2 and layer 3 intefaces
  * Manage LAG and LACP
  * Configure VLANs and VRFs
  * And many others

But, what happens if you want to automate an operation which doesn&#8217;t has its own dedicated module. For example, routing protocols.

####