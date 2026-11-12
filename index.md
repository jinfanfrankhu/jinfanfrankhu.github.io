<p>I'm a student at Yale University thinking of majoring in any of Math, CS, and linguistics. I like writing, I like cats, and I like learning. As of recent, I've been motivated by AI safety.

<p>Check out my <a href="/about">about me</a>, <a href="/projects">projects</a>, or some of my <a href="/posts">posts</a>.</p>

<!-- Recent posts as a standalone section -->
<h2>Recent Posts</h2>
<ul class="custom-post-list" style="padding-left: 0;">
  {% for post in site.posts limit:3 %}
    {% assign cat_index = forloop.index0 | modulo: 4 | plus: 1 %}
    <li style="margin-bottom: 8px;">
      <img src="/images/cat{{ cat_index }}.png" class="cat-bullet" alt="cat {{ cat_index }}" />
      <div class="post-content" style="display: inline-block;">
        <a href="{{ post.url | relative_url }}"><strong>{{ post.title }}</strong></a>
      </div>
    </li>
  {% endfor %}
</ul>

<h2>A Recent Project</h2>
<div class="project-card">
  <a href="https://jinfanhu.com/projects/gay-voice">
    <img src="/images/projects/gayvoice/cover.png" alt="Gay voice image" class="project-image"/>
  </a>
  <h2 class="project-title">
    <a href="https://jinfanhu.com/projects/gay-voice">A Neural Network Informed Study on the Gay Voice</a>
  </h2>
  <p class="project-description">An ongoing project with CUNY Queens on identifying linguistic features that correspond with both self-reported sexuality and listener-perceived sexuality in Gen Z youth.</p>
</div>
