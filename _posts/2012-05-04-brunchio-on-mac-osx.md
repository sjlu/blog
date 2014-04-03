---
layout: post
title: "Brunch.io on Mac OSX"
description: ""
category:
tags: []
---


## Introduction

This is just a simple tutorial on how to setup Brunch.io to work on Mac. If you don't already know, Brunch is a javascript application assembler. You use CoffeeScript, Handlebars.js, Stylus and Backbone to write your application while Node.js watches for changes and compiles it for you.

## Install

- We need to install Homebrew first.

{% highlight bash %}
/usr/bin/ruby -e "$(/usr/bin/curl -fsSL https://raw.github.com/mxcl/homebrew/master/Library/Contributions/install_homebrew.rb)"
{% endhighlight %}

- Install Node.js and npm

{% highlight bash %}
brew install nodejs
curl http://npmjs.org/install.sh | sh
{% endhighlight %}

- Installing Brunch.io

{% highlight bash %}
npm install -g brunch
{% endhighlight %}

- Starting a new Brunch.io project

{% highlight bash %}
brunch new test_project
{% endhighlight %}

- Running a Brunch instance and web server

{% highlight bash %}
cd test_project
brunch watch --server
{% endhighlight %}

## How it works

When you run an instance of Brunch.io, it will watch it the project directory for changes. Every time you make a change, it will compile it for you. You write your code in the `app` directory and it will compile to directory named `public`.

By default, Brunch installs Backbone.js for you, some version of HTML5 Boilerplate, Stylus and Handlebars.js. If you're not familiar with any of these, I suggest you read up on them, especially on Backbone.js. You can easily install Jade by running `npm install jade`.

## Extra reading

- [Brunch.io](http://brunch.io/)
- [Backbone.js](http://documentcloud.github.com/backbone/)
