---
title: "Ciprian Turcu Blog"
description: "I write about coding, mostly focused on python and ML nowadays"
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
