---
layout: post
title: "MongoDB PHP Extension for MAMP"
description: ""
category:
tags: []
---


## Introduction
So you want to use MongoDB for the PHP application, well we need to install a extension for PHP so we can easily interface with it. Unforunately, using PECL will not build it correctly, so we need to install a pre-built extension from 10gen themselves.

## Simple steps

{% highlight bash %}
cd /Applications/MAMP/bin/php/php5.3.6/lib/php/extensions/no-debug-non-zts*
wget https://github.com/downloads/mongodb/mongo-php-driver/osx-php5.3-1.0.11.zip
unzip osx-php5.3-1.0.11.zip
rm -rf osx-php5.3-1.0.11.zip
echo "extension=mongo.so" >> /Applications/MAMP/bin/php/php5.3.6/conf/php.ini
{% endhighlight %}
