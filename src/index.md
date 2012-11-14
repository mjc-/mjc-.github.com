---
layout: default
title: Only failure makes us experts 
tagline: you take your bitz and you make stuff
---

<div class="row">
  <div class="span8">
    <h2>Recent Posts</h2>
  </div>
  <div class="span4">
    <h2>Sidebar</h2>
  </div>
</div>

<div class="row">
  <div class="span4">
    <div class="posts">

      {% for post in site.posts limit:3 %}
      <h4><a href="{{ post.url }}">{{ post.title }}</a></h4>
      <p><span class="datetime"><small>{{ post.date | date_to_string }}</small></span></p>
      <p>{{ post.content | strip_html | truncatewords: 75 }}</p>
      <p><a class="more" href="{{ post.url }}">continue reading &raquo;</a></p>
      <br />
      {% endfor %}

    </div>
  </div>

  <div class="span4">
    <div class="posts">
      
      {% for post in site.posts limit:3 offset:3 %}
      <h4><a href="{{ post.url }}">{{ post.title }}</a></h4>
      <p><span class="datetime"><small>{{ post.date | date_to_string }}</small></span></p>
      <p>{{ post.content | strip_html | truncatewords: 75 }}</p>
      <p><a class="more" href="{{ post.url }}">continue reading &raquo;</a></p>
      <br />
      {% endfor %}
      
    </div>
    
  </div>
  <div class="span4">
      <div id="tag-cloud">
      {{ site | tag_cloud }}
      </div>
  </div>
</div>

<div class="row">
  <div class="span12">
    <blockquote>
      <p>I have discovered that there are two types of command interfaces in the world of computing: good interfaces and user interfaces.</p>
      <small>djb in <cite title="Qmail Security Guarantee">Qmail Security Guarantee</cite></small>
    </blockquote>
  </div>
</div>

