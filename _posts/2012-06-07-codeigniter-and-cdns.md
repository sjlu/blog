---
layout: post
title: "CodeIgniter and CDNs"
description: ""
category:
tags: []
---


## Introduction

So in the past, I realized that some of projects gained traction and new projects had a requirement to off-load static content using a Content Delivery Network.

## Simple Steps

So you should be using `base_url` already in your application to link to your assets or content. If you are not, you are not making your website environment independent. I assume you are already using the function, you should change your assets and content to use this function if this isn't the case.

1. Lets extend the current URL helper function. Edit the file `application/config/config.php`.
{% highlight php %}
/*
|--------------------------------------------------------------------------
| Class Extension Prefix
|--------------------------------------------------------------------------
|
| This item allows you to set the filename/classname prefix when extending
| native libraries.  For more information please see the user guide:
|
| http://codeigniter.com/user_guide/general/core_classes.html
| http://codeigniter.com/user_guide/general/creating_libraries.html
|
*/
$config['subclass_prefix'] = 'extend.';
{% endhighlight %}

2. Lets overwrite the existing `base_url` function. Create a new file `application/helpers/extend.url_helper.php`.
{% highlight php %}
/**
 * base_url
 *
 * This function OVERRIDES the current
 * CodeIgniter base_url function to support
 * CDN'ized content.
 */
function base_url($uri)
{
   $CI =& get_instance();

   $cdn = $CI->config->item('cdn_url');
   if (!empty($cdn))
      return $cdn . $uri;

   return $CI->config->base_url($uri);
}
{% endhighlight %}

3. Now lets set the CDN URL. Edit `applicaiton/config/config.php`. Add these new lines.
{% highlight php %}
/*
|--------------------------------------------------------------------------
| Content Delivery Network URL
|--------------------------------------------------------------------------
|
| Loading content like .js, .png, .css files on a CDN is much better.
| This allows you to support CDNs.
|
|  http://example.com/
|
| Use the normal base_url() function and upload your /assets and /content
| to the CDN.
|
*/
$config['cdn_url']   = '';
{% endhighlight %}

## Conclusion

So once you create your cdn_url, your `base_url` function will operate normally and use `cdn_url` instead of `base_url` if applicable. So all your pages will be referencing into the CDN. That was easy.
