---
layout: post
title: "Moving Away from MAMP and into Homebrew"
description: ""
category:
tags: []
---


So one of the most annoying things I ran into the PECL support from MAMP. This means no easy external extention setups for you. It also means those `php.h` headers that you need to compile against PHP with does not exist either. Don't get me wrong, MAMP is a really awesome startup to help you get your environment running but it doesn't nearly reach the point of having a barebone, compiled version on your Mac.

## Steps

First thing you need to do is have Homebrew setup. If you don't know what Homebrew is, shame on you. It's basically a package manager for your Mac for external tools that are useful for your computer or Terminal. Remember that Homebrew requires Xcode, which is a free application in the Mac App Store, so go get that first!
{% highlight bash %}
ruby -e "$(curl -fsSkL raw.github.com/mxcl/homebrew/go)"
{% endhighlight %}

Next, make sure Homebrew is setup properly, you can easily check by running `brew doctor`. It'll even tell you how to fix things if it finds problems.

After, we need to "tap" some external Homebrew repositories, specifically the ones related to PHP. To do so run the following commands.
{% highlight bash %}
brew tap homebrew/dupes
brew tap josegonzalez/homebrew-php
{% endhighlight %}

Once that is done, we will need to install PHP using Homebrew. It has a couple of compilation flags, so follow carefully!
{% highlight bash %}
brew install php53 --with-mysql --with-imap --with-pgsql --with-suhosin
{% endhighlight %}

Note that when it is finished installing it will throw out some things you need to do. Make sure you do it! Or else you may have some problems in the future.

Now it's time to place PHP into Mac's version of Apache. To do so, we need to edit the file `/etc/apache2/httpd.conf` and add the following line.
{% highlight bash %}
LoadModule php5_module /usr/local/Cellar/php53/5.3.17/libexec/apache2/libphp5.so
{% endhighlight %}

Last but not least for Apache, we need to enable it through `Settings->Sharing->Web Sharing`. When that's on, Apache's config file will be loaded and be placed as a process. You can now run `sudo apachectl restart` restart, stop, and start the server.

You can also edit the file `/etc/apache2/httpd.conf` to enable any of the modules that it already has. I specifically like enabling "Virtual Hosts" so maybe you should too! An example of my `/etc/apache2/extra/httpd-vhosts.conf` looks something like this.
{% highlight bash %}
#
# Use name-based virtual hosting.
#
NameVirtualHost *:80

# Defualt virtual host
# /Users/sjlu/Web
<VirtualHost *:80>
   DocumentRoot "/Users/sjlu/Web"

   <Directory "/Users/sjlu/Web">
       Options Indexes MultiViews FollowSymLinks
       AllowOverride All
       Order allow,deny
       Allow from all
   </Directory>
</VirtualHost>
{% endhighlight %}

Remember how I had this [post](http://blog.stevenlu.com/2012/05/14/mongodb-php-extension-for-mamp/) to get Mongo adapter working? Well, it's so much more simple now! All you need to do is run.
{% highlight bash %}
pecl install mongo
{% endhighlight %}

Note you may need to edit `/usr/local/etc/php/5.3/php.ini` and add this line to get it working.
{% highlight bash %}
extension=mongo.so
{% endhighlight %}

Last but not least, we need MySQL. Really easy with Homebrew, just run the following lines and follow its instructions.
{% highlight bash %}
brew install mysql
{% endhighlight %}

Done! You should now have a full MAMP setup, with out the MAMP :).