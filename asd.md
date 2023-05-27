---
layout: default
title: "Coding With Chip - Home page"
description: "I write about coding, mostly focused on python and ML nowadays. A blog by Ciprian Turcu"
---

<h1>Home page</h1>
<!-- All posts -->
{% for post in paginator.posts %}

  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p>{{ post.excerpt }}</p>
{% endfor %}
<!-- navigation -->
{% if paginator.next_page %}
  <a href="{{ paginator.next_page_path }}">Next</a>
{% endif %}
{% if paginator.previous_page %}
  <a href="{{ paginator.previous_page_path }}">Previous</a>
{% endif %}
