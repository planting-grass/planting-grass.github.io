---
layout: post
title: "김희원"
permalink: /til/김희원/
---

<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&family=Noto+Sans+KR:wght@400;500;700&display=swap" rel="stylesheet">

<style>
/* --- Modern Diary & Photo Style System --- */
:root {
  --bg-color: #FBFBFD; 
  --card-bg: rgba(255, 255, 255, 0.7);
  --text-primary: #1D1D1F;
  --text-secondary: #6E6E73;
  
  /* Modern Green Palette */
  --green-primary: #2D3436; /* 다이어리 느낌을 위해 메인을 차분하게 변경 */
  --green-accent: #34C759; 
  --green-gradient: linear-gradient(135deg, #32D74B 0%, #248A3D 100%);
  
  /* Accents */
  --sat-color: #007AFF; 
  --sun-color: #FF3B30; 
  
  /* Effects */
  --radius-photo: 24px;
  --shadow-diary: 0 10px 30px rgba(0, 0, 0, 0.03), 0 1px 8px rgba(0, 0, 0, 0.02);
  --glass-border: 1px solid rgba(255, 255, 255, 0.4);
}

/* 전체 컨테이너: 사진/속지 느낌 */
.til-container {
  font-family: 'Inter', 'Noto Sans KR', sans-serif;
  background: white;
  padding: 60px 40px;
  border-radius: var(--radius-photo);
  max-width: 850px;
  margin: 40px auto;
  color: var(--text-primary);
  box-shadow: var(--shadow-diary);
  border: var(--glass-border);
  position: relative;
}

/* 헤더 영역 */
.grass-header {
  text-align: left; /* 다이어리처럼 왼쪽 정렬 */
  margin-bottom: 60px;
  border-bottom: 2px solid #F5F5F7;
  padding-bottom: 20px;
}

.grass-title {
  font-size: 2.8rem;
  font-weight: 800;
  margin: 0;
  letter-spacing: -0.04em;
  color: #1D1D1F;
}

.grass-subtitle {
  font-size: 1rem;
  color: var(--text-secondary);
  font-weight: 400;
  margin-top: 10px;
  letter-spacing: 0.05em;
  text-transform: uppercase;
}

/* 연도 구분 */
.year-divider {
  font-size: 2rem;
  font-weight: 800;
  color: #E5E5E5; /* 배경처럼 연하게 */
  position: absolute;
  right: 40px;
  margin-top: -10px;
}

/* 월 뱃지 */
.month-badge {
  font-size: 1.2rem;
  font-weight: 700;
  color: var(--text-primary);
  margin: 40px 0 20px 5px;
  display: flex;
  align-items: baseline;
  gap: 5px;
}

.month-badge::after {
    content: "MONTHLY LOG";
    font-size: 0.6rem;
    color: var(--text-secondary);
    letter-spacing: 0.1em;
}

/* 달력 카드 (Glassmorphism 적용) */
.grass-calendar-wrapper {
  background: var(--card-bg);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border-radius: 20px;
  border: var(--glass-border);
  box-shadow: 0 4px 15px rgba(0,0,0,0.02);
  padding: 30px;
  margin-bottom: 50px;
}

/* 달력 테이블 */
.grass-calendar {
  width: 100%;
  border-collapse: separate;
  border-spacing: 8px;
  table-layout: fixed;
}

.grass-calendar th {
  font-size: 0.7rem;
  color: #B2B2B2;
  font-weight: 700;
  padding-bottom: 15px;
  letter-spacing: 0.1em;
}

.grass-calendar td {
  height: 55px;
  text-align: center;
  background: #F9F9FB;
  border-radius: 14px;
  font-size: 1rem;
  font-weight: 500;
  color: #86868B;
  transition: all 0.3s ease;
  border: 1px solid rgba(0,0,0,0.02);
}

/* 휴일 */
.grass-calendar td.sat { color: var(--sat-color); }
.grass-calendar td.sun { color: var(--sun-color); }

/* 포스트가 있는 날 (잔디) */
.grass-calendar td.grass {
  background: var(--green-gradient);
  color: white !important;
  font-weight: 700;
  box-shadow: 0 10px 20px rgba(52, 199, 89, 0.2);
  border: none;
}

.grass-calendar td.grass a {
  display: block;
  width: 100%;
  height: 100%;
  line-height: 55px;
  text-decoration: none;
  color: inherit;
}

.grass-calendar td.grass:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 15px 30px rgba(52, 199, 89, 0.3);
}

.day-number { opacity: 0.5; }

/* 모바일 대응 */
@media (max-width: 600px) {
  .til-container { padding: 30px 20px; margin: 10px; }
  .grass-title { font-size: 2rem; }
  .grass-calendar td { height: 45px; border-radius: 10px; }
  .year-divider { display: none; }
}
</style>

<div class="til-container">
  <div class="grass-header">
    <h1 class="grass-title">Hee-won't TIL</h1>
    <div class="grass-subtitle">Memories of Learning</div>
  </div>

{% assign posts = site.posts | where_exp: "post", "post.path contains '_posts/til/김희원'" %} 
{% assign grouped = posts | group_by_exp: "post", "post.date | date: '%Y'" %}

{% for year in grouped %}
  <div class="year-divider">{{ year.name }}</div>
  
  {% assign months = year.items | group_by_exp: "post", "post.date | date: '%m'" %}
  {% for month in months %}
    {% assign month_num = month.name | plus: 0 %}
    
    <div class="month-badge">{{ month_num }}</div>

    {% assign days = month.items | group_by_exp: "post", "post.date | date: '%d'" %}
    {% assign first_post = month.items | first %}
    {% assign year_num = first_post.date | date: "%Y" | plus: 0 %}
    {% assign month_num_str = month.name %}
    {% assign first_day_str = year_num | append: '-' | append: month_num_str | append: '-01' %}
    {% assign first_day = first_day_str | date: "%w" | plus: 0 %}

    {% if month_num == 1 or month_num == 3 or month_num == 5 or month_num == 7 or month_num == 8 or month_num == 10 or month_num == 12 %}
      {% assign last_day = 31 %}
    {% elsif month_num == 2 %}
      {% if year_num | modulo: 400 == 0 or year_num | modulo: 100 != 0 and year_num | modulo: 4 == 0 %}
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
            <th>SUN</th><th>MON</th><th>TUE</th><th>WED</th><th>THU</th><th>FRI</th><th>SAT</th>
          </tr>
        </thead>
        <tbody>
          {% assign day = 1 %}
          {% assign total_cells = last_day | plus: first_day %}
          {% assign total_rows = total_cells | divided_by: 7 %}
          {% if total_cells | modulo: 7 != 0 %}{% assign total_rows = total_rows | plus: 1 %}{% endif %}

          {% for row in (1..total_rows) %}
            <tr>
              {% for col in (0..6) %}
                {% if row == 1 and col < first_day %}
                  <td style="background: transparent; border: none;"></td>
                {% elsif day > last_day %}
                  <td style="background: transparent; border: none;"></td>
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
                  {% if col == 0 %}{% assign cell_class = "sun" %}
                  {% elsif col == 6 %}{% assign cell_class = "sat" %}
                  {% endif %}

                  {% if day_group %}
                    <td class="{{ cell_class }} grass">
                      <a href="{{ day_group.items[0].url }}">{{ day }}</a>
                    </td>
                  {% else %}
                    <td class="{{ cell_class }}">
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
