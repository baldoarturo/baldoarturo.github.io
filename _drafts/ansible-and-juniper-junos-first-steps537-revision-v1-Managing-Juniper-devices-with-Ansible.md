---
id: 568
title: Managing Juniper devices with Ansible
date: 2019-11-25T22:04:40-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/537-revision-v1/
permalink: /537-revision-v1/
---
On the previous post, I proposed a quick and dirty method to provision an Ansible control node, using Vagrant and VirtualBox. But, if you really want to spin the volume up to 11, the best is to work in a dedicated Linux machine. 

On this lab, I will be using Linux Mint, which is a Ubuntu derivate, but most examples will work on any Linux distribution.

First, install Ansible if already don&#8217;t have it. There are two easy methods.

  * Using `pip` to install the `ansible` Python module. You&#8217;ll need a working installation of Python2 (altough the latests releases of Ansible works with Python 3 too). Also, `pip` has to be installed and on path.  
  
    `$ pip install ansible`

  * Using `apt` by adding the Ansible _ppa_ repository and offload all the work to the system package manager.  
  
    `$ sudo apt-add-repository ppa:ansible/ansible`  
    `$ sudo apt-get update`  
    `$ sudo apt install ansible`

I preffer the last one and leave _apt_ handle all the job, because the _ppa_ repository is usually up-to-date with the latest release.

Once installed, check your setup with `ansible --version`.

Ansible manages Junos using _NETFCONF_ over SSH. In order to be able to connect to Junos via Ansible, both SSH and NETCONF services has to be enabled on the remote host.

SSH can be used for sending raw commands using the junos_command module, but NETCONF is definetly more versatile and supports the whose set of Ansible modules, which you can see [here](https://docs.ansible.com/ansible/latest/modules/list_of_network_modules.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW#junos).

To use NETCONF, you will need an additional Python module in your Ansible control node.

<pre class="wp-block-code"><code>$ pip install ncclient</code></pre>

And to enable SSH and NETCONF in your Juniper host , do the following. Probably you already have SSH enabled on your router so you can ignore that setting.

<pre class="wp-block-code"><code>admin> edit  
Entering configuration mode
[edit]
admin# set system services ssh 
admin# set system services netconf ssh 
[edit]
admin# commit 
commit complete</code></pre>

#### Building the Ansible Inventory

Ansible inventory files can use many formats, depending on the plugins you have. The two main formats are INI files, and YAML.

**INI-style Inventory**

<pre class="wp-block-code"><code>&#91;junos]
192.168.15.194

&#91;junos:vars]
ansible_connection=netconf
ansible_network_os=junos
ansible_user=admin
ansible_password=Password$1</code></pre>

**YAML Inventory**

<pre class="wp-block-code"><code>all:
  hosts:
    "192.168.15.194":
  vars:
    ansible_connection: netconf
    ansible_network_os: junos
    ansible_user: admin
    ansible_password: Password$1</code></pre>

Both files represent the same set of information on a different style. Personally, I preffer the YAML notation because it is more human-friendly and readeable, and it allows you to learn YAML which is used on many other automation and orchestration tools.

**What does this means**

<pre class="wp-block-code"><code>all:
  hosts:
    "192.168.15.194":</code></pre>

`all:` stands for all hosts and its always required. All hosts on an Ansible inventory belongs to at least two groups, _all_ and _ungrouped._ All groups of course, all hosts, and ungrouped contains all hosts which don&#8217;t belong to another specific group besides _all._

`hosts:` specifies the start position for the hosts. Hosts groups can be then written like:

<pre class="wp-block-code"><code>mendoza:
   hosts:
     "192.168.15.194":
     core.thisnetwork.net:</code></pre>

Hosts groups can share a set of variables, which provide information for the Ansible modules. For this example, as the `vars` section is a direct children of the `all`group, these variables will apply to all hosts in the inventory.

<pre class="wp-block-code"><code>vars:
     ansible_connection: netconf
     ansible_network_os: junos
     ansible_user: admin
     ansible_password: Password$1</code></pre>

The Junos modules for Ansible