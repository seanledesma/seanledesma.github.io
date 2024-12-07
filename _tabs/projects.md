---
layout: 
permalink: /projects/
icon: fas fa-rocket
order: 1
---

<style>
  .project-preview {
    padding: 1rem;
    margin-bottom: 1.5rem;
    background: var(--card-bg);
    border-radius: 0.625rem;
    transition: all 0.3s ease-in-out;
    border: 1px solid rgba(158, 158, 158, 0.2);  /* Explicit light border */
  }
  
  .project-preview:hover {
    background: var(--card-hovor-bg, var(--mask-bg));
    transform: translateX(0.25rem);
  }

  .project-preview h2 {
    font-size: 1.4rem;
    margin-top: 0;
    margin-bottom: 0.4rem;
  }
  
  .project-preview a {
    color: var(--text-color);
    text-decoration: none !important;
  }

  .project-preview .post-meta {
    font-size: 0.85rem;
  }
</style>

<div class="post-content px-1">
  {% assign sorted_projects = site.projects | sort: 'date' | reverse %}
  {% for project in sorted_projects %}
    <a href="{{ project.link }}">
      <div class="project-preview">
        <h2>{{ project.title }}</h2>
        <div class="post-content">
          <p style="color: var(--text-color);">{{ project.description }}</p>
        </div>
        <div class="post-meta text-muted">
          <i class="far fa-calendar fa-fw"></i>
          {{ project.date | date: "%b %d, %Y" }}
        </div>
      </div>
    </a>
  {% endfor %}
</div>