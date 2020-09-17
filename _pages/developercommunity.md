---
title: Developer Community
layout: no-banner
permalink: /developercommunity/
---

<section class="panel panel-default">
    <div class="panel-heading">
		<ul>
			{% for item in site.data.developercommunity.menuitems %}
				<li><a href="{{ item.url }}">{{ item.title }}</a></li>
			{% endfor %}
		</ul>
    </div>
</section>
