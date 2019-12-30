---
id: 620
title: Ansible and Juniper Junos | First Steps
date: 2019-11-28T09:18:20-03:00
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

Once installed, check your setup with `ansible --version` or `ansible localhost -m setup`.

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

<pre class="wp-block-preformatted">[junos]
192.168.15.194

[junos:vars]
ansible_connection=netconf
ansible_network_os=junos
ansible_user=admin
ansible_password=Password$1</pre>

**YAML Inventory**

<pre class="wp-block-preformatted">all:
  hosts:
    "192.168.15.194":
  vars:
    ansible_connection: netconf
    ansible_network_os: junos
    ansible_user: admin
    ansible_password: Password$1</pre>

Both files represent the same set of information on a different style. Personally, I preffer the YAML notation because it is more human-friendly and readeable, and it allows you to learn YAML which is used on many other automation and orchestration tools.

**What does this means**

<pre class="wp-block-preformatted">all:
  hosts:
    "192.168.15.194":</pre>

`all:` stands for all hosts and its always required. All hosts on an Ansible inventory belongs to at least two groups, _all_ and _ungrouped._ All groups of course, all hosts, and ungrouped contains all hosts which don&#8217;t belong to another specific group besides _all._

`hosts:` specifies the start position for the hosts. Hosts groups can be then written like:

<pre class="wp-block-preformatted">mendoza:
   hosts:
     "192.168.15.194":
     core.thisnetwork.net:</pre>

Hosts groups can share a set of variables, which provide information for the Ansible modules. For this example, as the `vars` section is a direct children of the `all`group, these variables will apply to all hosts in the inventory.

<pre class="wp-block-preformatted">vars:
     ansible_connection: netconf
     ansible_network_os: junos
     ansible_user: admin
     ansible_password: Password$1</pre>

The Junos modules for Ansible use the _netconf_ connection, which uses SSH and NETCONF, so ensure to allow TCP/22 and TCP/830 on your rules.

Although a fixed inventory file with manually added hosts can be enough for some users, on future entries we&#8217;ll set up a dynamic inventory calling a source of truth like [Netbox](https://github.com/netbox-community/netbox), or a monitoring system like [Zabbix](https://www.zabbix.com/).

#### Testing our setup

The real fun with Ansible is on `ansible-playbook`, but first let&#8217;s fire up an Ansible module to warm up your _Ansible-fu._

