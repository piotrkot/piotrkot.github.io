---
layout: default
title: Programming blog by piotrkot
---

<ul>
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &rarr; <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

