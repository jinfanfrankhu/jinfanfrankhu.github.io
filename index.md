---
layout: default
title: "Jinfan Frank Hu | Phillips Academy Andover | Linguistics & AI"
description: "Welcome to the personal website of Jinfan Frank Hu, a student at Phillips Academy exploring computational linguistics, AI research, NLP projects, coding, language preservation, and occasional posts about cats, hockey, and cheese."
---

<h1>Hi. My name is Jinfan Frank Hu. Welcome to my website!</h1>
<p>I’m a senior at Phillips Academy Andover exploring language and AI.</p>
<p>Check out my <a href="/about">about me</a>, <a href="/projects">projects</a>, or some of my <a href="/posts">posts</a>.</p>

<p>I won't lie to you and tell you that I've got it all figured out, but I love making stuff, fixing problems, and understanding things. I also love my cats. Even if they're a little naughty. Or if they scratch me. (The black one is Mittens and the white one is Coco!)</p>

<p>I've broken two bones (thanks hockey), I speak English, Mandarin, Spanish, and French decent enough (will still get yelled at by a Parisian though).</p>

<!-- Two-column layout with collage and recent posts -->
<div style="display: flex; align-items: flex-start; gap: 30px; margin-top: 20px;">
  <!-- Left: Profile collage -->
  <div style="flex-shrink: 0;">
    <img src="{{ '/images/ProfileCollage.jpg' | relative_url }}" alt="Collage of Jinfan Frank Hu" style="width: 150px; height: 150px; object-fit: cover;">
  </div>

  <!-- Right: Recent posts -->
  <div style="flex: 1;">
    <h2>Recent Posts</h2>
    <ul class="custom-post-list">
      {% for post in site.posts limit:3 %}
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
  </div>
</div>