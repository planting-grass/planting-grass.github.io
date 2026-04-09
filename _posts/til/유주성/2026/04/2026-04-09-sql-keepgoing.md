---
title: SQL
author: 유주성
date: 2026-04-09
category: TIL/유주성/2026/04
layout: post
---
https://school.programmers.co.kr/learn/courses/30/lessons/157340

못 풀겠는 문제는 끝도 없이 나온다. 이거 무슨 CASE를 이중으로 사용하는 …

```sql
-- 코드를 입력하세요
SELECT CAR_ID,
    CASE
    WHEN SUM(
        CASE
        WHEN '2022-10-16' BETWEEN START_DATE AND END_DATE
        THEN 1
        ELSE 0
        END
    ) > 0 THEN "대여중"
    ELSE "대여 가능"
    END AS AVAILABILITY
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
GROUP BY CAR_ID
ORDER BY CAR_ID DESC
```