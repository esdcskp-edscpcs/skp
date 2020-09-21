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
	{% if item.topicitems %>
		<div class="panel-body">
			<ul>
		{% for topicitem in item.topicitems %}
				<li><a href="{{ topicitem.url }}">{{ topicitem.title }}</a></li>
		{% endfor %}
			</ul>
		</div>
	{% endif %}
</section>
	
{% endfor %}
