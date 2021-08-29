---
layout: page
title: Portfolio
permalink: /porfolio/
---

This page lists my featured projects.

{% for post in site.categories["project"] %}
  <article class="portfolio-article">
    <h2>
      <a href="{{ post.url }}">
        {{ post.title }}
      </a>
    </h2>
    {{ post.summary }}
  </article>
{% endfor %}