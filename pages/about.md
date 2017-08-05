---
layout: page
title: About
description: 
keywords: Zhang Huan, 智能作坊
comments: true
menu: 关于
permalink: /about/
---

## Richard

* 一生放荡不羁爱自由

## 联系

* GitHub：[@octopus](https://github.com/octopusy)
* 微博: [@octopus](http://weibo.com/octopus)
## Skill Keywords

#### Software Engineer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_software_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>

#### Mobile Developer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_mobile_app_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>

#### Windows Developer Keywords
<div class="btn-inline">
    {% for keyword in site.skill_windows_keywords %}
    <button class="btn btn-outline" type="button">{{ keyword }}</button>
    {% endfor %}
</div>
