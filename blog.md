--- 
title: Blog
layout: default
---
# {{ page.title }}

PhilTel's blog posts are listed here!

{% for post in site.posts %}
{% assign currentdate = post.date | date: "%Y-%m-%d" %}
{%- if post.title and post.deprecated == nil or post.deprecated == "false" -%}
<p>{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a></p>
{%- endif -%}
{%- if post.title and post.deprecated == true -%}
<p>{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}"><s>{{ post.title }}</s></a></p>
{%- endif -%}
{% endfor %}