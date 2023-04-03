---
title: "Ciprian Turcu Blog"
description: "I write about coding, mostly focused on python and ML nowadays"
---

{% for post in site.posts %}

<h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
{{ post.content }}
{% endfor %}
