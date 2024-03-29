---
layout: page
refactor: true
icon: fas fa-rocket
order: 1
title: Projects
---

{% include lang.html %}

{% assign posts = site.posts | where_exp: 'item', 'item.project == true and item.hidden != true' %}

<div
  id="post-list"
  {% unless has_paginator %}
    class="mb-5"
  {% endunless %}
>
  {% for post in posts %}
    {% assign titleColor = "" %}
    {% assign contentColor = "" %}
    {% if post.blog == true %}
      {% assign titleColor = "LightSkyBlue" %}
      {% assign contentColor = "#5684A0" %}
    {% elsif post.project == true %}
      {% assign titleColor = "Gold" %}
      {% assign contentColor = "#A38A00" %}
    {% elsif post.tech == true %}
      {% assign titleColor = "Tomato" %}
      {% assign contentColor = "#A33F2E" %}
    {% endif %}

    <a href="{{ post.url | relative_url }}" class="card-wrapper">
      <div class="card post-preview flex-md-row-reverse">
        {% if post.image %}
          {% if post.image.lqip %}
            {% capture lqip %}lqip="{{ post.image.lqip }}"{% endcapture %}
          {% endif %}

          {% assign src = post.image.path | default: post.image %}
          {% unless src contains '//' %}
            {% assign src = post.img_path | append: '/' | append: src | replace: '//', '/' %}
          {% endunless %}

          {% assign alt = post.image.alt | default: 'Preview Image' %}

          <img src="{{ src }}" w="17" h="10" alt="{{ alt }}" {{ lqip }}>
        {% endif %}

        <div class="card-body d-flex flex-column">
          <h1 class="card-title my-2 mt-md-0" style='color: {{titleColor}}'>
            {{ post.title }}
          </h1>

          <div class="card-text post-content mt-0 mb-2">
            <p style='color: {{contentColor}}'>
              {% include no-linenos.html content=post.content %}
              {{ content | markdownify | strip_html | truncate: 200 | escape }}
            </p>
          </div>

          <div class="post-meta flex-grow-1 d-flex align-items-end">
            <div class="me-auto">
              <!-- posted date -->
              <i class="far fa-calendar fa-fw me-1"></i>
              {% include datetime.html date=post.date lang=lang %}

              <!-- categories -->
              {% if post.categories.size > 0 %}
                <i class="far fa-folder-open fa-fw me-1"></i>
                <span class="categories">
                  {% for category in post.categories %}
                    {{ category }}
                    {%- unless forloop.last -%},{%- endunless -%}
                  {% endfor %}
                </span>
              {% endif %}
            </div>

            {% if post.pin %}
              <div class="pin ms-1">
                <i class="fas fa-thumbtack fa-fw"></i>
                <span>{{ site.data.locales[lang].post.pin_prompt }}</span>
              </div>
            {% endif %}
          </div>
          <!-- .post-meta -->
        </div>
        <!-- .card-body -->
      </div>
    </a>
  {% endfor %}
</div>
<!-- #post-list -->