<pre class="wp-block-code"><code>$ ansible -m junos_facts -i juniper-hosts.yml all</code></pre>

  * `-m junos_facts` instructs Ansible to use the _[junos_facts](https://docs.ansible.com/ansible/latest/modules/junos_facts_module.html)_ module
  * `juniper-hosts.yml` is our previously configured inventory file
  * `all` tells Ansible the group of hosts inside the inventory to use

If everythig works allright, this will output a long JSON with a bunch of information about your device.

<pre class="wp-block-preformatted">192.168.15.195 | SUCCESS =&gt; {
    "ansible_facts": {
        "ansible_net_api": "netconf", 
        "ansible_net_filesystems": [
            "/dev/md0.uzip", 
            "devfs", 
            "/dev/gpt/junos", 
            "devfs", 
            "/dev/md1.uzip", 
            "/dev/md2.uzip", 
            "tmpfs", 
            "procfs", 
            "/dev/ada1s1e", 
            "/dev/ada1s1f", 
            "/dev/md3.uzip", 
            "/dev/md4.uzip", 
            "/dev/md5.uzip", 
            "/dev/md6.uzip", 
            "/dev/md7.uzip", 
            "/dev/md8.uzip", 
            "/dev/md9.uzip", 
            "/dev/md10.uzip", 
            "/dev/md11.uzip", 
            "/packages/mnt/junos-libs-compat32/usr/lib32", 
            "/packages/mnt/os-libs-compat32-10/usr/lib32", 
            "/packages/mnt/os-compat32/libexec", 
            "/var/jails/rest-api", 
            "/dev/md12", 
            "/dev/md13.uzip", 
            "/dev/md14.uzip", 
            "/dev/md15.uzip", 
            "/dev/md16.uzip", 
            "/dev/md17.uzip", 
            "/dev/md18.uzip", 
            "/dev/md19.uzip", 
            "/dev/md20.uzip", 
            "/dev/md21.uzip", 
            "/dev/md22.uzip", 
            "/dev/md23.uzip", 
            "/dev/md24.uzip", 
            "/dev/md25.uzip", 
            "/dev/md26.uzip", 
            "/dev/md27.uzip", 
            "/dev/md28.uzip", 
            "tmpfs", 
            "junosprocfs"
        ], 
        "ansible_net_gather_network_resources": [], 
        "ansible_net_gather_subset": [
            "hardware", 
            "default", 
            "interfaces"
        ], 
        "ansible_net_has_2RE": false, 
        "ansible_net_hostname": "None", 
        "ansible_net_interfaces": {
            ".local.": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "Loopback"
            }, 
            "cbp0": {
                "admin-status": "up", 
                "macaddress": "00:05:86:cc:c1:11", 
                "mtu": "9192", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "Ethernet"
            }, 
            "demux0": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "9192", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "Software-Pseudo"
            }, 
            "dsc": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "Software-Pseudo"
            }, 
            "em1": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:cf:9b:01", 
                "mtu": "1514", 
                "oper-status": "up", 
                "speed": "1000mbps", 
                "type": "Ethernet"
            }, 
            "esi": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "Software-Pseudo"
            }, 
            "fxp0": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:cf:9b:00", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "Unspecified", 
                "type": "Ethernet"
            }, 
            "ge-0/0/0": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:02", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/1": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:03", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/2": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:04", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/3": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:05", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/4": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:06", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/5": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:07", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/6": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:08", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/7": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:09", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/8": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:0a", 
                "mtu": "1514", 
                "oper-status": "down", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "ge-0/0/9": {
                "admin-status": "up", 
                "macaddress": "0c:b8:15:f6:47:0b", 
                "mtu": "1514", 
                "oper-status": "up", 
                "speed": "1000mbps", 
                "type": null
            }, 
            "gre": {
                "admin-status": "up", 
                "macaddress": null, 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "GRE"
            }, 
            "ipip": {
                "admin-status": "up", 
                "macaddress": null, 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "IPIP"
            }, 
            "irb": {
                "admin-status": "up", 
                "macaddress": "00:05:86:cc:c8:f0", 
                "mtu": "1514", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "Ethernet"
            }, 
            "jsrv": {
                "admin-status": "up", 
                "macaddress": "00:05:86:cc:c8:c0", 
                "mtu": "1514", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "Ethernet"
            }, 
            "lc-0/0/0": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "0", 
                "oper-status": "up", 
                "speed": "800mbps", 
                "type": "Unspecified"
            }, 
            "lo0": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "Loopback"
            }, 
            "lsi": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "Software-Pseudo"
            }, 
            "mtun": {
                "admin-status": "up", 
                "macaddress": null, 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "Multicast-GRE"
            }, 
            "pfe-0/0/0": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "0", 
                "oper-status": "up", 
                "speed": "800mbps", 
                "type": "Unspecified"
            }, 
            "pfh-0/0/0": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "0", 
                "oper-status": "up", 
                "speed": "800mbps", 
                "type": "Unspecified"
            }, 
            "pimd": {
                "admin-status": "up", 
                "macaddress": null, 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "PIMD"
            }, 
            "pime": {
                "admin-status": "up", 
                "macaddress": null, 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "PIME"
            }, 
            "pip0": {
                "admin-status": "up", 
                "macaddress": "00:05:86:cc:c8:b0", 
                "mtu": "9192", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "Ethernet"
            }, 
            "pp0": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "1532", 
                "oper-status": "up", 
                "speed": "Unspecified", 
                "type": "PPPoE"
            }, 
            "rbeb": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "Software-Pseudo"
            }, 
            "tap": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "Software-Pseudo"
            }, 
            "vtep": {
                "admin-status": "up", 
                "macaddress": "Unspecified", 
                "mtu": "Unlimited", 
                "oper-status": "up", 
                "speed": "Unlimited", 
                "type": "Software-Pseudo"
            }
        }, 
        "ansible_net_memfree_mb": 179384, 
        "ansible_net_memtotal_mb": 2058336, 
        "ansible_net_model": "vmx", 
        "ansible_net_modules": [
            {
                "name": "Midplane"
            }, 
            {
                "description": "RE-VMX", 
                "name": "Routing Engine 0"
            }, 
            {
                "description": "VMX SCB", 
                "name": "CB 0"
            }, 
            {
                "description": "VMX SCB", 
                "name": "CB 1"
            }, 
            {
                "chassis_sub_module": null, 
                "description": "Virtual FPC", 
                "name": "FPC 0"
            }
        ], 
        "ansible_net_python_version": "2.7.15+", 
        "ansible_net_routing_engines": {
            "0": {
                "cpu_background": "0", 
                "cpu_background1": "0", 
                "cpu_background2": "0", 
                "cpu_background3": "0", 
                "cpu_idle": "61", 
                "cpu_idle1": "94", 
                "cpu_idle2": "95", 
                "cpu_idle3": "95", 
                "cpu_interrupt": "2", 
                "cpu_interrupt1": "1", 
                "cpu_interrupt2": "1", 
                "cpu_interrupt3": "1", 
                "cpu_system": "27", 
                "cpu_system1": "4", 
                "cpu_system2": "3", 
                "cpu_system3": "3", 
                "cpu_user": "10", 
                "cpu_user1": "1", 
                "cpu_user2": "1", 
                "cpu_user3": "1", 
                "last_reboot_reason": "Router rebooted after a normal shutdown.", 
                "load_average_fifteen": "0.62", 
                "load_average_five": "0.73", 
                "load_average_one": "0.75", 
                "mastership_priority": "master (default)", 
                "mastership_state": "master", 
                "memory_buffer_utilization": "13", 
                "memory_dram_size": "2010 MB", 
                "memory_installed_size": "(2048 MB installed)", 
                "model": "RE-VMX", 
                "slot": "0", 
                "start_time": "2019-11-26 12:06:10 UTC", 
                "status": "OK", 
                "up_time": "11 hours, 46 minutes, 19 seconds"
            }
        }, 
        "ansible_net_serialnum": "VM5DDBEA932E", 
        "ansible_net_system": "junos", 
        "ansible_net_version": "17.1R1.8", 
        "ansible_network_resources": {}, 
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false
}
</pre>

