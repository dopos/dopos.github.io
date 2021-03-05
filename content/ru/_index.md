---
title: "dopos"
description: "Docker-серверы"
date: 2020-01-11T14:09:21+09:00
draft: false
landing:
  height: 530
  title:
    - Docker-серверы
  text:
  -  Настройка и развёртывание docker-приложений на выделенном сервере с помощью dcape
  titleColor:
  textColor:
  spaceBetweenTitleText: 25
  buttons:
    - link: https://github.com/dopos/dcape
      text: DCAPE
      color: primary
    - link: https://github.com/topics/dcape2-compartible
      text: Настройки приложений
      color: primary
    - link: dcape
      text: Документация
      color: default
sections:
  - bgcolor: teal
    type: card
    description: "Технология использует docker-образы opensource приложений и не добавляет требований к ресурсам после их развёртывания на локальном, тестовом или боевом сервере, поддерживает собственные и внешние приложения. Ключевые приложения:"
    header: 
      title: Преимущества dopos
      hlcolor: DarkKhaki #"#8bc34a"
      color: landing-button-primary
      fontSize: 32
      width: 360
    cards:
      - subtitlePosition: center
        description: "Proxy для docker-приложений и управление TLS сертификатами"
        image: images/traefik.svg
        color: white
        button: 
          name: traefik
          link: https://traefik.io/
          size: large
          target: _blank
          color: 'white'
          bgcolor: '#283593'
      - subtitlePosition: center
        description: "Хостинг git-репозиториев и запуск обработчиков при их изменении"
        image: images/gitea.svg
        color: white
        button: 
          name: gitea
          link: https://gitea.io/
          size: large
          target: _blank
          color: 'white'
          bgcolor: '#283593'      
      - subtitlePosition: center
        description: "Выполнение кода для тестов и развёртывания при изменении в git-репозитории"
        image: images/drone.svg
        color: white
        button: 
          name: drone
          link: https://www.drone.io/
          size: large
          target: _blank
          color: 'white'
          bgcolor: '#283593'
      - subtitlePosition: center
        description: "БД для служебных данных и для приложений"
        image: images/postgresql.svg
        color: white
        button: 
          name: postgresql
          link: https://www.postgresql.org/
          size: large
          target: _blank
          color: 'white'
          bgcolor: '#283593'
      - subtitlePosition: center
        description: "Управление docker-контейнерами"
        image: images/dc.svg
        color: white
        button: 
          name: docker-compose
          link: https://docs.docker.com/compose/
          size: large
          target: _blank
          color: 'white'
          bgcolor: '#283593'
      - subtitlePosition: center
        description: "Создание docker-compose.yml и .env"
        image: images/gnu-make.svg
        color: white
        button: 
          name: make
          link: https://www.gnu.org/software/make/
          size: large
          target: _blank
          color: 'white'
          bgcolor: '#283593'
---
