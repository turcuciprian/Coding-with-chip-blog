---
title: "Ciprian Turcu Blog"
description: "I write about coding, mostly focused on python and ML nowadays"
---

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    </li>
  {% endfor %}
</ul>
