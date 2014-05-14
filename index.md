---
layout: default
title: Home
---

<div class="container">
    <div class="page-header">
        <center><h1 id="timeline">Blog Post Timeline</h1></center>
    </div>
    <ul class="timeline">
    {% assign left = true %} 
     {% for post in site.posts %}

    {% if left %}
    	<li>
    	{% assign left = false %}
    {% else %}
    	<li class="timeline-inverted">
    	{% assign left = true %}
    {% endif %}
        
          <div class="timeline-badge warning"><i class="glyphicon glyphicon-check"></i></div>
          <div class="timeline-panel">
            <div class="timeline-heading">
              <h4 class="timeline-title">{{ post.title }}</h4>
              <hr>
              <p><small class="text-muted"><i class="glyphicon glyphicon-time"></i> {{ post.date }}</small></p>
            </div>
            <div class="timeline-body">
              <p>{{ post.excerpt }}</p>
            <hr>
              <p><span class="glyphicon glyphicon-circle-arrow-right"></span>
              <a href="{{ post.url }}">Read more...</a>
              </p>
             
            </div>
          </div>
        </li>
    
    
    {% endfor %}
     
     <!-- End loop through for each post -->
     
    </ul>
</div>


