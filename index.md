---
layout: default
title: Home
---

### Intro
With a background in computer networking, software, finance, machine learning, and data security,
I help businesses leverage the power of data to drive innovation and redevelopment.

As an Enterprise Solutions Architect at Immuta: I lead workshops and evaluations to help
prospects make the right buying decisions, support the success of customers by collaborating
with partners and product teams, and travel to build relationships through meaningful conversations.

In addition to my role at Immuta, I am also a General Manager at Vraid Systems,
where I deliver client facing projects. My goal is that technology should serve people,
not the other way around.

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
