---
title: "Coding With Chip - Home page"
description: "I write about coding, mostly focused on python and ML nowadays. A blog by Ciprian Turcu"
---

{% for post in site.posts %}

<h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
{{ post.excerpt }}
<p><a href="{{ post.url | relative_url }}">Read More</a></p>
{% endfor %}
{% for post in paginator.posts %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
  <p>{{ post.excerpt }}</p>
{% endfor %}

{% if paginator.total_pages > 10 %}

  <div class="pagination">
    {% if paginator.previous_page %}
      <a href="{{ paginator.previous_page_path }}">Previous</a>
    {% else %}
      <span>Previous</span>
    {% endif %}
    {% if paginator.next_page %}
      <a href="{{ paginator.next_page_path }}">Next</a>
    {% else %}
      <span>Next</span>
    {% endif %}
  </div>
{% endif %}
