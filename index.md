---
layout: default
title: Home
---

### Intro
With a background in construction, energy, finance, and logistics, I help businesses leverage the power of data and software to drive innovation and redevelopment. I focus on cash flow positive projects and ignore the other noise.

I delight in having conversations with interesting people - ask those how and why questions, help me understand something new, let's celebrate growth. My superpower is an efficient focus on solving new problems; sales adjacent roles seem to be the best place to exercise this.

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
