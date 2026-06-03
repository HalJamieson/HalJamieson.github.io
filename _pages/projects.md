---
layout: single
title: "Projects"
permalink: /projects/
author_profile: true
---

<ul>
  {% assign sorted_projects = site.projects | sort: 'date' | reverse %}
  {% for project in sorted_projects %}
    <li style="margin-bottom: 1.5rem; list-style-type: none;">
      <strong><a href="{{ project.url }}" style="font-size: 1.15rem; text-decoration: none;">{{ project.title }}</a></strong>
      
      {% if project.date %}
        <span style="font-size: 0.85rem; color: #888; margin-left: 0.6rem; font-weight: normal;">
          ({{ project.date | date: "%B %Y" }})
        </span>
      {% endif %}
      
      <div style="font-style: italic; color: #666; margin-top: 0.3rem; line-height: 1.4;">
        {{ project.excerpt }}
      </div>
    </li>
  {% endfor %}
</ul>