---
title: Projects
layout: page
permalink: /projects/
icon: fas fa-rocket
order: 1
--- 

<style>
/* Custom CSS for Project Boxes */
.card-deck {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.card {
  width: 100%;
  max-width: 650px;
  margin-bottom: 1rem;
  box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  border: 1px solid #ddd;
  border-radius: 5px;
}

.card-title {
  font-size: 1.25rem;
  font-weight: bold;
}

.card-text {
  margin: 0.5em 0;
}

.post-meta {
  color: #888;
  font-size: 0.875rem;
}

/* Dark Mode Styles */
@media (prefers-color-scheme: dark) {
  .card {
    background-color: #333;
    border-color: #444;
    box-shadow: 0 4px 8px rgba(255, 255, 255, 0.1);
  }

  .card-title, .card-text, .post-meta {
    color: #ddd;
  }

  .post-meta {
    color: #bbb;
  }
}
</style>

<div class="card-deck">
  {% assign sorted_projects = site.projects | sort: 'date' | reverse %}
  {% for project in sorted_projects %}
    <div class="card">
      <div class="card-body">
        <h5 class="card-title">
          <a href="{{ project.link }}" target="_blank">
            {{ project.title }}
          </a>
        </h5>
        <p class="card-text">{{ project.description }}</p>
        <p class="post-meta">Posted on {{ project.date | date: "%B %d, %Y" }}</p>
      </div>
    </div>
  {% endfor %}
</div>

