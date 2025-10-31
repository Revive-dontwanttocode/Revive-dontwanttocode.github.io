---
layout: page
title: "Chih-Hsi Chen's Blog"
permalink: /blog/
---

{% for post in site.posts %}
- [{{ post.title }}]({{ post.url }}) — {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}