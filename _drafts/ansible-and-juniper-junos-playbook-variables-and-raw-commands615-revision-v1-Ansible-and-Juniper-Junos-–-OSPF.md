---
id: 619
title: Ansible and Juniper Junos – OSPF
date: 2019-11-27T19:35:54-03:00
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

#### Enter junos_command

As you might guess, the _junos_command_ module allows us to execute arbitrary commands on the Junos CLI, using Ansible. This maybe doesn&#8217;t makes sense for a single host (or does it?), but when you have a dozen, a hundred, or thousands hosts with similar configuration, it is a life-saver.

Using Ansible, you could update the NTP servers for your entire fleet, or just a group of hosts. Maybe you need to bulk update passwords, change routing protocols keys, deploy a new service, or any other repetitive task.

Even if Ansible can help you with routine and repetitive tasks, another of its main features is _idempotency._ For Ansible it means after 1 run of a playbook to set things to a desired state, further runs of the same playbook should result in 0 changes. In simplest terms, idempotency means you can be sure of a consistent state in your environment. This can ensure consistent configuration across all the devices.

#### Configuring OSPF in Junos using Ansible

For this example scenario we&#8217;ll use two Juniper vMX routers linked to each other via ge-0/0/1. 

The only commands to be entered from the router CLI are:

  * IP addresses for the management interface _fxp0_
  * Enabling SSH

The objective is to use Ansible to configure the rest.

  * Create a new non-root user
      * As this is a lab environment, we won&#8217;t be using AAA, and the user will be authenticated locally with _admin_ for the username, and _Password$1_ for the password.
  * Proper IP address for the interfaces
      * vMX-01 ge-0/0/1 will use 10.10.10.1/30
      * vMX-02 ge-0/0/1 will use 10.10.10.2/30
  * Establish a OSPF relation between them to exchange routes
      * Interfaces in area 0, no authentication for now.