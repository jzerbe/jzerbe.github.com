---
layout: default
permalink: /travels.html
title: extended adventures
---

<div data-role="page">
    {% include header.html %}

    <div data-role="content">
        <ul data-role="listview" data-inset="true">
            {% for post in site.posts %}
                {% if post.tags contains 'travels' and post.tags contains 'preview' %}
                    {% comment %} liquid is missing negation {% endcomment %}
                {% elsif post.tags contains 'travels' %}
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

    {% include footer_travels.html %}
</div>
