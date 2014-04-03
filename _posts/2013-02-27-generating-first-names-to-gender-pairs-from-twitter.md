---
layout: post
title: "Generating First Names to Gender Pairs from Twitter"
description: ""
category:
tags: []
---


## Introduction

So, I am working on some semantics algorithm that can help my company figure out how well their traction is. For right now, my focus is on Twitter, as it is more open to people's thoughts (in my opinion) and is very real time.

## Build

The first thing that I needed to complete was grabbing public user profiles from Twitter. I easily done this through two API endpoints and crawled my way through. After generating 4 million result sets, I think it was enough to stop at. A sample user response from Twitter looks something like this (that's me by the way):

{% highlight javascript %}
{
    "id": 14695072,
    "favourites_count": 7,
    "profile_link_color": "009999",
    "profile_background_image_url_https": "https://twimg0-a.akamaihd.net/images/themes/theme14/bg.gif",
    "friends_count": 111,
    "entities": { }
    "profile_image_url_https": "https://twimg0-a.akamaihd.net/profile_images/2737790984/62812e3dbbff54ae781e4964cd2cdbe9_normal.jpeg",
    "followers_count": 1884,
    "profile_use_background_image": true,
    "utc_offset": -18000,
    "screen_name": "StevenLu",
    "profile_text_color": "333333",
    "id_str": "14695072",
    "is_translator": false,
    "statuses_count": 935,
    "name": "Steven Lu",
    "lang": "en",
    "contributors_enabled": false,
    "created_at": "Thu May 08 02:26:13 +0000 2008",
    "default_profile": false,
    "profile_sidebar_border_color": "eeeeee",
    "notifications": false,
    "time_zone": "Eastern Time (US & Canada)",
    "protected": false,
    "profile_image_url": "http://a0.twimg.com/profile_images/2737790984/62812e3dbbff54ae781e4964cd2cdbe9_normal.jpeg",
    "url": "http://stevenlu.com",
    "geo_enabled": false,
    "profile_background_tile": true,
    "listed_count": 11,
    "profile_sidebar_fill_color": "efefef",
    "profile_banner_url": "https://twimg0-a.akamaihd.net/profile_banners/14695072/1350768231",
    "verified": false,
    "following": false,
    "follow_request_sent": false,
    "location": "United States",
    "profile_background_color": "131516",
    "default_profile_image": false,
    "description": "Guy. Student at Rutgers. Engineer at Noise.",
    "profile_background_image_url": "http://a0.twimg.com/images/themes/theme14/bg.gif"
}
{% endhighlight %}

As you can see, some of the data is great, some of it not so much. To build semantics on this user object is quite difficult since there's no gender, no birthday, age or clear-cut location.

## Parsing

So, from the post title, I had to figure out someone's gender from that object, so I decided to parse the `description` field. In my description, I saw that I had the word "Guy" in there and from that I can assume I am a male.

This isn't 100% accurate because there are plenty of people who'll put in "Have a very loving wife" in their description. In this case, I assume that they are a female because of the word "wife", but in fact they are a male. However, the number of people who have descriptions like that is small and is why its okay for me to have these bad answers since there is a small result set.

Now if I have a description with a female and male keyword, I'd cut out there because I'm not 100% positive about their gender.

My list of keywords isn't great, but it did the job.
{% highlight text %}
male, boy, brother, father, gentleman, dude, bro, man, mr., guy, sir, son, grandfather, grandson
female, girl, sister, mother, woman, miss, ms., mrs., daughter, grandmother, granddaughter, lady, babe, chick, gal, princess, queen
{% endhighlight %}

## Logging Names to Gender

Now after I figure out if that Twitter user is a male or female, I sniff the `name` field and split it up for their first name and store the name to gender pair in my database. I assume that if other Twitter user first names match what is in my database, they are also that same gender.

Now at first, I thought this wasn't gonna work very well. But in fact, I was able to get a pretty good list of first names to genders purely from my result set. Take a look. Note that this is a CSV and you can import this into your spreadsheet editor or into your database for pragmatic use. The `count` field shows how many times I've logged that name, `gender` of "1" is female, "2" is male, `confidence` shows the percentage off I'm away, zero being 100%. It's ordered by occurrence and confidence. [Here's the list of names and their genders](https://gist.github.com/sjlu/5058141).

To get more of an understanding on what I generate, here's the query I use:
{% highlight sql %}
SELECT COUNT(*) AS count, ROUND(AVG(gender)) as gender, STDDEV(gender) as confidence, name FROM twitter_names_gender GROUP BY name HAVING count > 15 AND confidence <= 0.35 ORDER BY confidence ASC, count DESC
{% endhighlight %}

## Conclusion

After running this for a couple of hours, I had about 750,000 twitter users I could determine genders of and found out 66% of them were male while 33% of those were female.