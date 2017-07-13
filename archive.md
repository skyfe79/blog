---
layout: default
title: Archive
permalink: /archive/
---

<!--<ul class="posts">
    {% for post in site.posts %}
      <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul> 
-->
<ul>
{% for post in site.posts %}
	{% capture month %}{{ post.date | date: '%m%Y' }}{% endcapture %}
	{% capture nmonth %}{{ post.next.date | date: '%m%Y' }}{% endcapture %}
		{% if month != nmonth %}
			{% if forloop.index != 1 %}</ul>{% endif %}
			<b>{{ post.date | date: '%B %Y' }}</b><ul>
		{% endif %}
	<li style="font-size: 14px"><span>{{ post.date | date: "%e" }}</span> &raquo; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</ul>