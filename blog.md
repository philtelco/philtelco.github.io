--- 
title: Blog
layout: default
---
# {{ page.title }}

PhilTel's blog posts are listed here!

{% for post in site.posts %}
{% assign currentdate = post.date | date: "%Y-%m-%d" %}
{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
{% endfor %}