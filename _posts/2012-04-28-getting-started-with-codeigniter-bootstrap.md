---
layout: post
title: "Getting Started with CodeIgniter Bootstrap"
description: ""
category:
tags: []
---


## About

CodeIgniter gives you the ability to write PHP code quickly and effectively while Twitter Bootstrap allows you to create
interfaces quickly without needing to style primary elements. However, the two are not combined and certain things between
CodeIgniter and Twitter Boostrap do not play nicely. With CodeIgniter Bootstrap, it allows you to skip this "combination"
phase and have you start developing your idea instead of developing the common web elements.

## Getting Started

Make sure you have a Apache, MySQL and PHP installation or equilvalent.
When you are done with that, you need to retrieve the CodeIgniter Boostrap code base.

{% highlight bash %}
git clone https://github.com/sjlu/CodeIgniter-Bootstrap.git
{% endhighlight %}

You should now be able to point your browser to the project and see the frontpage screen.

## Basic Tutorial

Your controller is what controls what is routed between the `model` and the `view`.
Or in better terms, between the user and your backend data (possibly your database).
Your controller can also accept input variables in the URL so that it can properly
distinguish what it should deliver to the view.

`application/controller/frontpage.php`
{% highlight php %}
<?php
class Frontpage extends CI_Controller {

   public function index()
   {

   }

}
?>
{% endhighlight %}

This is your basic controller, you will see that we are naming our router `Fromtpage`
which is extending the standard `CI_Controller` class. The function `index()` will be
the default route that everyone will see when they request the `Frontpage` router.

The next thing we will need to do is add a view that the controller can send. This is
basic HTML that will incorporate some basic Twitter Bootstrap classes.

`application/view/frontpage.php`
{% highlight html %}
<div class="container">
   <div class="hero-unit">
      This is the frontpage view.
   </div>
</div>
{% endhighlight %}

We aren't done yet, we now need to tell the controller to load our view for the specific
route. To do so, all we need to add is one more line, to make our controller look like this.

`application/controller/frontpage.php`
{% highlight php %}
<?php
class Frontpage extends CI_Controller {

   public function index()
   {
      $this->load->view('frontpage');
   }

}
?>
{% endhighlight %}

By that single call, CodeIgniter will look in the `application/view` directory for a view
that is name `frontpage.php`. Don't worry, it'll auto append the extension for you.

So when you go to `http://localhost/CodeIgniter-Bootstrap/index.php/frontpage`
in your browser you should see the view that you've created for that specific route.

## Conclusion

You have set up a basic installation of CodeIgniter Bootstrap and created a
basic controller and view. Now I know this looks really basic, but there will
soon be more tutorials up about how to use it, so stay tuned!

## Extra Reading
- [CodeIgniter Bootstrap](https://github.com/sjlu/CodeIgniter-Bootstrap)
- [CodeIgniter Documentation](http://codeigniter.com/user_guide/)
- [Twitter Bootstrap Documentation](http://twitter.github.com/bootstrap/)
