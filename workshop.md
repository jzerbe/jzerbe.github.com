---
layout: default
permalink: /workshop.html
title: from the lab
---

<div data-role="page">
    {% include header.html %}

    <div data-role="content">
        <ul data-role="listview" data-inset="true">
            {% for post in site.posts %}
                {% if post.tags contains 'workshop' and post.tags contains 'preview' %}
                    {% comment %} liquid is missing negation {% endcomment %}
                {% elsif post.tags contains 'workshop' %}
                    <li>
                        <a href="{{ post.url }}" title="{{ post.title }} permalink">
                            <h2>{{ post.title }}</h2>
                            <p>{{ post.date | date_to_string }}</p>
                        </a>
                    </li>
                {% endif %}
            {% endfor %}
        </ul>
    </div>

    {% include footer_workshop.html %}
</div>
