---
title: "Paper Notes"
permalink: /paper-notes/
layout: single
---

Here I collect my reading notes on papers in **distributed systems** and **databases**.

I focus on:
- the main ideas and contributions,  
- why the problem is important, and  
- what I learned that could influence how I design or evaluate systems.

{% assign paper_posts = site.posts | where_exp: "post", "post.categories contains 'paper-notes'" %}
{% for post in paper_posts %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  <span style="font-size: 0.9em; color: #777;">{{ post.date | date: "%b %d, %Y" }}</span>  
  {{ post.excerpt | strip_html | truncate: 160 }}
{% endfor %}
