---
layout: post
title: "Installing AirVideo Server on Debian Squeeze"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## Introduction

If you didn't already know, AirVideo is a nice utility which lets you stream your media files to your iOS devices. It compresses and re-encodes them on the fly so that it is playable by these devices.

If you don't already have it, AirVideo is $2.99 on the [App Store](http://itunes.apple.com/us/app/air-video-watch-your-videos/id306550020?mt=8).

This tutorial helps you setup AirVideo on your Linux server, specifically Debian.

## Steps

First, add the [Debian Multimedia](http://deb-multimedia.org) to your apt-sources if you have not already done so. 
{% highlight bash %}
sudo apt-get install deb-multimedia-keyring
sudo echo deb http://www.deb-multimedia.org squeeze main non-free >> /etc/apt/sources.list
apt-get update && apt-get upgrade
{% endhighlight %}

Next, we need to compile libav! But we need to install the dependencies first.
{% highlight bash %}
sudo apt-get install autoconf build-essential checkinstall git libfaac-dev libgpac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev librtmp-dev libtheora-dev libtool libvorbis-dev pkg-config texi2html yasm zlib1g-dev x264 libx264-dev
{% endhiglight %}

Finally, we can get our custom version of libav, specifically patched for AirVideo. Do not use the latest, it will not work otherwise. Do not `make install`, we will place our custom build ourselves.
{% highlight bash %}
wget http://s3.amazonaws.com/AirVideo/Linux-2.4.6-beta3/libav.tar.bz2
tar -xjvf libav.tar.bz2
cd libav.tar.bz2
./configure --enable-pthreads --disable-shared --enable-static --enable-gpl --enable-libx264 --enable-libmp3lame --enable-nonfree --enable-encoder=libfaac
make
{% endhighlight %}
