---
title: Developer Community
layout: no-banner
permalink: /developercommunity/
---

<div>
	<p>
		The Developer Community section provides you with important information about security and can give you guidance and help to implement security by design, during the development of your product.
	</p>
</div>

{% for item in site.data.developercommunity.training %}

<section class="panel panel-default">
	<div class="panel-heading">
		<h2 class="panel-title" id="{{ item.topic | slugify }}">{{ item.topic }}</h2>
	</div>
	{% if item.topic-items %>
		<div class="panel-body">
			<ul>
		{% for topic-item in item.topic-items %}
				<li><a href="{{ topic-item.url }}">{{ topic-item.title }}</a></li>
		{% endfor %}
			</ul>
		</div>
	{% endif %}
</section>
	
{% endfor %}
