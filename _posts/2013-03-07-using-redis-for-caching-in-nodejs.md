---
layout: post
title: "Using Redis for Caching in Node.js"
description: ""
category:
tags: []
---


Your web service is slow and you want to store things into memory for cache. Luckily, it's pretty easy to do this in your Node.js Express application.

## Steps

First you want to install your Redis module from npm. Also make sure to edit your `package.json` with the appropriate module and its version.

{% highlight bash %}
npm install redis
{% endhighlight %}

Then include it in the file where your caching your data. You usually want to put this in your "controller" or whatever is handling the requests to the client. Normally, we will put the Redis cache before your `res.send()` call.

{% highlight javascript %}
var redis = require('redis');
{% endhighlight %}

Next you want to connect your Redis module to something useful and create your handler. I normally use Nodejitsu so I access the environment variables. It should be something similar to Heroku or any service you use, just make sure you set the environment variables or change the code to something it'll connect to properly.

{% highlight javascript %}
var client = redis.createClient(process.env.REDIS_PORT, process.env.REDIS_HOST);
if (typeof process.env.REDIS_PASSWORD)
	client.auth(process.env.REDIS_PASSWORD);
{% endhighlight %}

When a request comes in, we want to check if we have a response already prepared for them in Redis. To do so, we apply the following code. Don't worry about expiration yet, that is covered when we set the actual data.

{% highlight javascript %}
var handler = function() {};

client.get('some-key-value', function (err, result) {
	if (err || !result)
	   execute_function(zipcode, handler);
	else
		res.end(result);
});
{% endhighlight %}

Basically what this does is ask Redis for data given for the key. Make you're key unique to the request. Normally what I'd do stack all the parameters together and `sha1` them together, that way I have a unique key that I can rebuild and request later on. For example, if my request depends on a zipcode, I'll be making my key exactly the zipcode. If the key does not contain any data, we'll execute our function that normally generates this data. In this case `execute_function` will be the function I will call.

Last, we need to store the compiled data, note that we're passing a `handler` function over to our executed function, `execute_function`. We're now gonna replace that function to something that stores to Redis.

{% highlight javascript %}
var handler = function(response)
{
	response = JSON.stringify(response);
	client.setex('some-key-value', 21600, response);
	res.end(response);
};
{% endhighlight %}

We'll note here that Redis will only store strings so we need to change our response data into a JSON string. It doesn't know what Javascript objects are, so don't give it that! We then want to use `.setex` which means set value and expire after 'X' seconds. In this case, based on the key I created, I stored a value of `response` that will expire in 6 hours or 21600 seconds. Now because its set in Redis, my `client.get` function will capture and return data and instead respond directly with the data retrieved from Redis. After 6 hours, this data will expire in Redis and return nothing to the `client.get` function resulting in needing to re-run `execute_function` and `handler` when it is complete and restore the data.