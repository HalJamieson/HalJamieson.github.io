---
layout: single
title: "Projects"
permalink: /projects/
author_profile: true
---

Explore some of my featured technical projects and architecture deep dives below:

<ul>
  {% for project in site.projects %}
    <li>
      <strong><a href="{{ project.url }}">{{ project.title }}</a></strong> — <em>{{ project.excerpt }}</em>
    </li>
  {% endfor %}
</ul>