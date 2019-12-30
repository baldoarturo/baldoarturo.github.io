---
id: 614
title: 'Ansible and Juniper Junos &#8211; Interfaces'
date: 2019-11-27T13:48:53-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/611-revision-v1/
permalink: /611-revision-v1/
---
Previously we had made our first incursions connecting an Ansible control node with a Juniper router. In this post, we&#8217;ll see how to retrieve information about the router interfaces, both layer 2 and layer 3, and configure new interfaces.

The official Ansible modules reference will be your main guide for any additional information.  
[https://docs.ansible.com/ansible/latest/modules/list\_of\_network_modules.html#junos](https://docs.ansible.com/ansible/latest/modules/list_of_network_modules.html#junos)

If you are interested on this subject, but don&#8217;t have access to physical gear, most of it should work on virtual appliances like vMX, vQFX, which you can operate on a stand-alone mode or on a network environment like GNS3 or EVE-NG. 

Juniper vLabs will also give you an introduction to the Juniper platform.  
<https://jlabs.juniper.net/vlabs/portal/index.page> 

#### Layer 2 Interfaces

A basic layer 2 interface configuration in Junos looks like this:

<pre class="wp-block-preformatted">ge-0/0/1 {
     description "L2 interface";
     speed 1g;
     unit 0 {
         family ethernet-switching {
             interface-mode access;
             vlan {
                 members vlan30;
             }
         }
     }
 }</pre>

This configuration can be written as an Ansible playbook like this:

<pre class="wp-block-preformatted">- name: "Replace provided configuration with device configuration"
  junos_l2_interfaces:
    config:
      - name: ge-0/0/1
        access:
          vlan: v30
    state: merged</pre>

Currently, I do not have any EX series or QFX series to decomission and run tests against it, so stay tuned for any updates on this.

The official module documentation is on [https://docs.ansible.com/ansible/latest/modules/junos\_l2\_interfaces_module.html](https://docs.ansible.com/ansible/latest/modules/junos_l2_interfaces_module.html).

#### Layer 3 Interfaces

A basic layer 3 interface configuration in Junos looks like this:

<pre class="wp-block-preformatted">ge-0/0/1 {
     unit 0 {
         family inet {
             address 192.168.1.10/24;
         }
     }
 }</pre>

This configuration can be written as an Ansible playbook like the following, using the same format as the last post.

<pre class="wp-block-preformatted">---
- hosts: all
  gather_facts: no

  tasks:

  - name: Config ge-0/0/1
    junos_l3_interfaces:
      config:
        - name: ge-0/0/1
          ipv4:
            - address: 192.168.1.10/24
      state: merged</pre>

Let&#8217;s run it and check the result.

<pre class="wp-block-code"><code>$ ansible-playbook juniper.yml -i juniper-hosts.yml

PLAY [all] ********************************************************************************************************************

TASK [Config ge-0/0/1] ********************************************************************************************************
[WARNING]: Platform linux on host 192.168.15.220 is using the discovered Python interpreter at /usr/bin/python, but future
installation of another Python interpreter could change this. See
https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information.

changed: [192.168.15.220]

PLAY RECAP ********************************************************************************************************************
192.168.15.220             : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   </code></pre>

Did you noticed the _**changed**_ ****output?  
What about the configuration on the router now?

<pre class="wp-block-code"><code>admin> show configuration interfaces
ge-0/0/1 {
    unit 0 {
        family inet {
            address 192.168.1.10/24;
        }
    }
}
fxp0 {
    unit 0 {
        family inet {
            address 192.168.15.220/24;
        }
        family inet6;
    }
}
</code></pre>

That&#8217;s awesome! We just configured and IP address on `ge-0/0/1`.

How does Ansible knows what to replace, what to override, and what to delete?

If you take a closer look to the playbook, you will see a line with `state: merged`. This is a module parameter that specifies the state of the router configuration after the module finishes its job.

The possible values are:

  * `merged`
  * `replaced`
  * `overriden`
  * `deleted`

In fact, the module matches whatever configuration you build on its parameters, applies a configuration action, and commits the result.

The official module documentation is on [https://docs.ansible.com/ansible/latest/modules/junos\_l3\_interfaces_module.html](https://docs.ansible.com/ansible/latest/modules/junos_l3_interfaces_module.html).