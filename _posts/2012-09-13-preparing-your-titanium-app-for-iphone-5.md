---
layout: post
title: "Preparing Your Titanium App for iPhone 5"
description: ""
category:
tags: []
---


## Introduction

So as you know, Apple has released a new iOS device that increases the screen height. If you app isn't prepared for it, it will letterbox your existing application. It'll make your app look odd and you're not using the full screen space! What a waste.

Now because Titanium hasn't released support for XCode 4.5, and iOS 6.0, you'll need to fix up some application settings, add a new photo, and possibly change some of your codebase, but I'll tell you how to do that all here.

## What to do

1. Well, first make sure you have XCode 4.5 installed, make sure you can run your application using the new iOS simulator "Retina 4-inch" should be the name.

2. Next thing you need to do is create an splash image. Without it, XCode will not run your application in the full length and instead run it in its letterbox mode. Your image should be of size 640x1136 and named `Default-568h@2x.png` and placed in the folder `Resources/iphone`.

3. Now for some reason, you'll get a verification error if you try submitting it to the App Store, something near the lines of "armv6 is missing". Apple could be deprecating support for iOS 4.0 or maybe your new XCode doesn't have the compiler. You can fix this by adding the following to your `Resources/tiapp.xml` file.

{% highlight xml %}
<ios>
   <min-ios-ver>4.3</min-ios-ver>
</ios>
{% endhighlight %}

4. If you have defined object heights, especially your window sizes or views, you want to change that with dynamic heights. If your app is used for Android, you may already have this done. Just change it with the following. Make sure to deduct whatever you need from this variable.

{% highlight javascript %}
Titanium.Platform.displayCaps.platformHeight;
{% endhighlight %}

## Finish

Submit your application and you should be good to go! You will get stopped since you need to provide screenshots to iTunes Connect for the iPhone 5 resolutions.
