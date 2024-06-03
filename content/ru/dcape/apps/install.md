---
title: Установка
description: Инсталляция приложения дикейп
draft: false
weight: 2
---

## Через CI/CD

* В сервисе управления исходниками сделать форк или зеркало этого проекта
* Активировать полученный репозиторий в CI/CD
* В сервисе управления исходниками выполнить тестовую доставку, она отработает с ошибкой, но в сервисе управления конфигурациями (в dcape это enfist) будет создан пример конфигурации.
* В сервисе управления конфигурациями отредактировать настройки приложения и удалить .sample в имени настроек
* В сервисе управления исходниками повторно выполнить тестовую доставку - сервис будет развернут и запущен
* After that just change source and do `git push` - app will be reinstalled and restarted on CI/CD host

## Через терминал

На целевом сервере после установки [dcape](https://github.com/dopos/dcape) выполнить команды

```bash
git clone https://github.com/dopos/dcape-app-enfist.git
cd dcape-app-enfist
make config-if
... <edit .env.sample>
make up
```
