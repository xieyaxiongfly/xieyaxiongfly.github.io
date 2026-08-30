---
layout: page
title: Teaching
permalink: /teaching/
description: 
nav: true
nav_order: 4
# Categories are detected automatically from the `category` field of the files
# in _teaching/. The list below is only an *ordering* hint: whatever is listed
# here is shown first, in this order; any other course found in _teaching/ is
# appended afterwards. A brand new course therefore appears without editing
# this file -- list it here only if you care about where it sits on the page.
display_categories: [CSE589 Modern Networking Concepts, 
CSE610 Special Topics on Mobile Systems and Edge Intelligence, 
CSE610 Special Topics on Mobile Sensing & Mobile Networks, 
CSE710 Seminar on Wireless Networks, 
COS563 Wireless Networks (@Princeton University)]
# Categories listed here are never displayed (leftovers from the theme demo).
exclude_categories: [fun]
display_universities: [University at Buffalo, Princeton University] 
horizontal: false
---

{%- comment -%}
Build the ordered list of categories: the ones named in `display_categories`
first (only if they actually exist in the collection), then every remaining
category found in _teaching/, alphabetically. "|" is used as the join
delimiter because no course title contains it.
{%- endcomment -%}
{%- assign found_categories = site.teaching | map: "category" | compact | uniq | sort -%}
{%- assign ordered_categories = "" -%}
{%- for category in page.display_categories -%}
  {%- unless page.exclude_categories contains category -%}
    {%- if found_categories contains category -%}
      {%- assign ordered_categories = ordered_categories | append: category | append: "|" -%}
    {%- endif -%}
  {%- endunless -%}
{%- endfor -%}
{%- for category in found_categories -%}
  {%- unless page.display_categories contains category or page.exclude_categories contains category -%}
    {%- assign ordered_categories = ordered_categories | append: category | append: "|" -%}
  {%- endunless -%}
{%- endfor -%}
{%- assign teaching_categories = ordered_categories | split: "|" -%}

<!-- pages/projects.md -->
<div class="projects">
{%- if site.enable_project_categories and teaching_categories.size > 0 %}
  <!-- Display categorized projects -->
  {%- for category in teaching_categories %}
  <!-- <h2 class="category">{{ category }}</h2> -->
  <h4>{{ category }}</h4>
  {%- assign categorized_projects = site.teaching | where: "category", category -%}
  {%- assign sorted_projects = categorized_projects | sort: "importance" %}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
  <hr> 
  {% endfor %}

{%- else -%}
<!-- Display projects without categories -->
  {%- assign sorted_projects = site.teaching | sort: "importance" -%}
  <!-- Generate cards for each project -->
  {% if page.horizontal -%}
  <div class="container">
    <div class="row row-cols-2">
    {%- for project in sorted_projects -%}
      {% include projects_horizontal.html %}
    {%- endfor %}
    </div>
  </div>
  {%- else -%}
  <div class="grid">
    {%- for project in sorted_projects -%}
      {% include projects.html %}
    {%- endfor %}
  </div>
  {%- endif -%}
{%- endif -%}
</div>
