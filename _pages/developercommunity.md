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
	<div class="panel-body">
		{% for trainingitem in site.data.developercommunity.topic-items %}
			<ul>
				<li><a href="{{ trainingitem.url }}">{{ trainingitem.title }}</a></li>
			</ul>
		{% endfor %}
	</div>
</section>
	
{% endfor %}
