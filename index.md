---
layout: default
title: Home
---

### Intro
With a background in software development, machine learning, and data solutions,
I help businesses leverage the power of data to drive innovation, efficiency, and growth.

As an Enterprise Solutions Architect at Immuta: I lead workshops and evaluations
to help prospects make the right buying decisions,
support the success of customers by collaborating with product teams,
and travel to build relationships through meaningful conversations.

In addition to my role at Immuta, I am also a Director at Vraid Systems Limited,
where I create new technology assisted alternatives in various domains,
such as construction, law, and real estate.
My goal is that technology should serve people, not the other way around.

---
---

<div class="posts">
  {% assign posts_by_order = site.posts | sort: "custom_order" %}
  {% for post in posts_by_order %}
  <div class="post">
    <h3 class="post-title">
      <a href="{{ post.url }}">
        {{ post.title }}
      </a>
    </h3>

    <span class="post-date">{{ post.tags | array_to_sentence_string }}</span>
  </div>
  {% endfor %}
</div>
