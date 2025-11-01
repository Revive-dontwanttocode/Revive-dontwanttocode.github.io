---
layout: default
title: "Chih-Hsi Chen's Blog"
permalink: /blog/
author_profile: true
pagination:
  enabled: true
  per_page: 10
  collection: "posts"
---

<div class="page__content" markdown="1">

# 📰 Blog Posts

{% for post in paginator.posts %}
<div class='paper-box'>
  <div class='paper-box-image'>
    <div>
      {% if post.image %}
      <img src='{{ post.image }}' alt="{{ post.title }}" width="100%">
      {% else %}
      <img src='/images/500x300.png' alt="{{ post.title }}" width="100%">
      {% endif %}
    </div>
  </div>
  <div class='paper-box-text' markdown="1">

[{{ post.title }}]({{ post.url }})

📅 {{ post.date | date: "%B %-d, %Y" }}

{{ post.excerpt | strip_html | truncate: 160 }}

  </div>
</div>
{% endfor %}

<!-- Pagination -->
<div class="pagination" style="margin-top:30px;">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="btn btn--inverse">← Newer</a>
  {% endif %}
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="btn btn--inverse">Older →</a>
  {% endif %}
</div>

</div>
