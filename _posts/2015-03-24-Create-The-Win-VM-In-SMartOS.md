---
date: 2015-03-24 22:10:43
layout: post
title: Install the Windows vitual machine in SmartOS
categories: 文章
tags: 新奇好玩
---

Before we create the Windows virtual machine, there are some preparative things to on your SmartOS.

The windows needs smartos-vmtools for the virtual dirvers like network,disk and so on.

First you need to get the pkgin Tool ready to install some needed binaries like git and mkisofs for creating the smartos-vmtools:

```shell
[root@SmartOS ~]# cd /
[root@SmartOS /]# curl -k http://pkgsrc.joyent.com/sdc6/2012Q2/x86_64/bootstrap.tar.gz | gzcat | tar -xf -
[root@SmartOS /]# pkg_admin rebuild
[root@SmartOS /]# pkgin -y up
[root@SmartOS /]# pkgin in scmgit-base gcc47 unzip
[root@SmartOS /]# pkgin install cdrtools-3.01alpha24nb1
```

Move to your /opt dir and get the latest smartos-vmtools sources:

```shell
[root@SmartOS /]# cd /opt/
[root@SmartOS /opt]# git clone git://github.com/joyent/smartos-vmtools.git smartos-vmtools
```
But because there are some unknow reasons that i don't konow,the git command does not work, so i used wget command to get the source code from github.com

```shell
[root@SmartOS /opt]# wget --no-check-certificate https://github.com/joyent/smartos-vmtools/archive/master.zip
[root@SmartOS /opt]# unzip master.zip
```
Build the iso which will be needed from source:

```shell
[root@SmartOS /opt]# cd /opt/smartos-vmtools/
[root@SmartOS /opt/smartos-vmtools/]# ./bin/build-image
```
Create an empty KVM instance:
I store the *.json file in /usbkey/vmcfg, you can choose the dir where you liked.
```shell
[root@SmartOS /opt/smartos-vmtools/]# cd /usbkey/vmcfg
[root@SmartOS /usbkey/vmcfg]# vi win7.json
```

```shell
{
  "brand": "kvm",
  "alias": "win7_64",
  "vcpus": 2,
  "autoboot": false,
  "ram": 2048,
  "disks": [
    {
      "boot": true,
      "model": "ide",
      "size": 61440
    }
  ],
  "nics": [
  {
    "nic_tag": "admin",
    "model": "virtio",
    "ip": "10.0.0.21",
    "netmask": "255.255.255.0",
    "gateway": "10.0.0.1",
    "primary": true
  }
  ]
}
```
the "disks"->"model" you can also choose virtio, but i failed when loading the driver. So i suggest "ide" in you config file.

Then creat the windows VM:
```shell
[root@SmartOS /usbkey/vmcfg]# vmadm create -f win7.json
```

Cope the smartos-vmtools to the new VM's root:
```shell
[root@SmartOS /usbkey/vmcfg]# cp /opt/smartos-vmtools-master/cache/smartos-vmtools-20150315092000.iso /zones/YourInstanceUUID/root/
```

Upload your windows install ISO to /zones/YourInstanceUUID/root/,you can use your share folder like samba or nfs on SmartOS or use the scp command in Linux/Mac or the winScp.exe in windows to upload the windows ISO file.


Then start the VM:
```shell
[root@SmartOS /usbkey/vmcfg]# cd /zones/YourInstanceUUID/
[root@SmartOS /zones/YourInstanceUUID/]# vmadm boot YourInstanceUUID order=cd,once=d cdrom=/yourwindows.iso,ide cdrom=/smartos-vmtools-20150315092000.iso,ide
```

You can access your VM via a VNC Viewer (we are using TightVNC). You can get the necessary connection setting via:

```shell
[root@SmartOS /zones/YourInstanceUUID/]# vmadm info YourInstanceUUID vnc
```

```shell
{
  "vnc": {
    "host": "10.0.0.6",
    "port": 34783,
    "display": 28883
  }
}
```

After install the windows, you can find the net drivers in the smartos-vmtools cdrom -> windows -> drivers -> NetKVM -> Vista.


Refer to these two articles:
http://ispire.me/how-to-create-smartos-windows-vm/
http://www.philipp.haussleiter.de/2013/07/creating-a-smartos-dataset-for-win-srv-2k12r2/

