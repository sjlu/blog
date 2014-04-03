---
layout: post
title: "Time Machine Backups to Debian/Ubuntu"
description: ""
category:
tags: []
---


## Introduction

You have a server that has a large disk array. You want to use that disk array for your Mac backups. Fortunately, Mac OSX has this nice utility named Time Machine which makes backing up and restoring your computer really easy. However, unfortunately, you need a physical disk or a Apple labeled networking product. This article is about bypassing that so that you can use your existing Linux and file storage backbone as your Time Machine disk.

## Installation

I'm using Debian Squeeze 6.0, it should be somewhat similar across Ubuntu and other versions of Debian. I'll mention all the version numbers of software that was installed.

### Installing Avahi

Avahi handles zeroconf, this means that it'll show up in the finder without you needing to do anything.

{% highlight bash %}
sudo apt-get update
sudo apt-get install avahi-daemon
{% endhighlight %}

### Installing netatalk

Unfortunately, we need a really specific version of netatalk that isn't in the repositories. Specifically, I used netatalk-2.2.2

{% highlight bash %}
sudo apt-get build-dep netatalk
sudo apt-get install build-essential
wget "http://downloads.sourceforge.net/project/netatalk/netatalk/2.2.2/netatalk-2.2.2.tar.gz?r=http%3A%2F%2Fnetatalk.sourceforge.net%2F&ts=1336662954&use_mirror=softlayer" -O netatalk-2.2.2.tar.gz
tar -xvf netatalk-2.2.2.tar.gz
cd netatalk-2.2.2
./configure --enable-debian
make
sudo make install
{% endhighlight %}

## Configuration

netatalk is a filing protocol that has advanced features that are required by Mac OSX's Time Machine. However, it doens't really play nice with Mac, so we want it to be broadcasted with Avahi so that way configuring Time Machine is a matter of double clicking.

### Directory setup

Make sure you create your directory where you'll store your Time Machine backups.

{% highlight bash %}
sudo mkdir /mnt/raid/timemachine
chmod 770 /mnt/raid/timemachine
chown www-data:staff /mnt/raid/timemachine
{% endhighlight %}

### netatalk

We need netatalk to recognize the folder as a Time Machine folder and accept advanced commands given by the Time Machine client

Add the following line to `/usr/local/etc/netatalk/AppleVolumes.default`
{% highlight bash %}
/mnt/raid/timemachine "Time Machine" cnidscheme:dbd options:usedots,upriv,tm
{% endhighlight %}

### Avahi

Avahi needs to constantly broadcast to the network about netatalk for easy setup.

Add the following file `afpd.service` to `/etc/avahi/services`
{% highlight bash %}
<service-group>
<name replace-wildcards=”yes”>%h</name>
<service>
<type>_afpovertcp._tcp</type>
<port>548</port>
</service>
<service>
<type>_device-info._tcp</type>
<port>0</port>
<txt-record>model=Xserve</txt-record>
</service>
</service-group>
{% endhighlight %}

### Finishing up

Restart the services.

{% highlight bash %}
sudo service netatalk restart
sudo service avahi restart
{% endhighlight %}
