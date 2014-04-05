---
layout: post
title: "Installing Node.js properly on Ubuntu"
description: ""
category:
tags: []
---

I do not know why this is not documented well and why Ubuntu has decided
not to update their repositories, but I have been encountering issues
when installing Node.js packages.

~~~
npm ERR! Error: failed to fetch from registry: bower
~~~

Fortunately you can fix this by removing your current Node.js and npm
installations and tapping in a custom Ubuntu repository for a newer
build of Node.js.

~~~
sudo apt-get purge nodejs npm
sudo apt-get update
sudo apt-get install -y python-software-properties python g++ make
sudo add-apt-repository ppa:chris-lea/node.js
sudo apt-get update
sudo apt-get install nodejs
~~~

Node.js in the new repository is bundled with npm so it is not needed
to install npm separately. In fact, you will probably get an error
doing so.

After you have done those steps, try installing your package again
and you should be good to go!
