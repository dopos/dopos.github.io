---
title: "Usage"
description: "test post"
date: 2020-01-28T00:39:06+09:00
weight: 1
draft: false
#collapsible: true
---

*Markdown here*
### Обновление файла .env

При обновлении проекта возможно появление новых переменных в `.env` файле.
Алгоритм обновления .env с сохранением старых настроек:

```bash
mv .env .env.bak
make init
```

Другой вариант:

```bash
mv .env .env.1019
make init CFG_BAK=.env.1019
```

Все совпадающие значения будут взяты из `.env.bak` (т.е. из старого конфига).
Если изменятся номера версий используемых образов docker, будут выведены предупреждения.

Для того, чтобы обновить номера версий образов docker, сохранив остальные настройки, надо подготовить `.env.bak`, убрав из него номера версий:

```bash
grep -v "_VER=" .env > .env.bak
mv .env .env.all
make init
```

