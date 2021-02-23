---
layout: projects
title: Projects
permalink: /projects/
---

# Projects
<ul class="section-list">
  {% for section in site.data.projects %}
  <h2>{{ section.section-title }}</h2>
  {% for project in section.projects %}
    <li class="project-block">
      {% if project.url %}
      <a rel="external" href="{{ project.url }}"><h2>{{ project.name }}</h2></a>
      {% else %}
        <h2>{{ project.name }}</h2>
      {% endif %}
      <p>{{ project.description }}</p>
    </li>
  {% endfor %}
  {% endfor %}
</ul>
