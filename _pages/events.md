---
title: Events
layout: no-banner
permalink: /events/
---

{% if site.data.events.next %}
<div class="well">
    <h2 id="next-event">Our Next Event</h2>
    <p class="lead">The next event is planned for <strong>{{ site.data.events.next.date }}</strong></p>
    <p>{{ site.data.events.next.overview }}</p>
</div>
{% endif %}

<h2>Past Events</h2>

{% for event in site.data.events.past %}

<section class="panel panel-default">
    <div class="panel-heading">
        <h3 class="panel-title" id="{{ event.topic | slugify }}">{{ event.topic }}</h3>
    </div>
    <div class="panel-body">
        <div class="pull-right mrgn-rght-lg text-muted small">
            <dl>
                <dt>Presented on:</dt>
                <dd>{{ event.date }}</dd>
            </dl>
        </div>
    {% if event.recording %}
        <p><strong><a href="{{ event.recording }}" target="_blank"><span class="glyphicon glyphicon-facetime-video"></span> View the recording</a></strong></p>
    {% endif %}
    {% if event.presentation %}
        <p><a href="{{ event.presentation }}" target="_blank"><span class="glyphicon glyphicon-file"></span> View the presentation slides</a></p>
    {% endif %}
    {% if event.resources %}
        <p>Resources:</p>
        <ul>
        {% for resource in event.resources %}
            <li><a href="{{ resource.link }}" target="_blank">{{ resource.title }}</a></li>
        {% endfor %}
        </ul>
    {% endif %}
    </div>
</section>

{% endfor %}
