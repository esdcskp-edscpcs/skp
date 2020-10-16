---
title: Guides
layout: no-banner
permalink: /guides/
---

<h2>Guides</h2>

{% for guide in site.data.guides %}

	<section class="panel panel-default">
		<div class="panel-heading">
			<h3 class="panel-title" id="{{ guide.title | slugify }}">{{ guide.title }}</h3>
		</div>
		<div class="panel-body">
			{% if guide.description %}
				<p>{{ guide.description }}</p>
			{% endif %}
			{% if guide.location %}
				<p><strong><a href="{{ guide.location }}" target="_blank"><span class="glyphicon glyphicon-file"></span> See the guide</a></strong></p>
			{% endif %}
		</div>
	</section>

{% endfor %}
