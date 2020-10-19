---
title: Guides
layout: no-banner
permalink: /guides/
---

{% for guide in site.data.guides.listofguides %}

<section class="panel panel-default">
	<div class="panel-heading">
		<h2 class="panel-title" id="{{ guide.title | slugify }}">{{ guide.title }}</h2>
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
