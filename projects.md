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
        {% if project.url %}
        <a href={{ project.url }}><h2>{{ project.name }}</h2></a>
        {% else %}
          <h2>{{ project.name }}</h2>
        {% endif %}
        <p>{{ project.description }}</p>
      </div>
    </li>
  {% endfor %}
</ul>
