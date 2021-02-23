---
layout: projects
title: Projects
permalink: /projects/
---

# Projects

{% for project in site.data.projects %}
  <div class="project-block">
    <h2>{{ project.name }}</h2>
    <p>{{ project.description }}</p>
  </div>

{% endfor %}
