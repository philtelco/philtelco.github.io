--- 
title: Blog
layout: default
---
# {{ page.title }}

PhilTel's blog posts are listed here!

{% for post in site.posts %}
{% assign currentdate = post.date | date: "%Y-%m-%d" %}
{%- if post.title and post.deprecated == nil or post.deprecated == "false" -%}
{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}">{{ post.title }}</a>
{% else %}
{{ currentdate }} - <a href="{{ post.url }}" title="{{ post.title }}"><s>{{ post.title }}</s></a><br><br>
{%- endif -%}
{% endfor %}