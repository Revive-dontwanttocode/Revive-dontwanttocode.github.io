---
layout: default
title: "Chih-Hsi Chen's Blog"
permalink: /blog/
author_profile: true
---

<div class="page__content" markdown="1">

# Blog Posts

{% for post in site.posts %}
<div class='paper-box'>
  <div class='paper-box-image'>
    <div>
      {% if post.badge %}
      <div class="badge">{{ post.badge }}</div>
      {% endif %}
      {% if post.image %}
      <img src='{{ post.image }}' alt="{{ post.title }}" width="100%">
      {% else %}
      <img src='/images/500x300.png' alt="{{ post.title }}" width="100%">
      {% endif %}
    </div>
  </div>
  <div class='paper-box-text' markdown="1">

[{{ post.title }}]({{ post.url }})

{{ post.date | date: "%B %-d, %Y" }}

- {{ post.excerpt | strip_html | truncate: 160 }}

  </div>
</div>
{% endfor %}

</div>