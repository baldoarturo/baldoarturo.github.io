---
id: 503
title: Quick Ansible control node
date: 2019-11-21T11:28:41-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/498-revision-v1/
permalink: /498-revision-v1/
---
Ansible is an IT automation tools which can configure systems, deploy files and software and orchestrate almost every possible task you can imagine.

Unlike other configuration management tools like Chef or Puppet, Ansible is agent-less, and does all its magic over SSH. Forget about keeping up client daemons updated and additional certificates. Just use your existing SSH keys, or usernames and passwords.

Due to it agent-less nature, we just need a host to initiate SSH sessions towards the managed hosts. This node is called a _control node_ in the Ansible jargon.

#### Get Vagrant

We&#8217;ll use [Vagrant](https://www.vagrantup.com/downloads.html) to quickly spin up a control node, based on a [ubuntu](https://app.vagrantup.com/ubuntu)/[xenial64](https://app.vagrantup.com/ubuntu/boxes/xenial64) box. If you are not familiar with Vagrant, you just need to know it is a tool capable to provision VMs on different hypervisors, using a _Vagrantfile_ a VM template.

First, get Vagrant for your system here, <https://www.vagrantup.com/downloads.html>, and install it.

#### Use Git

In order to make everything easier, we&#8217;ll init a new git repository to keep track of all the configuration changes. Also, we can push this repository to a remote and clone from wherever we need it, making a portable Ansible control node. 

<pre class="wp-block-code"><code>C:\Users\Arturo\Desktop\ansible-control-node>git init
Initialized empty Git repository in C:/Users/Arturo/Desktop/ansible-control-node/.git/

C:\Users\Arturo\Desktop\ansible-control-node>git config user.name "Arturo Baldo"

C:\Users\Arturo\Desktop\ansible-control-node>git config user.email "baldoarturo@gmail.com"

C:\Users\Arturo\Desktop\ansible-control-node></code></pre>

Although everything can be done from a shell, I prefer to use the integrated terminal on [VSCode](https://code.visualstudio.com/download), and make use of the universe of plugins. There is support for Ansible and Ansible Playbooks, Vagrant, a lot more!

Also, even if this post uses a Windows system, everything is reproducible on Linux platforms because all the tools are multiplatform.

#### Init the Vagrant environment

The Vagrant CLI is self explanatory and easy to understand.

<pre class="wp-block-code"><code>C:\Users\Arturo\Desktop\ansible-control-node>vagrant
Usage: vagrant [options] &lt;command> [&lt;args>]

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.

Common commands:
     box             manages boxes: installation, removal, etc.
     cloud           manages everything related to Vagrant Cloud
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     halt            stops the vagrant machine
     help            shows the help for a subcommand
     init            initializes a new Vagrant environment by creating a Vagrantfile
     login
     package         packages a running vagrant environment into a box
     plugin          manages plugins: install, uninstall, update, etc.
     port            displays information about guest port mappings
     powershell      connects to machine via powershell remoting
     provision       provisions the vagrant machine
     push            deploys code in this environment to a configured destination
     rdp             connects to machine via RDP
     reload          restarts vagrant machine, loads new Vagrantfile configuration
     resume          resume a suspended vagrant machine
     snapshot        manages snapshots: saving, restoring, etc.
     ssh             connects to machine via SSH
     ssh-config      outputs OpenSSH valid configuration to connect to the machine
     status          outputs status of the vagrant machine
     suspend         suspends the machine
     up              starts and provisions the vagrant environment
     upload          upload to machine via communicator
     validate        validates the Vagrantfile
     version         prints current and latest Vagrant version
     winrm           executes commands on a machine via WinRM
     winrm-config    outputs WinRM configuration to connect to the machine</code></pre>

First, run `vagrant intit` to initialize a new environment.

<pre class="wp-block-code"><code>C:\Users\Arturo\Desktop\ansible-control-node>vagrant init
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.</code></pre>