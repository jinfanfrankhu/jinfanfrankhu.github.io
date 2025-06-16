---
layout: post
title: "Testing Post Features"
date: 2025-06-16
---

<article class="post">
  <h1>{{ page.title }}</h1>

  <div class="entry">
    {{ content }}
  </div>

  <div class="date">
    Written on {{ page.date | date: "%B %e, %Y" }}
  </div>

  {% include disqus.html %}
</article>