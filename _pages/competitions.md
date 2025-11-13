---
title: "Competitions & Project Write-ups"
permalink: /competitions/
layout: single
---

This section includes write-ups for **competitions, benchmarks, and projects**
related to distributed systems and data-intensive applications.

I use these write-ups to reflect on:
- the problem setting and constraints,  
- system design and architecture choices, and  
- what went well, and what I would do differently next time.

{% assign comp_posts = site.posts | where_exp: "post", "post.categories contains 'competitions'" %}
{% for post in comp_posts %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  <span style="font-size: 0.9em; color: #777;">{{ post.date | date: "%b %d, %Y" }}</span>  
  {{ post.excerpt | strip_html | truncate: 160 }}
{% endfor %}
