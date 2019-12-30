---
id: 654
title: Ansible and Juniper Junos – Playbook Variables and Raw Commands
date: 2019-12-03T09:43:21-03:00
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

#### Enter junos_config

As you might guess, the _junos_config_ module allows us to execute arbitrary commands on the Junos CLI, using Ansible. This maybe doesn&#8217;t makes sense for a single host (or does it?), but when you have a dozen, a hundred, or thousands hosts with similar configuration, it is a life-saver.

Using Ansible, you could update the NTP servers for your entire fleet, or just a group of hosts. Maybe you need to bulk update passwords, change routing protocols keys, deploy a new service, or any other repetitive task.

Even if Ansible can help you with routine and repetitive tasks, another of its main features is _idempotency._ For Ansible it means after 1 run of a playbook to set things to a desired state, further runs of the same playbook should result in 0 changes. In simplest terms, idempotency means you can be sure of a consistent state in your environment. This can ensure consistent configuration across all the devices.

#### Configuring OSPF in Junos using Ansible

For this example scenario we&#8217;ll use two Juniper vSRX instances linked to each other via ge-0/0/1. 

The only commands to be entered from the router CLI are:

  * IP addresses for the management interface _ge-0/0/0_.
      * vSRX01 _ge-0/0/0_ 192.168.100.101/24
      * vSRX02 _ge-0/0/0_ 192.168.100.102/24
  * Enabling SSH, and NETCONF over SSH

The objective is to use Ansible to configure the rest.

  * Create a new non-root user
      * As this is a lab environment, we won&#8217;t be using AAA, and the user will be authenticated locally with _admin_ for the username, and _Password$1_ for the password.
  * Proper IP address for the interfaces
      * vSRX01 ge-0/0/1 will use 10.10.10.1/30
      * vSRX02 ge-0/0/1 will use 10.10.10.2/30
  * Establish a OSPF relation between them to exchange routes
      * Interfaces in area 0, no authentication for now.

<div class="wp-block-image">
  <figure class="aligncenter size-large"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-41.png?w=640&#038;ssl=1" alt="" class="wp-image-650" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-41.png?w=494&ssl=1 494w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-41.png?resize=300%2C273&ssl=1 300w" sizes="(max-width: 494px) 100vw, 494px" data-recalc-dims="1" /></figure>
</div>

Cloud _tap1_ is connected to a host-only network of the GNS3 VM, in fact a _tap_ inteface, in the 192.168.100.0/24 subnet. This keeps the management interfaces in a common subnet, which is also accessible to the physical machine.

This is a reference configuration for vSRX01:

<pre class="wp-block-code"><code>admin@vSRX01# show | display set             
set version 12.1X47-D20.7
set system host-name vSRX01
set system root-authentication encrypted-password "$1$V7oU.Wtz$ferb8fVB.FNBf/kqGSr.V1"
set system login user admin uid 2000
set system login user admin class super-user
set system login user admin authentication encrypted-password "$1$tUJBllq0$XVWiCQYBv5H9KgEwK1Ovj0"
set system services ssh
set system services netconf ssh
set system services web-management http interface ge-0/0/0.0
set system syslog user * any emergency
set system syslog file messages any any
set system syslog file messages authorization info
set system syslog file interactive-commands interactive-commands any
set system license autoupdate url https://ae1.juniper.net/junos/key_retrieval
set interfaces ge-0/0/0 unit 0 family inet address 192.168.100.101/24
set interfaces ge-0/0/1 unit 0
set security zones security-zone MGMT host-inbound-traffic system-services ssh
set security zones security-zone MGMT host-inbound-traffic system-services ping
set security zones security-zone MGMT host-inbound-traffic system-services snmp
set security zones security-zone MGMT host-inbound-traffic system-services netconf
set security zones security-zone MGMT interfaces ge-0/0/0.0
set security zones security-zone BACKBONE host-inbound-traffic system-services ping
set security zones security-zone BACKBONE host-inbound-traffic protocols ospf
set security zones security-zone BACKBONE interfaces ge-0/0/1.0

