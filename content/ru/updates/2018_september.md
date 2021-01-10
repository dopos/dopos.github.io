---
title: "Сентябрь 2018"
description: "Dcape v1.0.0-rc2"
date: 2017-08-11T00:10:37+09:00
draft: false
weight: -5
---

## v1.0.0-rc2

Date: 2018-09-23
Github: [dcape](https://github.com/dopos/dcape)
Release: [v1.0.0-rc2, Use apisite](https://github.com/dopos/dcape/releases/tag/v1.0.0-rc2)

### Изменено

* apps/enfist: вместо [pgrpc](https://github.com/pgrpc/pgrpc-sql-enfist) теперь используется [apisite](https://github.com/apisite/app-enfist)

### Установка обновления

```bash
git pull
mv .env .env.bak
make init
make enfist-apply
make up
```
