---
title: Training
layout: no-banner
permalink: /app-security-training/
---

## Cloud Development

<p>
	<ul class="list-unstyled">
	{% assign list_of_courses = site.data.app-security-training.cloud | sort_natural: "title" %}
	{% for course in list_of_courses %}
	  <li>
	  <details>
		<summary>
		  <h3 id="{{ course.title | slugify }}">{{ course.title }}</h3>
		</summary>
		{{ course.details }}
	  </details>
	  </li>
	{% endfor %}
	</ul>
</p>

## Secure Software

<p>
	<ul class="list-unstyled">
	{% assign list_of_courses = site.data.app-security-training.securesoftware | sort_natural: "title" %}
	{% for course in list_of_courses %}
	  <li>
	  <details>
		<summary>
		  <h3 id="{{ course.title | slugify }}">{{ course.title }}</h3>
		</summary>
		{{ course.details }}
	  </details>
	  </li>
	{% endfor %}
	</ul>
</p>
