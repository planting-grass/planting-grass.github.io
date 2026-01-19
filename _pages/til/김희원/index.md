---
layout: post
title: "김희원"
permalink: /til/김희원/
---

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">

<style>
/* --- Modern iOS Style System --- */
:root {
  --bg-color: #F5F5F7; /* iOS 베이스 배경색 */
  --card-bg: #FFFFFF;
  --text-primary: #1D1D1F;
  --text-secondary: #86868B;
  
  /* Green Palette */
  --green-primary: #34C759; /* Apple Green */
  --green-gradient-start: #4ADE80;
  --green-gradient-end: #16A34A;
  
  /* Accents */
  --sat-color: #5856D6; /* Soft Blue/Purple */
  --sun-color: #FF3B30; /* Soft Red */
  
  /* Shapes & Shadows */
  --radius-l: 20px;
  --radius-m: 12px;
  --radius-s: 8px;
  --shadow-soft: 0 4px 20px rgba(0, 0, 0, 0.05);
  --shadow-hover: 0 8px 25px rgba(52, 199, 89, 0.25);
}

/* 전체 컨테이너 */
.til-container {
  font-family: 'Inter', 'Noto Sans KR', sans-serif;
  background-color: var(--bg-color);
  padding: 40px 20px;
  border-radius: var(--radius-l);
  max-width: 900px;
  margin: 0 auto;
  color: var(--text-primary);
}

/* 헤더 영역 */
.grass-header {
  text-align: center;
  margin-bottom: 50px;
}

.grass-title {
  font-size: 2.5rem;
  font-weight: 800;
  margin: 0;
  letter-spacing: -0.02em;
  background: linear-gradient(135deg, #111, #555);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.grass-subtitle {
  font-size: 0.95rem;
  color: var(--text-secondary);
  font-weight: 500;
  margin-top: 8px;
  display: inline-block;
  background: #E5E5EA;
  padding: 6px 16px;
  border-radius: 20px;
}

/* 연도 구분선 (Sticky Header 느낌) */
.year-divider {
  font-size: 1.8rem;
  font-weight: 700;
  color: var(--text-primary);
  margin: 60px 0 20px 0;
  padding-left: 10px;
  border-left: 5px solid var(--green-primary);
  line-height: 1;
}

/* 월 뱃지 */
.month-badge {
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--green-primary);
  margin: 30px 0 15px 4px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.month-badge::after {
  content: '';
  flex: 1;
  height: 1px;
  background: #E5E5EA;
}

/* 달력 카드 래퍼 */
.grass-calendar-wrapper {
  background: var(--card-bg);
  border-radius: var(--radius-l);
  box-shadow: var(--shadow-soft);
  padding: 24px;
  margin-bottom: 40px;
  transition: transform 0.2s ease;
}

.grass-calendar-wrapper:hover {
  transform: translateY(-2px);
}

/* 달력 테이블 */
.grass-calendar {
  width: 100%;
  border-collapse: separate; /* 셀 간격 분리 */
  border-spacing: 6px; /* 셀 사이 간격 */
  table-layout: fixed;
}

.grass-calendar th {
  font-size: 0.8rem;
  color: var(--text-secondary);
  font-weight: 600;
  padding-bottom: 12px;
  text-transform: uppercase;
}

.grass-calendar td {
  height: 50px; /* 정사각형 비율 유지 */
  text-align: center;
  vertical-align: middle;
  background: #F2F2F7;
  border-radius: var(--radius-m);
  font-size: 0.95rem;
  font-weight: 500;
  color: var(--text-secondary);
  transition: all 0.2s cubic-bezier(0.25, 0.8, 0.25, 1);
  position: relative;
  border: none;
}

/* 주말 텍스트 색상 */
.grass-calendar td.sat { color: var(--sat-color); background: #F0F0FF; }
.grass-calendar td.sun { color: var(--sun-color); background: #FFF0F0; }

/* 잔디(포스트 있는 날) 스타일 - 핵심 */
.grass-calendar td.grass {
  background: linear-gradient(135deg, var(--green-gradient-start), var(--green-gradient-end));
  box-shadow: 0 4px 12px rgba(52, 199, 89, 0.3);
  color: white !important;
  font-weight: 700;
  cursor: pointer;
}

/* 링크 스타일 */
.grass-calendar td.grass a {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
  justify-content: center;
  text-decoration: none;
  color: white;
  border-radius: var(--radius-m);
}

/* 잔디 호버 효과 */
.grass-calendar td.grass:hover {
  transform: scale(1.08);
  box-shadow: var(--shadow-hover);
  z-index: 10;
}

/* 날짜 숫자 */
.day-number {
  opacity: 0.6;
}

/* 반응형 모바일 대응 */
@media (max-width: 600px) {
  .grass-calendar td {
    height: 40px;
    font-size: 0.8rem;
    border-radius: var(--radius-s);
  }
  .grass-calendar-wrapper {
    padding: 15px;
  }
  .grass-title {
    font-size: 2rem;
  }
}
</style>

<div class="til-container">
  <div class="grass-header">
    <h1 class="grass-title">Hee-won's TIL</h1>
    <div class="grass-subtitle">Daily Learning Log</div>
  </div>

{% assign posts = site.posts | where_exp: "post", "post.path contains '_posts/til/김희원'" %} 
{% assign grouped = posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in grouped %}
  <div class="year-divider">{{ year.name }}</div>
  
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
            <th style="color: var(--sun-color);">SUN</th>
            <th>MON</th>
            <th>TUE</th>
            <th>WED</th>
            <th>THU</th>
            <th>FRI</th>
            <th style="color: var(--sat-color);">SAT</th>
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
                  <td style="background: transparent;"></td>
                {% elsif day > last_day %}
                  <td style="background: transparent;"></td>
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
                      <a href="{{ day_group.items[0].url }}" title="{{ day_group.items[0].title }}">{{ day }}</a>
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
