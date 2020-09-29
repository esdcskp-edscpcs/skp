---
title: Application Security Training
layout: no-banner
permalink: /app-security-training/
---

A list of application security courses. Group prices are available for some of the courses.

<ul class="list-unstyled">
{% assign list_of_courses = site.data.app-security-training.training | sort_natural: "title" %}
{% for course in list_of_courses %}
  <li>
  <details>
    <summary>
      <h2 class="h3" id="{{ course.title | slugify }}">{{ course.title }}</h2>
    </summary>
	{{ course.details }}
  </details>
  </li>
{% endfor %}
</ul>