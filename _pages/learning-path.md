---
title: "Learning Path: Distributed Systems & Databases"
permalink: /learning-path/
layout: single
---

This page summarizes my learning path for **distributed systems** and **databases**.  
It currently consists of two long-form posts:

- how I built a structured roadmap for fundamentals, and  
- how I connected theory with hands-on projects and systems.

Below are all posts tagged as part of my learning path.

{% assign learning_posts = site.posts | where_exp: "post", "post.categories contains 'learning-path'" %}
{% for post in learning_posts %}
- **[{{ post.title }}]({{ post.url | relative_url }})**  
  <span style="font-size: 0.9em; color: #777;">{{ post.date | date: "%b %d, %Y" }}</span>  
  {{ post.excerpt | strip_html | truncate: 160 }}
{% endfor %}
