---
title: Projects
layout: page
permalink: /projects/
icon: fas fa-rocket
order: 1
--- 

{% assign sorted_projects = site.projects | sort: 'date' | reverse %}
{% for project in sorted_projects %}
  <div class="project-item">
    <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
    <p>{{ project.description }}</p>
    <p><small>Posted on {{ project.date | date: "%B %d, %Y" }}</small></p>
  </div>
{% endfor %}