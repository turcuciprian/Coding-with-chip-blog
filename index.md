---
title: "Coding With Chip Blog"
description: "I write about coding, mostly focused on python and ML nowadays. A blog by Ciprian Turcu"
---

{% for post in site.posts %}

<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
{{ post.excerpt }}
{% endfor %}
