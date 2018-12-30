---
layout: page
title: Dynamics 365 Data Export Service 
---

<ul>
{% for post in site.posts %}
  {% for category_name in post.categories %}
    {% if category_name == "API" %}
      <li>
        <b><a href="{{ post.url }}">{{ post.title }}</a></b>
        <p>{{ post.excerpt }}</p>
    </li>
    {% endif %}
  {% endfor %}
{% endfor %}
</ul>