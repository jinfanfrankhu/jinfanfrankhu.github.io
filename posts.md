---
layout: page
title: Posts
permalink: /posts/
---

<ul class="custom-post-list">
  {% for post in site.posts %}
    {% assign cat_index = forloop.index0 | modulo: 4 | plus: 1 %}
    <li>
      <img src="/images/cat{{ cat_index }}.png" class="cat-bullet" alt="cat {{ cat_index }}" />
      <a href="{{ post.url | relative_url }}"><strong>{{ post.title }}</strong></a>:&nbsp;<span>{{ post.date | date: "%B %d, %Y" }}</span>
    </li>
  {% endfor %}
</ul>