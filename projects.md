---
layout: projects
title: Projects
permalink: /projects/
---

# Projects
<ul>
  {% for project in site.data.projects %}
    <li>
      <div class="project-block">
        <h2>{{ project.name }}</h2>
        <p>{{ project.description }}</p>
      </div>
    </li>
  {% endfor %}
</ul>
