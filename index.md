---
layout: page
title: Steven Lu
tagline: tech blog
---
{% include JB/setup %}

<p>
Hey, I'm Steven Lu -- A full-stack developer who loves to create innovative products.
I also love constantly learning about technologies that I've never encountered before and sharing what I've gained to everyone.
If you would like to know more about me, you should visit my <a href="http://stevenlu.com/">website</a>.
</p>

<hr>

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
