---
layout: page
title: Posts
permalink: /posts/
description: "Jinfan Frank Hu's posts and musings about whatever he's thinking about. Look out for philosophy, linguistics, and of course, cats."
---

<ul class="custom-post-list">
  {% for post in site.posts %}
    {% assign cat_index = forloop.index0 | modulo: 4 | plus: 1 %}
    <li>
      <img src="/images/cat{{ cat_index }}.png" class="cat-bullet" alt="cat {{ cat_index }}" />
      <div class="post-content">
        <a href="{{ post.url | relative_url }}"><strong>{{ post.title }}</strong></a>
        <div class="post-date">{{ post.date | date: "%B %d, %Y" }}</div>
      </div>
    </li>
  {% endfor %}
</ul>