---
id: 353
title: Introduction to MikroTik CHR
date: 2019-04-15T20:01:02-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/331-revision-v1/
permalink: /331-revision-v1/
---
MikroTik Cloud Hosted Router (CHR) is a RouterOS version intended to be used as a virtual machine instance.

It runs on x86-64-bit architecture and can be deployed on most hypervisors such as:

  * VMWare, ESXi, Player and Workstation
  * Microsoft Hyper-V
  * Oracle VirtualBox
  * KVM‌
  * And others, like Xen, but I haven&#8217;t tested it yet

Some special requeriments apply depending on the subyacent hypervisor.

**ESXi**

Network adapters must be vmxnet3 or E1000‌. Just use vmxnet3 to get the most. Disks must be IDE, VMware paravirtual SCSI, LSI Logic SAS or LSI Logic Parallel.

**Hyper-V**

Network adapters must be Network adapter or Legacy Network adapter .Disks IDE or SCSI.

**Qemu****/KVM**

Virtio, E1000 or vmxnet3 NICs. IDE, Sata or Virtio disks.

**VirtualBox**

Networking using E1000 or rtl8193, and disks with IDE, SATA, SCSI or SAS interfaces.

### Licensing

The CHR images have full RouterOS features enabled by default, but they use a different licensing model than other RouterOS versions.

#### Paid licenses

**p1**

_p1_ (perpetual-1), which allows CHR to run indefinitely. It comes with a limit of 1Gbps upload per interface. All the rest of the features provided by CHR are available without restrictions. It can be upgraded _p1_&nbsp;to&nbsp;_p10_&nbsp;or&nbsp;_p-unlimited_.

**p10**

_p10_ (perpetual-10), which also allows CHR to run indefinitely, with a 10Gbps upload limit per interface. All features are available without restrictions. It can be upgraded to _p-unlimited._

**p-unlimited (really?)**

The&nbsp;_p-unlimited_&nbsp;(perpetual-unlimited) license level allows CHR to run indefinitely. It is the highest tier license and it has no enforced limitations.

#### Free licenses (yay!)

There are two ways to use and try CHR free of charge.

**free**

The&nbsp;_free_ license level allows CHR to run indefinitely, with a limit of 1Mbps upload per interface. All the rest of the features have no restrictions. This level comes activated by default on all images.

**60-day trial**

Th p1/p10/pU licenses can be tested with a 60 days trial.

## Cool. How can i try it?

The easiest way to spin up a working instance of CHR is using the OVA appliance provided by MikroTik.  


[https://download2.mikrotik.com/](https://download2.mikrotik.com/routeros/6.43.14/chr-6.43.14.ova)[routeros](https://download2.mikrotik.com/routeros/6.43.14/chr-6.43.14.ova)[/6.43.14/chr-6.43.14.ova](https://download2.mikrotik.com/routeros/6.43.14/chr-6.43.14.ova)

#### Deployment on ESXi

Once downloaded, the OVA can be used to deploy a new instance. I&#8217;ll be using ESXi on this example. The OVA comes preconfigured with a single network adapter, but more interfaces can be added on a later stage.<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-21.png?w=640&#038;ssl=1" alt="" class="wp-image-342" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-21.png?w=914&ssl=1 914w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-21.png?resize=300%2C91&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-21.png?resize=768%2C234&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> <figcaption>Creating new VM from OVA template</figcaption></figure> <figure class="wp-block-image"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-23.png?w=640&#038;ssl=1" alt="" class="wp-image-344" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-23.png?w=896&ssl=1 896w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-23.png?resize=300%2C148&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-23.png?resize=768%2C378&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>Setting VM name, and uploading OVA file</figcaption></figure> <figure class="wp-block-image"><img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-24.png?w=640&#038;ssl=1" alt="" class="wp-image-345" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-24.png?w=917&ssl=1 917w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-24.png?resize=300%2C99&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-24.png?resize=768%2C253&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>I&#8217;ll use local storage for it</figcaption></figure> <figure class="wp-block-image"><img src="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-25.png?w=640&#038;ssl=1" alt="" class="wp-image-346" srcset="https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-25.png?w=918&ssl=1 918w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-25.png?resize=300%2C69&ssl=1 300w, https://i2.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-25.png?resize=768%2C177&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>Thin provisioned disks, and a previously configured VM network</figcaption></figure> <figure class="wp-block-image"><img src="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-26.png?w=640&#038;ssl=1" alt="" class="wp-image-347" srcset="https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-26.png?w=770&ssl=1 770w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-26.png?resize=300%2C143&ssl=1 300w, https://i1.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-26.png?resize=768%2C366&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /><figcaption>Review everything, and deploy</figcaption></figure> 

#### Initial Configuration

After the VM boots, log in via CLI with the default credentials:

  * Username: _admin_ 
  * Password: none

CHR comes with a free licence‌ by default, limited to 1Mbps upload limit. This is handy for lab purposes, or low traffic scenarios like stand-alone DHCP servers.<figure class="wp-block-image">

<img src="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-27.png?w=640&#038;ssl=1" alt="" class="wp-image-348" srcset="https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-27.png?w=850&ssl=1 850w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-27.png?resize=300%2C179&ssl=1 300w, https://i0.wp.com/arturobaldo.com.ar/wordpress/content/uploads/image-27.png?resize=768%2C458&ssl=1 768w" sizes="(max-width: 640px) 100vw, 640px" data-recalc-dims="1" /> </figure> 

A DHCP client is enabled by default on the single existing _ether1_ interface. Use any of the following methods to find out the adquired address.

<pre class="wp-block-code"><code>/ip dhcp-client print
/ip address print</code></pre>

Let&#8217;s get a trial licence. You will need the credentials for your MikroTik account. If you don&#8217;t have a MikroTik account, get one [here.](https://mikrotik.com/client/register)

The CHR instance will also need Internet access, so be sure to connect the virtual NIC to a VM network where it can make its way to the outside.

<pre class="wp-block-code"><code>[admin@CHR] > sys license renew account=your@account.com password=yourpassword level=

Level ::= p-unlimited | p1 | p10
</code></pre>

Once you request a trial license, check the status with

<pre class="wp-block-code"><code>[admin@CHR] > sys lic print
        system-id: 0ywIRMYrtGA
            level: p1
  next-renewal-at: may/05/2019 17:59:59
      deadline-at: jun/04/2019 17:59:59
</code></pre>

We&#8217;ll install _The Dude_ on the next post, and configure it for some custom monitoring.