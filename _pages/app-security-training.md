---
title: Application Security Training
layout: no-banner
permalink: /app-security-training/
---

<ul class="list-unstyled">
{% for course in site.data.app-security-training.training %}
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