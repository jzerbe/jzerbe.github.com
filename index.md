---
layout: default
title: Home
---

## Career Profile
With a background in software development, machine learning, and data solutions,
I help businesses leverage the power of data to drive innovation, efficiency, and growth.
As an Enterprise Solutions Architect at Immuta, I lead technical demos, workshops,
and evaluations to showcase the value of our data security platform.

In addition to my role at Immuta, I am also a Director at Vraid Systems Limited,
where I create new technology assisted solutions in various domains, such as construction,
law, real estate, and markets; to scratch my serial entrepreneur itch.

### Experiences
__Enterprise Solutions Architect__
Immuta, Inc. Boston, Massachusetts, US. Jul 2021.

__Director__
Vraid Systems Limited. Kelowna, British Columbia, CA. Jun 2020.

__Senior Solutions Architect__
Koverse, Inc. Seattle, Washington, US. Apr 2018 - Jul 2021.

__Senior Software Engineer__
Tendril Networks, Inc. Boulder, Colorado, US. Aug 2016 - Apr 2018.

__Senior Full Stack Engineer__
Cheddar Up, Inc. Denver, Colorado, US. Aug 2015 - Aug 2016.

__Senior Software Engineer__
Workiva, Inc. Ames, Iowa, US. Jan 2013 - Jul 2015.

__Service Developer__
Ninja Metrics Inc. Minneapolis, Minnesota, US. Jun 2012 - Jan 2013.

__Engineering Intern__
MediaBeacon, Inc. Minneapolis, Minnesota, US. May 2011 - Aug 2011.

__Freelance Developer__
Vraid Systems. St. Paul, Minnesota, US. Jun 2007 - Mar 2011.

### Certifications
__AIARE Avalanche Rescue__
American Institute for Avalanche Research and Education (AIARE). Telluride, Colorado, US. Mar 2023.

__Investment Advisor Representative__
Colorado Department of Regulatory Agencies. Denver, Colorado, US. Nov 2022.

__Series 65__
Financial Industry Regulatory Authority, Inc. Washington, D.C., US. Aug 2022.

__AIARE 1__
American Institute for Avalanche Research and Education (AIARE). Telluride, Colorado, US. Dec 2018.

__Bachelor of Science, Computer Science__
University of Minnesota - Twin Cities. Minneapolis, Minnesota, US. Sep 2009 – May 2012.

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
