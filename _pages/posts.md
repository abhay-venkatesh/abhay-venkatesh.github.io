---
permalink: /posts
layout: page
title: Posts
---

<a href="{{ '/' | relative_url }}">{{ site.title }}</a>

<ul>
{% for post in site.posts %}
  <li><a href="{{ post.url | relative_url }}">{{ post.title }}</a> ({{ post.date | date: "%B %Y" }})</li>
{% endfor %}
</ul>
