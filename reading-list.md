---
layout: page
title: What I'm reading
permalink: /reading-list/
---

<div class="img-wrapper">
    <img class="title-img" src="/images/reading-cat.gif">
</div>

{% if site.pinboard_user %}
<section>
  <ul id="pinboard-linkroll">Please wait for the internet fairies to finish cavorting...</ul>
    
  <p class="rss-subscribe">
    browse my <a href="http://pinboard.in/u:{{ site.pinboard_user }}">pinboard</a><br>
    subscribe <a href="https://feeds.pinboard.in/rss/u:kablamo/">via RSS</a>
  </p>
</section>
<script type="text/javascript">
  var linkroll = 'pinboard-linkroll'; //id target for pinboard list
  var pinboard_user = "{{ site.pinboard_user }}"; //id target for pinboard list
  var pinboard_count = {{ site.pinboard_count }}; //id target for pinboard list
  (function(){
    var pinboardInit = document.createElement('script');
    pinboardInit.type = 'text/javascript';
    pinboardInit.async = true;
    pinboardInit.src = '{{ root_url }}/js/pinboard.js';
    document.getElementsByTagName('head')[0].appendChild(pinboardInit);
  })();
</script>
{% endif %}

