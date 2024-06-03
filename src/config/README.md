# enfist
Enfist application package for dcape

**Enfist** - это сервис хранения конфигураций приложений. Конфигурации хранятся в БД в виде Key-value таблицы, где ключ (key) формируется из адреса git репозитория `organization--repo_name--branch` (`организация--проект--ветка`), а значение (value) - содержимое `.env` файла.

Доступ к хранилищу ограничивается [narra](/dcape/coreapps/narra) и осуществляется через фронтенд **dcape**.

Кроме веб-интерфейса, работа с конфигурациями запуска может осуществляться посредством [dcape-config-cli](https://github.com/dopos/dcape-config-cli).
Примеры команд, доступных после клонирования (git clone) и настройки (make .env) dcape-config-cli:

* `make get TAG=name` - получить из хранилища конфигурацию для ключа (тега) `name` и сохранить в файл `name.env`
* `make set TAG=name` - загрузить файл `name.env` в хранилище с ключом (тегом) `name`

Тег содержит значение, равное ключу БД Key-value хранилища: `organization--name_of_repo--branch` (`организация--проект--ветка`)

В файле конфигурации dcape-config-cli задается два параметра:

* `ENFIST_URL` - адрес сервиса enfist
* `CIS_TOKEN` - токен для авторизации в gitea
