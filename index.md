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



<script src="{{site.url}}/js/freewall.js"></script>

<script type="text/javascript">
			var wall = new freewall("#freewall");
			wall.reset({
				selector: '.brick',
				animate: true,
				cellW: 200,
				cellH: 'auto',
				onResize: function() {
					wall.fitWidth();
				}
			});
			
			var images = wall.container.find('.brick');
			var length = images.length;
			images.css({visibility: 'hidden'});
			images.find('img').load(function() {
				-- length;
				if (!length) {
					setTimeout(function() {
						images.css({visibility: 'visible'});
						wall.fitWidth();
					}, 505);
				}
			});

		</script>
