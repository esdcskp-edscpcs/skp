---
title: Application Security Training
layout: no-banner
permalink: /app-security-training/
---

<ul class="list-unstyled">
{% for product in site.data.security-champions.products %}
  <li>
  <details>
    <summary>
      <h2 class="h3" id="{{ name | slugify }}">{{ product.name }}</h2>
    </summary>
    {% if product.nominatedby %}
      <p>
		<span><strong>Nominated by:</strong></span> 
        {% for nominator in product.nominatedby %}
          <span class="label label-primary"><a href="mailto:{{ nominator.name }}">{{nominator.name}}</a></span>
        {% endfor %}
      </p>
    {% endif %}
    <p>
		<span><strong>Nominees:</strong></span>
	</p>
	<ul class="list-group list-inline row mrgn-lft-0 mrgn-rght-0">
      {% for nominee in product.nominees %}
        <li class="list-group-item col-md-4 brdr-rds-0">
          <h3 class="list-group-item-heading" id="{{ nominee.name | slugify }}"><a href="mailto:{{ nominee.name }}">{{ nominee.name }}</a></h3>
          <ul class="list-group-item-text list-inline">
            {% if nominee.phone %}
              <li>{{nominee.phone}}</li>
            {% endif %}
          </ul>
        </li>
      {% endfor %}
    </ul>
  </details>
  </li>
{% endfor %}
</ul>