If you take a closer look, you&#8217;ll find out that I am using a vMX appliance, can you spot where is that information?

What if we could use this and more information to gather more useful data about our router, and execute actions of provision new configuration depending on the gathered data? 

If Ansible modules are the tools in your workshop, playbooks are your instruction manuals, and your inventory of hosts are your raw material.

#### The First Playbook

Playbooks are expressed in YAML and are composed of one or more ‘plays’ in a list. The goal of a play is to map a group of hosts to some well defined roles, represented by things ansible calls tasks. At a basic level, a task is nothing more than a call to an ansible module.

Go ahead and create a new file, juniper.yml or whatever you like, and put the following inside:

<pre class="wp-block-preformatted">---
- hosts: all
  gather_facts: no

  tasks:

  - name: Get Configuration
    junos_command:
      commands:
        - show configuration</pre>

Going from top to bottom this playbook tells Ansible to:

  * `hosts: all`, use the all hosts from the inventory
  * `gather_facts: no`, don&#8217;t gather any facts for now. More on this later.
  * `tasks:`, this is the list of all the tasks I want you to do. 
  * `- name:`, this is the name of the task. It starts with a `-` because it&#8217;s a list, even if it has only a single entry
  * `junos_command:`use this module for this task, like `-m junos_command`. This module, like most, supports a set of parameters which you can see [here](https://docs.ansible.com/ansible/latest/modules/junos_command_module.html#parameters), and they are below!
  * `commands:` this is the list of commands to execute
  *  `- show configuration`: this is one of the commands

Now, run `ansible-playbook juniper.yml -i juniper-hosts.yml`. This will run your playbook, using all the hosts on the previously defined `juniper-hosts.yml` inventory.

<pre class="wp-block-code"><code>$ ansible-playbook juniper.yml -i juniper-hosts.yml

PLAY [all] ***********************************************************************************************************************************************************************

TASK [Get Configuration] *********************************************************************************************************************************************************
[WARNING]: Platform linux on host 192.168.15.195 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could
change this. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

ok: [192.168.15.195]

PLAY RECAP ***********************************************************************************************************************************************************************
192.168.15.195             : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
</code></pre>

Well, that was fun but.. there is nothing useful here, besides a warning! In fact, Ansible connected sucessfully to the router, and retrieved the configuration. We didn&#8217;t instructed it to show the configuration to us.

Modify your playbook so it looks like this:

<pre class="wp-block-preformatted">---
- hosts: all
  gather_facts: no

  tasks:

  - name: Get Configuration
    junos_command:
      commands:
        - show configuration
    register: config

  - name: Show Config
    debug: var=config</pre>

We added an additional line to the first task, `register: config`, which saves the result of the task in a variable called _config._ This name can be whatever you like.

There is also an additional task named _Show Config_, with a `debug: var=config` instruction, which outputs the config variable.

Run the playbook once again like you did before, and check the result. Of course it will depend on your configuration. On my vMX, which is almost blank, and just using DHCP for a couple interfaces, the result was this.

<pre class="wp-block-code"><code>$ ansible-playbook juniper.yml -i juniper-hosts.yml

PLAY [all] *****************************************************************************

TASK [Get Configuration] ***************************************************************
[WARNING]: Platform linux on host 192.168.15.195 is using the discovered Python
interpreter at /usr/bin/python, but future installation of another Python interpreter
could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html
for more information.

ok: [192.168.15.195]

TASK [Show Config] *********************************************************************
ok: [192.168.15.195] => {
    "config": {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python"
        }, 
        "changed": false, 
        "failed": false, 
        "stdout": [
            "## Last changed: 2019-11-26 19:02:46 UTC\nversion 17.1R1.8;\nsystem {\n    root-authentication {\n        encrypted-password \"$6$5LBS/EfQ$tL9utW2Aj4T56SfJUxdnVaF/.RIbaZ65keFn1SbCgOTi6r.LDiGt3FvvoP2WuxuuosVtx0RobNk67obTMeNgF.\";\n    }\n    scripts {\n        inactive: language python;\n    }\n    login {\n        user admin {\n            uid 2000;\n            class super-user;\n            authentication {\n                encrypted-password \"$6$9YynK9hD$Is6rEd7WNnEYGF7q2MqQJoRU/9vGjkQv7Qig.V2WT1905ShVlow4LXKeATM5HR8F1vTwROz2gUpF7z7eCJruo1\";\n            }\n        }\n    }\n    services {\n        ssh;\n        netconf {\n            ssh;\n        }\n    }\n    syslog {\n        user * {\n            any emergency;\n        }\n        file messages {\n            any notice;\n            authorization info;\n        }\n        file interactive-commands {\n            interactive-commands any;\n        }\n    }\n}\ninterfaces {\n    ge-0/0/1 {\n        unit 0 {\n            family inet {\n                dhcp;\n            }\n        }\n    }\n    ge-0/0/9 {\n        unit 0 {\n            family inet {\n                dhcp;\n            }\n        }\n    }\n}"
        ], 
        "stdout_lines": [
            [
                "## Last changed: 2019-11-26 19:02:46 UTC", 
                "version 17.1R1.8;", 
                "system {", 
                "    root-authentication {", 
                "        encrypted-password \"$6$5LBS/EfQ$tL9utW2Aj4T56SfJUxdnVaF/.RIbaZ65keFn1SbCgOTi6r.LDiGt3FvvoP2WuxuuosVtx0RobNk67obTMeNgF.\";", 
                "    }", 
                "    scripts {", 
                "        inactive: language python;", 
                "    }", 
                "    login {", 
                "        user admin {", 
                "            uid 2000;", 
                "            class super-user;", 
                "            authentication {", 
                "                encrypted-password \"$6$9YynK9hD$Is6rEd7WNnEYGF7q2MqQJoRU/9vGjkQv7Qig.V2WT1905ShVlow4LXKeATM5HR8F1vTwROz2gUpF7z7eCJruo1\";", 
                "            }", 
                "        }", 
                "    }", 
                "    services {", 
                "        ssh;", 
                "        netconf {", 
                "            ssh;", 
                "        }", 
                "    }", 
                "    syslog {", 
                "        user * {", 
                "            any emergency;", 
                "        }", 
                "        file messages {", 
                "            any notice;", 
                "            authorization info;", 
                "        }", 
                "        file interactive-commands {", 
                "            interactive-commands any;", 
                "        }", 
                "    }", 
                "}", 
                "interfaces {", 
                "    ge-0/0/1 {", 
                "        unit 0 {", 
                "            family inet {", 
                "                dhcp;", 
                "            }", 
                "        }", 
                "    }", 
                "    ge-0/0/9 {", 
                "        unit 0 {", 
                "            family inet {", 
                "                dhcp;", 
                "            }", 
                "        }", 
                "    }", 
                "}"
            ]
        ], 
        "warnings": [
            "Platform linux on host 192.168.15.195 is using the discovered Python interpreter at /usr/bin/python, but future installation of another Python interpreter could change this. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information."
        ]
    }
}

PLAY RECAP *****************************************************************************
192.168.15.195             : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0  </code></pre>

I know we all love `show config | display set`. Try adding `display: set` as an additional parameter of `junos_command`. It should look like this.

<pre class="wp-block-preformatted">junos_command:
      commands:
        - show configuration
      display: set
</pre>

Run the playbook again, and your output should be as expected if you ran it on the Junos CLI.

Stay tuned for more posts to learn how to configure Juniper Junos using Ansible.