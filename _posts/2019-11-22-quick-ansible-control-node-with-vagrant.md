---
id: 498
title: Quick Ansible control node with Vagrant
date: 2019-11-22T11:43:05-03:00
author: Arturo
layout: post
guid: https://arturobaldo.com.ar/?p=498
permalink: /quick-ansible-control-node-with-vagrant/
categories:
  - Projects
tags:
  - ansible
  - automation
  - python
  - vagrant
---
Ansible is an IT automation tools which can configure systems, deploy files and software and orchestrate almost every possible task you can imagine.

Unlike other configuration management tools like Chef or Puppet, Ansible is agent-less, and does all its magic over SSH. Forget about keeping up client daemons updated and additional certificates. Just use your existing SSH keys, or usernames and passwords.

Due to it agent-less nature, we just need a host to initiate SSH sessions towards the managed hosts. This node is called a _control node_ in the Ansible jargon.

On this post, we&#8217;ll catch up with the basics of Vagrant and Ansible. The repository for this post is on <https://github.com/baldoarturo/ansible-control-node>.

#### Download VirtualBox

VirtualBox is a general-purpose full virtualizer for x86 hardware, targeted at server, desktop and embedded use. Get it from [here](https://www.virtualbox.org/wiki/Downloads).

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

Wait! You have a new file on your git repository. Make it the first commit.

<pre class="wp-block-code"><code>C:\Users\Arturo\Desktop\ansible-control-node>git add .

C:\Users\Arturo\Desktop\ansible-control-node>git commit -m "First commit"
[master (root-commit) 1416f75] First commit
 1 file changed, 70 insertions(+)
 create mode 100644 Vagrantfile

C:\Users\Arturo\Desktop\ansible-control-node></code></pre>

On the newly create _Vagrantfile_, we can set configuration and provisioning options for the VM.

First, replace `config.vm.box = "base"` with `config.vm.box&nbsp;=&nbsp;"ubuntu/xenial64"` to use this box. 

If you want to see the VM, config `vb.gui = true`. By default this setting is commented out, so the VM will start in headless mode.

Feel free to customize the VM memory by commenting out the `vb.memory&nbsp;=&nbsp;"1024"` setting and adjusting it to your needs.

Once you are done, save your changes, commit, execute `vagrant up`, and watch Vagrant create a brand new VM for you.

<pre class="wp-block-code"><code>C:\Users\Arturo\Desktop\ansible-control-node>vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'ubuntu/xenial64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'ubuntu/xenial64' version '20191114.0.0' is up to date...
==> default: Setting the name of the VM: ansible-control-node_default_1574353915423_60685
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Warning: Connection reset. Retrying...
    default: Warning: Connection aborted. Retrying...
    default: Warning: Remote connection disconnect. Retrying...
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default:
    default: Guest Additions Version: 5.1.38
    default: VirtualBox Version: 6.0
==> default: Mounting shared folders...
    default: /vagrant => C:/Users/Arturo/Desktop/ansible-control-node</code></pre>

Well, how do we login into the new VM? Try `vagrant ssh`

<pre class="wp-block-code"><code>C:\Users\Arturo\Desktop\ansible-control-node>vagrant ssh
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-169-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


0 packages can be updated.
0 updates are security updates.

New release '18.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


vagrant@ubuntu-xenial:~$ whoami
vagrant
vagrant@ubuntu-xenial:~$ </code></pre>

Type `exit` and you will return to your system prompt.

You&#8217;ll see that you have new items on your directory. A `.log` file, with the output from Vagrant, and a `.vagrant/` directory.

To exclude them from the repository, create a `.gitignore`, with the following contents.

<pre class="wp-block-code"><code>.vagrant/
*.log</code></pre>

Save and commit.

#### Provisioning the VM

Return to the _Vagrantfile_, and add a section like this:

<pre class="wp-block-code"><code>  config.vm.provision "shell", inline: &lt;&lt;-SHELL
    apt-get update
    apt-get install software-properties-common -y
    apt-add-repository --yes --update ppa:ansible/ansible
    apt-get install ansible -y
  SHELL</code></pre>

This block instructs Vagrant to execute commands on the shell. First, the package list is updated, then the `software-properties-common` package is installed. To install Ansible, the `ppa:ansible/ansible` repository is added, and then Ansible is installed.

Notice the usage of `-y` to avoid interactive prompts.

Save the file, commit, and run `vagrant provision`. This will re-provision the VM with the new instructions from the _Vagrantfile_.

Once Vagrant finishes, connect once again with `vagrant ssh`, and run `ansible --version`.

<pre class="wp-block-code"><code>vagrant@ubuntu-xenial:~$ ansible --version
ansible 2.9.1
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/vagrant/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.12 (default, Oct  8 2019, 14:14:10) [GCC 5.4.0 20160609]</code></pre>

That&#8217;s great! We have just installed Ansible on our new VM.

#### Seek and Destroy

You might wonder, where is this VM? Head to the VirtualBox Manager, and you&#8217;ll find it inside.<figure class="wp-block-image size-large">

<img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-36.png?w=640&#038;ssl=1" alt="" class="wp-image-527" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-36.png?w=960&ssl=1 960w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-36.png?resize=300%2C164&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-36.png?resize=768%2C420&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption>Here it is!</figcaption></figure> 

Without asking anyone.. kill the machine and delete it.

<div class="wp-block-image">
  <figure class="aligncenter size-large"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-37.png?w=640&#038;ssl=1" alt="" class="wp-image-528" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-37.png?w=665&ssl=1 665w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-37.png?resize=300%2C167&ssl=1 300w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>Hard power off</figcaption></figure>
</div>

<div class="wp-block-image">
  <figure class="aligncenter size-large"><img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-38.png?w=640&#038;ssl=1" alt="" class="wp-image-529" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-38.png?w=332&ssl=1 332w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-38.png?resize=300%2C196&ssl=1 300w" sizes="(max-width: 332px) 100vw, 332px" data-recalc-dims="1" /><figcaption>Delete all files</figcaption></figure>
</div>

Head back to the console, and you will see that the SSH session has died. Of course, this makes perfect sense as the VM no longer exists.

<pre class="wp-block-code"><code>vagrant@ubuntu-xenial:~$ Connection to 127.0.0.1 closed by remote host.
 Connection to 127.0.0.1 closed.</code></pre>

If you try to run `vagrant provision`, it will fail, as there is no VM to provision.

<pre class="wp-block-code"><code>C:\Users\Arturo\Desktop\ansible-control-node>vagrant provision
==> default: VM not created. Moving on...</code></pre>

But what happens if you run `vagrant up` once again? Surprise! The VM will be recreated with all the previous configuration settings.

#### Using Ansible

Well, the VM is ready, Ansible is installed.. now what? Let&#8217;s write an inventory file and see what can we do.

I&#8217;ll create a simple file to connect to a testing docker instance on 192.168.85.253. No need to worry about ssh keys now, as we are testing, username and password will do the trick.

<pre class="wp-block-code"><code>vagrant@ubuntu-xenial:~$ cat docker
&#91;all:vars]
ansible_connection=ssh
ansible_user=MY_USERNAME
ansible_password=MY_PASSWORD

&#91;docker]
192.168.85.253</code></pre>

The `[all:vars]` section configures the connection settings for all hosts in the file. The `[docker]` section contains the lists of hosts, in this case just 192.168.85.253.

Let&#8217;s run: `ansible -m ping -i docker all`, where `-m` is the module to run, `-i` is the inventory file, and `all` is the section of the inventory file which contains the hosts. Notice this is a very special _ping._

<pre class="wp-block-code"><code>vagrant@ubuntu-xenial:~$ ansible -m ping -i docker all
&#91;DEPRECATION WARNING]: Distribution Ubuntu 16.04 on host 192.168.15.253 should use /usr/bin/python3, but is using /usr/bin/python for backward compatibility with prior Ansible releases. A future Ansible release
 will default to using the discovered platform python for this host. See https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html for more information. This feature will be removed   
in version 2.12. Deprecation warnings can be disabled by setting deprecation_warnings=False in ansible.cfg.
192.168.15.253 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}</code></pre>

Whoa! That definetly doesn&#8217;t look like a ICMP ping. First, this is an old box about to be decomissioned, so it has been very unmantained. Ansible warns us about an old version of Python, and a old version of Ansible which was installed previously on the remote hosts.

Now, try `ansible -m setup -i docker all`. Be aware this will throw a ton of data to you. The setup module gathers data about the host.

What it does and how does it, is documented [here](https://docs.ansible.com/ansible/latest/modules/setup_module.html).