</code></pre>

#### Using Inventory Variables

A couple changes has been made to the previous inventory. Now we have two hosts entries on the _all_ group, and each host has additional information below its definition.

<pre class="wp-block-preformatted">all:
  hosts:
    "192.168.100.101":
      ipv4_address: 10.10.10.1/24
    "192.168.101.102":
      ipv4_address: 10.10.10.2/24
  vars:
    ansible_connection: netconf
    ansible_network_os: junos
    ansible_user: admin
    ansible_password: Password$1</pre>

The new field, `ipv4_address`, contains information that will be passed to the playbook. Inside the playbook, this change is reflected in the following fashion.

<pre class="wp-block-preformatted">---
- hosts: all
  gather_facts: no

  tasks:

  - name: Config ge-0/0/1
    junos_l3_interfaces:
      config:
        - name: ge-0/0/1
          ipv4:
            - address: "{{ ipv4_address }}"
      state: replaced</pre>

Can you see the `{{ ipv4_address }}` field?

The variable in brackets is assigned with the respective information from the inventory. This means, when Ansible executes the module on a specific host, the `ipv4_address` value will be as declared on the playbook.

This is a very simple and unefficient method, because the interface is fixed on the playbook, and it could be assigned from the inventory file, but it&#8217;s enough for a first approach. Stay tuned for more scenarios with advanced methods.

Another change it that the desired state of the configuration now is _replaecd_ instead of _merged._ This is _idempotency._ No matter what you have under _ge-0/0/1,_ Ansible will replace the configuration with what builds from the inventory and the module.

<pre class="wp-block-code"><code>arturo@arturo-ThinkPad-L440:~/Desktop/ansible$ ansible-playbook juniper.yml -i juniper-hosts.yml

PLAY &#91;all] ********************************************************************************************************************

TASK &#91;Config ge-0/0/1] ********************************************************************************************************
&#91;WARNING]: Platform linux on host 192.168.100.101 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: &#91;192.168.100.101]
&#91;WARNING]: Platform linux on host 192.168.100.102 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: &#91;192.168.100.102]

TASK &#91;Put ge-0/0/1 on OSPF area 0] ********************************************************************************************
changed: &#91;192.168.100.101]
changed: &#91;192.168.100.102]

PLAY RECAP ********************************************************************************************************************
192.168.100.101            : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
192.168.100.102            : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   </code></pre>

Ok, we can see that Ansible identified needed changes on both hosts, so we can run the playbook now.

<pre class="wp-block-code"><code>arturo@arturo-ThinkPad-L440:~/Desktop/ansible$ ansible-playbook juniper.yml -i juniper-hosts.yml

PLAY [all] ********************************************************************************************************************

TASK [Config ge-0/0/1] ********************************************************************************************************
[WARNING]:  statement not found

[WARNING]: Platform linux on host 192.168.227.102 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: [192.168.227.102]
[WARNING]: Platform linux on host 192.168.227.101 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: [192.168.227.101]

PLAY RECAP ********************************************************************************************************************
192.168.227.101            : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
192.168.227.102            : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   </code></pre>

The output is the same, but the changes are being effective now. Go ahead and check the configuration on the routers.

<pre class="wp-block-code"><code>admin@VMX01> show configuration interfaces 
ge-0/0/1 {
    unit 0 {
        family inet {
            address 10.10.10.1/24;
        }
        family inet6;
    }
}
fxp0 {
    unit 0 {
        family inet {
            address 192.168.227.101/24;
        }
    }
}
</code></pre>

As expected, the _ge-0/0/1_ is configured now with the correct configuration.

Now, try to add an additional dummy address to _VMX1 ge-0/0/1_.

<pre class="wp-block-code"><code>admin@VMX02# set interfaces ge-0/0/1 unit 0 family inet address 192.168.77.44/24

admin@VMX02# commit

