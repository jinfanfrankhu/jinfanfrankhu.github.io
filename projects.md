---
layout: page
title: Projects
permalink: /projects/
---

<div class="projects-container">
  {% for project in site.projects %}
    <div class="project-card">
      <a href="{{ project.url | relative_url }}">
        <img src="{{ project.image }}" alt="{{ project.title }} image" class="project-image"/>
      </a>
      <h2 class="project-title">
        <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
      </h2>
      <p class="project-description">{{ project.description }}</p>
    </div>
    <hr class="project-divider"/>
  {% endfor %}
</div>