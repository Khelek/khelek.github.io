---
layout: post
title:  "Команда tcpdump, которая показывает http запросы в читаемом виде"
categories: linux
published: true
---

С непривычки разобраться в разнообразных аргументах tcpdump тяжело, так что сохранил команду, которая показывает в читабельном виде содержимое HTTP:
```
$ sudo tcpdump -i lo tcp port 8081 -vv -A
```
