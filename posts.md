---
layout: page
title: Posts
permalink: /posts/
description: "Jinfan Frank Hu's posts and musings about whatever he's thinking about. Look out for philosophy, linguistics, and of course, cats."
---

See all posts <a href="/allposts/">here,</a> or sort them by tags <a href="/posttags/">here!</a>

<h2>Deep Dives</h2>
<ul class="custom-post-list">
  {% assign deep_posts = site.posts | where_exp: "post", "post.tags contains 'deep'" | slice: 0, 5 %}
  {% for post in deep_posts %}
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

<br><br>

<h2>The Little Things</h2>
<ul class="custom-post-list">
  {% assign joy_posts = site.posts | where_exp: "post", "post.tags contains 'little'" | slice: 0, 5 %}
  {% for post in joy_posts %}
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
