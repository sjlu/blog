---
layout: post
title: "Inheriting a Base Controller In CodeIgniter"
description: ""
category:
tags: []
---


## Introduction

So sometimes you want to have a base controller class where you can store common methods through out all your controllers. Here's how you do it.

## Steps

### Writing the Inheritable Controller

All you need to do is write the file `application/core/MY_Controller.php`

{% highlight php %}
<?php
class MY_Controller extends CI_Controller
{
	function __construct()
	{
		parent::__construct();
	}
}
{% endhighlight %}

Then you should should write separate controllers depending on your methods, for example I will write `application/core/Main_Controller.php`

{% highlight php %}
<?php
class Main_Controller extends MY_Controller
{
	function __construct()
	{
		parent::__construct();
		// do some session stuff here :)
	}
}
{% endhighlight %}

### Auto-loading the Controllers

We need to ammend `config.php` to include all the controllers in the `application/core` directory. Note that means all these controllers will be loaded you will fallout of CodeIgniter conventions if you do so. Make sure you absolutely need these core functions or else you'll be loading in garabe you don't need on every request.

{% highlight php %}
/**
| -------------------------------------------------------------------
|  Native Auto-load
| -------------------------------------------------------------------
|
| Nothing to do with config/autoload.php, this allows PHP autoload to work
| for base controllers and some third-party libraries.
|
*/
function __autoload($class)
{
	if(strpos($class, 'CI_') !== 0)
 	{
  		@include_once( APPPATH . 'core/'. $class . EXT );
 	}
}
{% endhighlight %}

### Finishing up

Lastly, you need to actually use your new extended controller!

{% highlight php %}
<?php
class Action extends Main_Controller
{

}
{% endhighlight %}