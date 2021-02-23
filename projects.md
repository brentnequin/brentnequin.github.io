---
layout: projects
title: Projects
permalink: /projects/
---

# Projects
<ul class="section-list">
  {% for section in site.data.projects %}
  <li class="section-block">
    <h2>{{ section.section-title }}</h2>
    {% for project in section.projects %}
      <li class="project-block">
        <i class="fab fa-{{ project.icon }}"></i>
        {% if project.url %}
        <a href="{{ project.url }}"><h3>{{ project.name }}</h3></a>
        {% else %}
          <h3>{{ project.name }}</h3>
        {% endif %}
        <span>{{ project.description }}</span>
      </li>
    {% endfor %}
  </li>
  {% endfor %}
</ul>
