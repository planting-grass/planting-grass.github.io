---
layout: post
title: "김소희"
permalink: /til/김소희/
---

<style>
/* 잔디(Grass) 컨셉 스타일 */
.grass-calendar {
  width: 100%;
  table-layout: fixed; /* 열 비율 고정 */
  border-collapse: collapse;
  margin-bottom: 2em;
  background: #e8f5e9;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px #b2dfdb55;
}
.grass-calendar th, .grass-calendar td {
  width: 14.285%; /* 100% / 7일 */
  height: 40px;
  text-align: center;
  vertical-align: middle;
  border: 1px solid #c8e6c9;
  font-size: 1em;
  padding: 0;
  box-sizing: border-box;
}
.grass-calendar th {
  background: #a5d6a7;
  color: #33691e;
  font-weight: bold;
}
.grass-calendar td {
  background: #f1f8e9;
  transition: background 0.2s;
}
.grass-calendar td.sat {
  background: #e3f2fd !important;
  color: #1976d2;
}
.grass-calendar td.sun {
  background: #ffebee !important;
  color: #d32f2f;
}
.grass-calendar a {
  color: inherit;
  text-decoration: none;
  display: block;
  width: 100%;
  height: 100%;
}
.grass-calendar a:hover {
  text-decoration: underline;
}
.grass-title {
  font-size: 2em;
  color: #388e3c;
  margin-bottom: 0.5em;
  font-weight: bold;
  letter-spacing: 0.05em;
  text-shadow: 0 2px 4px #c8e6c9;
}
.grass-month {
  font-size: 1.2em;
  color: #43a047;
  margin: 1.5em 0 0.5em 0;
  font-weight: bold;
  letter-spacing: 0.03em;
}
.grass-calendar .grass-dot {
  display: inline-block;
  width: 28px;
  height: 28px;
  line-height: 28px;
  background: #81c784;
  color: #fff;
  font-weight: bold;
  border-radius: 50%;
  text-align: center;
  vertical-align: middle;
  font-size: 1em;
  margin: 2px 0;
}
</style>

<div class="grass-title">🌱 잔디 달력 🌱</div>

{% assign posts = site.posts | where_exp: "post", "post.path contains '_posts/til/김소희'" %} 
{% assign grouped = posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in grouped %}
{{ year.name }}
----------------
{% assign months = year.items | group_by_exp: "post", "post.date | date: '%m'" %}
{% for month in months %}
{% assign month_num = month.name | plus: 0 %}
<div class="grass-month">{{ month_num }}월</div>
{% assign days = month.items | group_by_exp: "post", "post.date | date: '%d'" %}

{% comment %} 해당 월의 첫 날짜와 마지막 날짜를 동적으로 계산 {% endcomment %}
{% assign first_post = month.items | first %}
{% assign year_num = first_post.date | date: "%Y" | plus: 0 %}
{% assign month_num_str = month.name %}
{% assign first_day_str = year_num | append: '-' | append: month_num_str | append: '-01' %}
{% assign first_day = first_day_str | date: "%w" | plus: 0 %}

{% comment %} 각 월의 날짜 수를 수동으로 설정 (윤년 고려) {% endcomment %}
{% if month_num == 1 or month_num == 3 or month_num == 5 or month_num == 7 or month_num == 8 or month_num == 10 or month_num == 12 %}
  {% assign last_day = 31 %}
{% elsif month_num == 2 %}
  {% if year_num | modulo: 400 == 0 %}
    {% assign last_day = 29 %}
  {% elsif year_num | modulo: 100 == 0 %}
    {% assign last_day = 28 %}
  {% elsif year_num | modulo: 4 == 0 %}
    {% assign last_day = 29 %}
  {% else %}
    {% assign last_day = 28 %}
  {% endif %}
{% else %}
  {% assign last_day = 30 %}
{% endif %}

<table class="grass-calendar">
  <thead>
    <tr>
      <th>일</th>
      <th>월</th>
      <th>화</th>
      <th>수</th>
      <th>목</th>
      <th>금</th>
      <th>토</th>
    </tr>
  </thead>
  <tbody>
    {% assign day = 1 %}
    {% assign total_cells = last_day | plus: first_day %}
    {% assign total_rows = total_cells | divided_by: 7 %}
    {% if total_cells | modulo: 7 != 0 %}
      {% assign total_rows = total_rows | plus: 1 %}
    {% endif %}

    {% for row in (1..total_rows) %}
      <tr>
        {% for col in (0..6) %}
          {% if row == 1 and col < first_day %}
            <td></td>
          {% elsif day > last_day %}
            <td></td>
          {% else %}
            {% assign day_str = day | prepend: '0' | slice: -2, 2 %}
            
            {% assign day_group = nil %}
            {% for group in days %}
              {% if group.name == day_str %}
                {% assign day_group = group %}
                {% break %}
              {% endif %}
            {% endfor %}

            {% assign cell_class = "" %}
            {% if col == 0 %}
              {% assign cell_class = cell_class | append: "sun" %}
            {% elsif col == 6 %}
              {% assign cell_class = cell_class | append: "sat" %}
            {% endif %}

            {% if day_group and day_group.items.size > 0 %}
              {% assign cell_class = cell_class | append: " grass" %}
              <td class="{{ cell_class | strip }}">
                <a href="{{ day_group.items[0].url }}"><span class="grass-dot">{{ day }}</span></a>
              </td>
            {% else %}
              <td class="{{ cell_class | strip }}">{{ day }}</td>
            {% endif %}
            
            {% assign day = day | plus: 1 %}
          {% endif %}
        {% endfor %}
      </tr>
    {% endfor %}
  </tbody>
</table>
{% endfor %}

{% endfor %}