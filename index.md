<h1>Hi. My name is Jinfan Frank Hu. Welcome to my website!</h1>
<p>I’m a senior at Phillips Academy Andover exploring language and AI.</p>
<p>Check out my <a href="/about">about me</a>, <a href="/projects">projects</a>, or some of my <a href="/posts">posts</a>.</p>

<p>I won't lie to you and tell you that I've got it all figured out, but I love making stuff, fixing problems, and understanding things. I also love my cats. Even if they're a little naughty. Or if they scratch me. (The black one is Mittens and the white one is Coco!)</p>

<p>I've broken two bones (thanks hockey), I speak English, Mandarin, Spanish, and French decent enough (will still get yelled at by a Parisian though).</p>

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
