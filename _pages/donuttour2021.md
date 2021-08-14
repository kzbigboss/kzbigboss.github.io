---
title: Donut Tour 2021 Posts
subtitle: I try to visit a different donut shop each month in 2021.  Check out my progress!
# description: A stunning personal blog Jekyll theme with an image-focused design.
featured_image: /images/backgrounds/Donuts 05.jpg
---

{% for post in site.categories.donut-tour-2021 %}
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
