---
title: Application Security Tools
layout: default
permalink: /app-security-tools/
---

## ESDC Standard Security Tools

IT Security requires development teams to integrate into their CI/CD Pipeline at least the following types of security testing using the recommended tools. The following tools are standard within the departement and integrates with the [**Threadfix**](https://threadfix.it/) corporate Application Vulnerabilities Management (AVM) tool in order to consolidate the results of all security tools scanning throughout the department. 

Implementation guides for our standard security tools can be found next to the product as soon as they are available to distribute.

To request a licence for any of the following security tools, please [contact us via the MS Teams Security Champions Network](https://teams.microsoft.com/l/channel/19%3a7fb48ff71f584a309817c64b3d599a77%40thread.tacv2/Licenses?groupId=bea80905-7f0f-432d-9a83-60561c1efcd2&tenantId=9ed55846-8a81-4246-acd8-b1a01abfc0d1).

NOTE: Developers can use any other supported security tools to fit their needs, but IT Security will require that the tool(s) selected integrate with the corporate Threadfix AVM solution. See [Additional recommended security tools](#additional-recommended-security-tools) for more details.

<ul class="list-unstyled">
{% for type in site.data.app-security-tools.standard %}
  <li>
  <details>
    <summary>
      <h2 class="h3" id="{{ type.focus | slugify }}">{{ type.focus }}</h2>
    </summary>
    {% if type.definition %}
      {{ type.definition %}}
    {% endif %}
    {% if type.tools %}
		<p><strong>Corporate Standard(s):</strong></p>
		<ul class="list-group list-inline row mrgn-lft-0 mrgn-rght-0">
		  {% assign list_of_tools = type.tools | sort_natural: "name" %}
		  {% for tool in list_of_tools %}
			<li class="list-group-item col-md-4 brdr-rds-0">
			  <h3 class="list-group-item-heading" id="{{ tool.name | slugify }}">{{ tool.name }}</h3>
			  <ul class="list-group-item-text list-inline">
				{% if tool.availability %}
				  <li>{{ tool.availability }}</li>
				{% endif %}
				{% if tool.details %}
				  <li><a href="{{ tool.details }}" target="_blank">Details</a></li>
				{% endif %}
				{% if tool.guide %}
				  <li><a href="{{ tool.guide }}" target="_blank">Guide</a></li>
				{% endif %}
			  </ul>
			</li>
		  {% endfor %}
		</ul>
	{% else %}
		<p><strong>ESDC has not procured any tool of this type so far.</strong></p>
	{% endif %}
  </details>
  </li>
{% endfor %}
</ul>
## Additional Recommended Security Tools

The following tools also integrate with the Threadfix corporate AVM tool and can be used instead of the standard tools listed above however, IT Security will not take ownership of these tools and may not be able to provide support for it.
<ul class="list-unstyled">
{% for type in site.data.app-security-tools.supported %}
  <li>
  <details>
    <summary>
      <h2 class="h3" id="{{ type.focus | slugify }}">{{ type.focus }}</h2>
    </summary>
    {% if type.tools %}
		<p><strong>Additional Recommended Tool(s):</strong></p>
		<ul class="list-group list-inline row mrgn-lft-0 mrgn-rght-0">
		  {% assign list_of_tools = type.tools | sort_natural: "name" %}
		  {% for tool in list_of_tools %}
			<li class="list-group-item col-md-4 brdr-rds-0">
			  <h3 class="list-group-item-heading">{{ tool.name }}<br />{{ tool.product }}</h3>
			  <ul class="list-group-item-text list-inline">
				{% if tool.pricing %}
				  <li>{{ tool.pricing }}</li>
				{% endif %}
				{% if tool.details %}
				  <li><a href="{{ tool.details }}" target="_blank">Details</a></li>
				{% endif %}
				{% if tool.guide %}
				  <li><a href="{{ tool.guide }}" target="_blank">Guide</a></li>
				{% endif %}
			  </ul>
			</li>
		  {% endfor %}
		</ul>
	{% else %}
		<p><strong>ESDC has not procured any tool of this type so far.</strong></p>
	{% endif %}
  </details>
  </li>
{% endfor %}
</ul>
