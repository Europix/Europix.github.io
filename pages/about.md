---
layout: page
title: About
description: qwq
keywords: Europix
comments: true
menu: 关于
permalink: /about/
---

Beacons are the lighthouse for those wanderers.


Searching for a Beacon right now.


## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
