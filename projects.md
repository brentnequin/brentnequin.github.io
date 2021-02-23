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
        {% if project.url %}
        <a rel="external" href="{{ project.url }}"><p>{{ project.name }}</p></a>
        {% else %}
          <p>{{ project.name }}</p>
        {% endif %}
        <p>{{ project.description }}</p>
      </li>
    {% endfor %}
  </li>
  {% endfor %}
</ul>
