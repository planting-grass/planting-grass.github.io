---
layout: post
title: "김희원"
permalink: /til/김희원/
---

<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323&family=Noto+Sans+KR:wght@400;700&display=swap" rel="stylesheet">

<style>
/* --- Y2K & Modern Variables --- */
:root {
  --bg-color: #f0f2f5;
  --grid-color: #e0e0e0;
  --card-bg: #ffffff;
  --main-green: #00ff41; /* 해커 느낌의 네온 그린 */
  --deep-green: #008f11;
  --text-primary: #333;
  --text-secondary: #666;
  --sat-color: #3b82f6;
  --sun-color: #ef4444;
  --border-style: 2px solid #000;
  --shadow-retro: 4px 4px 0px #000;
}

/* 전체 컨테이너 배경 (모눈종이 패턴) */
.til-container {
  font-family: 'Noto Sans KR', sans-serif;
  background-color: var(--bg-color);
  background-image: 
    linear-gradient(var(--grid-color) 1px, transparent 1px),
    linear-gradient(90deg, var(--grid-color) 1px, transparent 1px);
  background-size: 20px 20px;
  padding: 2rem;
  border-radius: 12px;
}

/* 타이틀 스타일 (픽셀 폰트 + 글리치 느낌) */
.grass-header {
  text-align: center;
  margin-bottom: 2rem;
}

.grass-title {
  font-family: 'Press Start 2P', cursive; /* 픽셀 폰트 */
  font-size: 1.5rem;
  color: #000;
  text-transform: uppercase;
  margin-bottom: 0.5rem;
  text-shadow: 2px 2px 0px var(--main-green);
  letter-spacing: -1px;
}

.grass-subtitle {
  font-family: 'VT323', monospace;
  font-size: 1.2rem;
  color: var(--text-secondary);
  background: #000;
  color: #fff;
  display: inline-block;
  padding: 0.2rem 0.8rem;
  border-radius: 4px;
}

/* 연도/월 구분선 */
.year-divider {
  font-family: 'Press Start 2P', cursive;
  font-size: 1.2rem;
  border-bottom: 4px solid #000;
  margin: 3rem 0 1rem 0;
  padding-bottom: 10px;
  color: #000;
}

.month-badge {
  display: inline-block;
  font-family: 'VT323', monospace;
  font-size: 1.5rem;
  font-weight: bold;
  background: var(--main-green);
  color: #000;
  padding: 2px 12px;
  border: 2px solid #000;
  box-shadow: 2px 2px 0 #000;
  margin: 1.5rem 0 1rem 0;
  transform: rotate(-2deg); /* 살짝 삐딱하게 */
}

/* 달력 테이블 스타일 */
.grass-calendar-wrapper {
  background: var(--card-bg);
  border: var(--border-style);
  box-shadow: var(--shadow-retro);
  padding: 1rem;
  margin-bottom: 2rem;
  overflow-x: auto; /* 모바일 대응 */
}

.grass-calendar {
  width: 100%;
  border-collapse: separate; /* 셀 간격 분리 */
  border-spacing: 4px; /* 셀 사이 여백 */
  table-layout: fixed;
}

.grass-calendar th {
  font-family: 'VT323', monospace;
  font-size: 1.2rem;
  color: var(--text-secondary);
  padding-bottom: 10px;
  border-bottom: 1px dashed #ccc;
}

.grass-calendar td {
  height: 40px;
  text-align: center;
  vertical-align: middle;
  background: #f4f4f4;
  border: 1px solid #ddd;
  border-radius: 4px; /* 살짝 둥근 사각형 (Squircles) */
  font-size: 0.9em;
  transition: all 0.2s;
  position: relative;
}

/* 요일 색상 */
.grass-calendar td.sat { color: var(--sat-color); background: #eff6ff; }
.grass-calendar td.sun { color: var(--sun-color); background: #fef2f2; }

/* 잔디(포스트가 있는 날) 스타일 */
.grass-calendar td.grass {
  background: #000 !important; /* 배경을 검정으로 */
  border: 1px solid #000;
  transform: translateY(-2px); /* 살짝 떠오르는 효과 */
  box-shadow: 2px 2px 0px var(--main-green); /* 그림자를 형광색으로 */
}

.grass-calendar td.grass a {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  color: var(--main-green); /* 글자색 형광 */
  font-weight: bold;
  font-family: 'VT323', monospace;
  font-size: 1.3em;
}

/* 잔디 호버 효과 */
.grass-calendar td.grass:hover {
  background: var(--main-green) !important;
  box-shadow: 2px 2px 0px #000;
}
.grass-calendar td.grass:hover a {
  color: #000;
}

/* 빈 날짜 숫자 스타일 */
.day-number {
  color: #bbb;
  font-family: 'VT323', monospace;
  font-size: 1.1em;
}
</style>

<div class="til-container">
  <div class="grass-header">
    <div class="grass-title">Pixel Garden</div>
    <div class="grass-subtitle">System.out.println("Hello, Hee-won!");</div>
  </div>

{% assign posts = site.posts | where_exp: "post", "post.path contains '_posts/til/김희원'" %} 
{% assign grouped = posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in grouped %}
  <div class="year-divider">📂 {{ year.name }} Archive</div>
  
  {% assign months = year.items | group_by_exp: "post", "post.date | date: '%m'" %}
  {% for month in months %}
    {% assign month_num = month.name | plus: 0 %}
    <div class="month-badge">{{ month_num }}월</div>

    {% assign days = month.items | group_by_exp: "post", "post.date | date: '%d'" %}

    {% comment %} 날짜 계산 로직 (기존 유지) {% endcomment %}
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
