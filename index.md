---
layout: default
pagination:
  enabled: true
---

<h1>Home page</h1>
{% for post in paginator.posts %}

<h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
{{ post.excerpt }}
<p><a href="{{ post.url | relative_url }}">Read More</a></p>
{% endfor %}

<div class="pagination">
  {% if paginator.previous_page %}
  <a href="{{ paginator.previous_page_path }}">Previous</a>
  {% else %}
  <span>Previous</span>
  - 
  {% endif %} {% if paginator.next_page %}
  <a href="{{ paginator.next_page_path }}">Next</a>
  {% else %}
  <span>Next</span>
  {% endif %}
</div>
