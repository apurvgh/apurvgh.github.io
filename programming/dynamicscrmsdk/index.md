---
layout: post
title: Dynamics 365 SDK
---

<ul>
{% for post in site.posts %}
  {% for category_name in post.categories %}
    {% if category_name == "dynamicscrmsdk" %}
      <li>
        <b><a href="{{ post.url }}">{{ post.title }}</a></b>
        <p>{{ post.excerpt }}</p>
    </li>
    {% endif %}
  {% endfor %}
{% endfor %}
</ul>
