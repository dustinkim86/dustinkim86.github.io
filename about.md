---
layout: page
title: About
permalink: /about/
---

2019년 1월부터 IT에 입문한 늦깍이 입문자 입니다. Python과 R을 이용한 데이터 분석과 개발을 공부하고 있습니다.

* [GitHub](https://www.github.com/dustinkim86/)

## 포스트 수

총 포스트 수: {{ site.posts | size }}개

{% assign number_of_posts = 0 %}
{% for post in site.posts %}{% assign currnet_year = post.date | date: "%Y" %}{% assign previous_year = post.previous.date | date: "%Y" %}{% assign number_of_posts = number_of_posts | plus: 1 %}{% if currnet_year != previous_year %}
* {{ currnet_year }}년 : {{ number_of_posts }}개의 포스트{% assign number_of_posts = 0 %}{% endif %}{% endfor %}
