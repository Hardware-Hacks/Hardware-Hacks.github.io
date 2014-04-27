---
layout: default
title: Home
---

<div id="home">
  <h1>Blog Posts</h1>

  <div id="freewall"  class="free-wall">
    {% for post in site.posts %}	
      <div class="brick"><img src="http://vnjs.net/www/project/freewall/example/i/photo/2.jpg" width="100%"><div class="info"><h3>{{ post.date | date_to_string }}</h3></div><a href="{{ post.url }}">{{ post.title }}</a></div>
    {% endfor %}
  </div>

</div>



