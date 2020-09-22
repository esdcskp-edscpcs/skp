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
	<div>
		{{ course.details }}
	</div>
  </details>
  </li>
{% endfor %}
</ul>
<p>
{% for course in site.data.app-security-training.training %}
	{{ course.details }}
{% endfor %}
</p>