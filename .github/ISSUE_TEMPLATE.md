---
title: New release {{ env.VERSION }}
assignees: RoundedToken
labels: RELEASE
---

Автор: {{ env.NAME }}
Дата: {{ date | date('dddd, MMMM Do') }}

CHANGELOG {{  env.BODY  }}
