---
title: Vacation Project 2020
subtitle: During a two week vacation, I set out to try crafting a self-healing data stream as a passion project.  These post capture my progress from inception to review.
# description:
featured_image: /images/2020/06/vacationlearning-stepfunctionplan.png
---

{% for post in site.categories.vacation-project-2020-06 %}
<article class="blog-post">

  <div class="blog-post__header">
    <h2 class="blog-post__title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
    <p class="blog-post__subtitle">{{ post.date | date: '%B %d, %Y' }}</p>
  </div>

  {% if post.featured_image %}
  <a href="{{ post.url | relative_url }}" class="blog-post__image" style="background-image: url({{ post.featured_image | relative_url }});"></a>
  {% endif %}

  <div class="blog-post__content">
    <p>{{ post.excerpt }}</p>
    <p><a href="{{ post.url | relative_url }}" class="button">Read More</a>
{% endfor %}
