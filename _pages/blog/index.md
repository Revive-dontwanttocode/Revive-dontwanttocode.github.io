---
layout: default
title: "Chih-Hsi Chen's Blog"
permalink: /blog/
author_profile: true
---

# 📝 Blog Posts

{% if paginator.posts.size > 0 %}
{% for post in paginator.posts %}

## [{{ post.title }}]({{ post.url | relative_url }})

**{{ post.date | date: "%B %d, %Y" }}**

{% if post.excerpt %}
{{ post.excerpt | strip_html | truncatewords: 50 }}
{% endif %}

[Read More: ]({{ post.url | relative_url }})

---

{% endfor %}

{% if paginator.total_pages > 1 %}
<div class="pagination">

{% if paginator.previous_page %}
[Newer Posts]({{ paginator.previous_page_path | relative_url }})
{% endif %}

Page {{ paginator.page }} of {{ paginator.total_pages }}

{% if paginator.next_page %}
[Older Posts]({{ paginator.next_page_path | relative_url }})
{% endif %}

</div>
{% endif %}

{% else %}

No blog posts yet.

{% endif %}