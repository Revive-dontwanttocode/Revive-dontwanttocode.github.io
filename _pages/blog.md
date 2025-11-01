---
layout: default
title: "Chih-Hsi Chen's Blog"
permalink: /blog/
author_profile: true
---

<div class="page__content">
  <h1>📰 Blog Posts</h1>

{% for post in paginator.posts %}
  <div class='paper-box'>
    <div class='paper-box-image'>
      <img src='{{ post.image | default: "/images/500x300.png" }}' alt="{{ post.title }}" width="100%">
    </div>
    <div class='paper-box-text'>
      <a href="{{ post.url }}" style="font-weight:600;">{{ post.title }}</a><br>
      <span class="blog-meta">{{ post.date | date: "%B %-d, %Y" }}</span>
      <p>{{ post.excerpt | strip_html | truncate: 160 }}</p>
    </div>
  </div>
  {% endfor %}

  <!-- Pagination -->
  <div class="pagination">
    {% if paginator.previous_page %}
      <a href="{{ paginator.previous_page_path }}" class="previous">← Newer Posts</a>
    {% endif %}
    {% if paginator.next_page %}
      <a href="{{ paginator.next_page_path }}" class="next">Older Posts →</a>
    {% endif %}
  </div>
</div>
