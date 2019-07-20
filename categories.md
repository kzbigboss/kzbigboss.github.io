---
layout: page
title: Posts by Category
permalink: /categories/
---
{% for category in site.categories %}
  <h3>{{ category[0] }}</h3>
  <ul>
    {% for post in category[1] %}
      <li><a href="{{ post.url }}">{{post.date | date: "%Y-%m-%d"}}: {{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
