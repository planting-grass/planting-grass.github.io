---
layout: post
title: "김희원"          permalink: /til/김희원/
---

<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323&family=Noto+Sans+KR:wght@400;700&display=swap" rel="stylesheet">

<style>
/* --- 스타일 시작 --- */
:root {
  --bg-color: #f0f2f5;
  --grid-color: #cfd8dc;
  --card-bg: #ffffff;
  --main-green: #00ff41; /* 해커 네온 그린 */
  --text-secondary: #555;
  --sat-color: #2563eb;
  --sun-color: #dc2626;
  --border-retro: 2px solid #000;
  --shadow-retro: 4px 4px 0px #000;
}

/* 전체 컨테이너: 모눈종이 배경 */
.til-container {
  font-family: 'Noto Sans KR', sans-serif;
  background-color: var(--bg-color);
  background-image: 
    linear-gradient(var(--grid-color) 1px, transparent 1px),
    linear-gradient(90deg, var(--grid-color) 1px, transparent 1px);
  background-size: 20px 20px;
  padding: 20px;
  border-radius: 8px;
  border: 1px solid #ddd;
}

/* 헤더 영역 */
.grass-header {
  text-align: center;
  margin-bottom: 30px;
  padding: 20px 0;
}

.grass-title {
  font-family: 'Press Start 2P', cursive;
  font-size: 1.8em;
  color: #000;
  text-transform: uppercase;
  margin: 0;
  text-shadow: 3px 3px 0px var(--main-green);
  line-height: 1.4;
}

.grass-subtitle {
  font-family: 'VT323', monospace;
  font-size: 1.4em;
  background: #000;
  color: var(--main-green);
  display: inline-block;
  padding: 4px 12px;
  margin-top: 10px;
  border-radius: 4px;
}

/* 연도 구분선 */
.year-divider {
  font-family: 'Press Start 2P', cursive;
  font-size: 1.2em;
  border-bottom: 3px solid #000;
  margin: 40px 0 20px 0;
  padding-bottom: 10px;
  color: #000;
}

/* 월 뱃지 */
.month-badge {
  display: inline-block;
  font-family: 'VT323', monospace;
  font-size: 1.6em;
  font-weight: bold;
  background: var(--main-green);
  color: #000;
  padding: 2px 10px;
  border: 2px solid #000;
  box-shadow: 3px 3px 0 #000;
  margin: 20px 0 10px 0;
  transform: rotate(-1deg);
}

/* 달력 테이블 */
.grass-calendar-wrapper {
  background: var(--card-bg);
  border: var(--border-retro);
  box-shadow: var(--shadow-retro);
  padding: 15px;
  margin-bottom: 30px;
  border-radius: 4px;
  overflow-x: auto;
}

.grass-calendar {
  width: 100%;
  border-collapse: separate;
  border-spacing: 4px;
  table-layout: fixed;
  min-width: 300px;
}

.grass-calendar th {
  font-family: 'VT323', monospace;
  font-size: 1.3em;
  color: var(--text-secondary);
  padding-bottom: 8px;
  border-bottom: 2px dashed #eee;
}

.grass-calendar td {
  height: 45px;
  text-align: center;
  vertical-align: middle;
  background: #f8f9fa;
  border: 1px solid #e9ecef;
  border-radius: 6px;
  font-size: 1em;
  position: relative;
  transition: transform 0.1s;
}

/* 주말 색상 */
.grass-calendar td.sat { color: var(--sat-color); background: #eff6ff; }
.grass-calendar td.sun { color: var(--sun-color); background: #fef2f2; }

/* 잔디(포스트 있는 날) 스타일 - 핵심 */
.grass-calendar td.grass {
  background: #222 !important;
  border: 2px solid #000;
  box-shadow: 2px 2px 0px var(--main-green);
}

.grass-calendar td.grass a {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  color: var(--main-green);
  font-weight: bold;
  font-family: 'VT323', monospace;
  font-size: 1.4em;
}

/* 잔디 호버 효과 */
.grass-calendar td.grass:hover {
  transform: translate(-1px, -1px);
  box-shadow: 4px 4px 0px var(--main-green);
}

.day-number {
  color: #ccc;
  font-family: 'VT323', monospace;
  font-size: 1.1em;
}
</style>

<div class="til-container">
  <div class="grass-header">
    <h1 class="grass-title">Hee-won's TIL</h1>
    <div class="grass-subtitle">Commit: Daily_Log</div>
  </div>

{% assign posts = site.posts | where_exp: "post", "post.path contains '_posts/til/김희원'" %} 
{% assign grouped = posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in grouped %}
  <div class="year-divider">💾 {{ year.name }} Archive</div>
  
  {% assign months = year.items | group_by_exp: "post", "post.date | date: '%m'" %}
  {% for month in months %}
    {% assign month_num = month.name | plus: 0 %}
    
    <div class="month-badge">{{ month_num }}월</div>

    {% assign days = month.items | group_by_exp: "post", "post.date | date: '%d'" %}

    {% assign first_post = month.items | first %}
    {% assign year_num = first_post.date | date: "%Y" | plus: 0 %}
    {% assign month_num_str = month.name %}
    {% assign first_day_str = year_num | append: '-' | append: month_num_str | append: '-01' %}
    {% assign first_day = first_day_str | date: "%w" | plus: 0 %}

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

    <div class="grass-calendar-wrapper">
      <table class="grass-calendar">
        <thead>
          <tr>
            <th>SUN</th>
            <th>MON</th>
            <th>TUE</th>
            <th>WED</th>
            <th>THU</th>
            <th>FRI</th>
            <th>SAT</th>
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
                    <td class="{{ cell_class | strip }} grass">
                      <a href="{{ day_group.items[0].url }}">{{ day }}</a>
                    </td>
                  {% else %}
                    <td class="{{ cell_class | strip }}">
                      <span class="day-number">{{ day }}</span>
                    </td>
                  {% endif %}
                  
                  {% assign day = day | plus: 1 %}
                {% endif %}
              {% endfor %}
            </tr>
          {% endfor %}
        </tbody>
      </table>
    </div>
  {% endfor %}
{% endfor %}
</div>