admin@VMX02> show configuration interfaces ge-0/0/1
ge-0/0/1 {
    unit 0 {
        family inet {
            address 10.10.10.2/24;
            address 192.168.77.44/24;
        }
        family inet6;
    }
}
</code></pre>

Run the playbook once again, and see what happens.

<pre class="wp-block-code"><code>arturo@arturo-ThinkPad-L440:~/Desktop/ansible$ ansible-playbook juniper.yml -i juniper-hosts.yml

PLAY [all] ********************************************************************************************************************

TASK [Config ge-0/0/1] ********************************************************************************************************
[WARNING]:  statement not found

[WARNING]: Platform linux on host 192.168.227.101 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

ok: [192.168.227.101]
[WARNING]: Platform linux on host 192.168.227.102 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: [192.168.227.102]

PLAY RECAP ********************************************************************************************************************
192.168.227.101            : ok=1    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
192.168.227.102            : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

arturo@arturo-ThinkPad-L440:~/Desktop/ansible$ </code></pre>

Ansible tell us that one host has changed, so it is applying the configuration once again. This is _idempotency_ stands for. 

#### Using junos_config

I changed the inventory once again, and it looks like this now.

<pre class="wp-block-preformatted">all:
   hosts:
     "192.168.227.101":
       ipv4_address: 10.10.10.1/24
       ospf_area_0: set protocols ospf area 0.0.0.0 interface ge-0/0/1
     "192.168.227.102":
       ipv4_address: 10.10.10.2/24
       ospf_area_0: set protocols ospf area 0.0.0.0 interface ge-0/0/1
   vars:
     ansible_connection: netconf
     ansible_network_os: junos
     ansible_user: admin
     ansible_password: Password$1</pre>

The _ospf\_area\_0_ item now is a raw Junos CLI command to run OSPF on area 0 on _ge-0/0/1._

The playbook now has a new task which uses the _junos_config_ module to run the previous _set_ command. Notice the _lines_ parameter is a list, so it is possible to run an arbitrary number of configuration commands. There is also a _confirm_commit_ parameter to apply the new configuration.

<pre class="wp-block-preformatted">---
- hosts: all
  gather_facts: no

  tasks:

  - name: Config ge-0/0/1
    junos_l3_interfaces:
      config:
        - name: ge-0/0/1
          ipv4:
            - address: "{{ ipv4_address }}"
      state: replaced
  
  - name: Put ge-0/0/1 on OSPF area 0
    junos_config: 
      lines:
        - "{{ ospf_area_0 }}"
      confirm_commit: yes</pre>

Run this new playbook and check the results.

<pre class="wp-block-code"><code>arturo@arturo-ThinkPad-L440:~/Desktop/ansible$ ansible-playbook juniper.yml -i juniper-hosts.yml

PLAY &#91;all] ********************************************************************************************************************

TASK &#91;Config ge-0/0/1] ********************************************************************************************************
&#91;WARNING]: Platform linux on host 192.168.100.101 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: &#91;192.168.100.101]
&#91;WARNING]: Platform linux on host 192.168.100.102 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: &#91;192.168.100.102]

TASK &#91;Put ge-0/0/1 on OSPF area 0] ********************************************************************************************
changed: &#91;192.168.100.101]
changed: &#91;192.168.100.102]

PLAY RECAP ********************************************************************************************************************
192.168.100.101            : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
192.168.100.102            : ok=2    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   </code></pre>

Well, something has changed.

<pre class="wp-block-code"><code>admin@vSRX01> show interfaces terse | match ge- 
ge-0/0/0                up    up  
ge-0/0/0.0              up    up   inet     192.168.100.101/24
ge-0/0/1                up    up  
ge-0/0/1.0              up    up   inet     10.10.10.1/24   
ge-0/0/2                up    up  
ge-0/0/3                up    up

admin@vSRX01> show ospf neighbor 
Address          Interface              State     ID               Pri  Dead
10.10.10.2       ge-0/0/1.0             Full      10.10.10.2       128    3</code></pre>

Everything worked as expected. The interface is now configured, and OSPF is running on both _ge-0/0/1.0_ interfaces