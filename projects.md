---
layout: projects
title: Projects
permalink: /projects/
---

# Projects

A selection of projects I have worked on or am currently working on. You can find these projects on my [GitHub profile](https://github.com/brentnequin).

<ul class="section-list">
  {% for section in site.data.projects %}
  <li class="section-block">
    <h2>{{ section.section-title }}</h2>
    {% for project in section.projects %}
      <li class="project-block">
        <i class="fab fa-{{ project.icon }}"></i>
        {% if project.url %}
        <a href="{{ project.url }}"><span>{{ project.name }}</span></a>
        {% else %}
          <span>{{ project.name }}</span>
        {% endif %}
        <span>{{ project.description }}</span>
      </li>
    {% endfor %}
  </li>
  {% endfor %}
</ul>
