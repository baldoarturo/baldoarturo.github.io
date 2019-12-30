---
id: 337
title: MikroTik CHR
date: 2019-04-13T16:21:18-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/331-revision-v1/
permalink: /331-revision-v1/
---
<span class="wsc-spelling-problem" data-spelling-word="Mikrotik" data-wsc-lang="en_US">Mikrotik</span> Cloud Hosted Router (CHR) is a <span class="wsc-spelling-problem" data-spelling-word="RouterOS" data-wsc-lang="en_US">RouterOS</span> version intended to be used as a virtual machine instance.

It runs on <span class="wsc-spelling-problem" data-spelling-word="x86-64-bit" data-wsc-lang="en_US">x86-64-bit</span> architecture and can be deployed on most <span class="wsc-spelling-problem" data-spelling-word="hypervisors" data-wsc-lang="en_US">hypervisors</span> such as:

  * <span class="wsc-spelling-problem" data-spelling-word="VMWare" data-wsc-lang="en_US">VMWare</span>, <span class="wsc-spelling-problem" data-spelling-word="ESXi" data-wsc-lang="en_US">ESXi</span>, Player and Workstation
  * Microsoft Hyper-V
  * Oracle <span class="wsc-spelling-problem" data-spelling-word="VirtualBox" data-wsc-lang="en_US">VirtualBox</span>
  * <span class="wsc-spelling-problem" data-spelling-word="KVM" data-wsc-lang="en_US">KVM</span>‌
  * And others, like <span class="wsc-spelling-problem" data-spelling-word="Xen" data-wsc-lang="en_US">Xen</span>, but I haven&#8217;t tested it yet

Some special <span class="wsc-spelling-problem" data-spelling-word="requeriments" data-wsc-lang="en_US">requeriments</span> apply depending on the subyacent hypervisor.

**ESXi**

Network adapters must be <span class="wsc-spelling-problem" data-spelling-word="vmxnet3" data-wsc-lang="en_US">vmxnet3</span> or <span class="wsc-spelling-problem" data-spelling-word="E1000" data-wsc-lang="en_US">E1000</span>‌. Just use <span class="wsc-spelling-problem" data-spelling-word="vmxnet3" data-wsc-lang="en_US">vmxnet3</span> to get the most. Disks must be IDE, VMware <span class="wsc-spelling-problem" data-spelling-word="paravirtual" data-wsc-lang="en_US">paravirtual</span> SCSI, <span class="wsc-spelling-problem" data-spelling-word="LSI" data-wsc-lang="en_US">LSI</span> Logic SAS or <span class="wsc-spelling-problem" data-spelling-word="LSI" data-wsc-lang="en_US">LSI</span> Logic Parallel.

**Hyper-V**

Network adapters must be Network adapter or Legacy Network adapter .Disks IDE or SCSI.

<span class="wsc-spelling-problem" data-spelling-word="Qemu" data-wsc-lang="en_US"><strong>Qemu</strong></span>**/KVM**

<span class="wsc-spelling-problem" data-spelling-word="Virtio" data-wsc-lang="en_US">Virtio</span>, <span class="wsc-spelling-problem" data-spelling-word="E1000" data-wsc-lang="en_US">E1000</span> or <span class="wsc-spelling-problem" data-spelling-word="vmxnet3" data-wsc-lang="en_US">vmxnet3</span> <span class="wsc-spelling-problem" data-spelling-word="NICs" data-wsc-lang="en_US">NICs</span>. IDE, <span class="wsc-spelling-problem" data-spelling-word="Sata" data-wsc-lang="en_US">Sata</span> or <span class="wsc-spelling-problem" data-spelling-word="Virtio" data-wsc-lang="en_US">Virtio</span> disks.

**VirtualBox**

Networking using <span class="wsc-spelling-problem" data-spelling-word="E1000" data-wsc-lang="en_US">E1000</span> or <span class="wsc-spelling-problem" data-spelling-word="rtl8193" data-wsc-lang="en_US">rtl8193</span>, and disks with IDE, <span class="wsc-spelling-problem" data-spelling-word="SATA" data-wsc-lang="en_US">SATA</span>, SCSI or SAS interfaces.

### Licensing

The CHR images have full <span class="wsc-spelling-problem" data-spelling-word="RouterOS" data-wsc-lang="en_US">RouterOS</span> features enabled by default, but they use a different licensing model than other <span class="wsc-spelling-problem" data-spelling-word="RouterOS" data-wsc-lang="en_US">RouterOS</span> versions.

#### Paid licenses

**p1**

_p1_ (perpetual-1), which allows CHR to run indefinitely. It comes with a limit of <span class="wsc-spelling-problem" data-spelling-word="1Gbps" data-wsc-lang="en_US">1Gbps</span> upload per interface. All the rest of the features provided by CHR are available without restrictions. It can be upgraded _p1_ to _<span class="wsc-spelling-problem" data-spelling-word="p10" data-wsc-lang="en_US">p10</span>_ or _p-unlimited_.

**p10**

_<span class="wsc-spelling-problem" data-spelling-word="p10" data-wsc-lang="en_US">p10</span>_ (perpetual-10), which also allows CHR to run indefinitely, with a <span class="wsc-spelling-problem" data-spelling-word="10Gbps" data-wsc-lang="en_US">10Gbps</span> upload limit per interface. All features are available without restrictions. It can be upgraded to _p-unlimited._

**p-unlimited (really?)**

The&nbsp;_p-unlimited_&nbsp;(perpetual-unlimited) license level allows CHR to run indefinitely. It is the highest tier license and it has no enforced limitations.

#### Free licenses (yay!)

There are two ways to use and try CHR free of charge.

**free**

The _free_ license level allows CHR to run indefinitely, with a limit of <span class="wsc-spelling-problem" data-spelling-word="1Mbps" data-wsc-lang="en_US">1Mbps</span> upload per interface. All the rest of the features have no restrictions. This level comes activated by default on all images.

**60-day trial**

Th p1/<span class="wsc-spelling-problem" data-spelling-word="p10" data-wsc-lang="en_US">p10</span>/pU licenses can be tested with a 60 days trial.