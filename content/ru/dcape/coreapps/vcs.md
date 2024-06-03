---
title: vcs
description: git-совместимый сервис для работы с репозиториями
draft: false
weight: 3
---

> Repo: [dcape-app-gitea](https://github.com/dopos/dcape-app-gitea)

 Роль в dcape | Сервис | Docker images
 --- | --- | ---
 vcs | [gitea](https://about.gitea.com/) | [gitea](https://hub.docker.com/r/gitea/gitea)

## Назначение

Git совместимый сервис для работы с репозиториями (если используется несколько серверов, разворачивается только на одном)/

Gitea - это сервис управления git-репозиториями, который поддерживает

* интеграцию с сервисом развертывания приложений [woodpecker](https://github.com/dopos/dcape-app-woodpecker)
* интеграцию с [narra](https://github.com/dopos/dcape-app-narra) по протоколу OAuth2

