---
title: References
layout: default
permalink: /references/
---

{% for resource in site.data.references.resources %}
<section class="panel panel-default">
    <div class="panel-heading">
        <h3 class="panel-title" id="{{ site.data.references.topic | slugify }}">{{ resource.topic }}</h3>
    </div>
    <div class="panel-body">
		<ul>
		{% for topicitem in resource.topicitems %}
			<li><a href="{{ topicitem.url }}">{{ topicitem.title }}</a></li>
		{% endfor %}
		</ul>
	</div>
<section>
{% endfor %}
