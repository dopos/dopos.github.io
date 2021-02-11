---
title: "dopos"
description: "Docker powered servers"
date: 2020-01-11T14:09:21+09:00
draft: false
sections:
  - bgcolor: \#7db88e
    type: normal
    header:
      title: Docker powered servers
      hlcolor: DarkKhaki
      fontSize: 32
      width: 415
    body:
      subtitle: Настройка и развёртывание docker-приложений
      subtitlePosition: left
      description: 
        Проект **dopos** (**do**cker **po**wered **s**ervers) - это еще один вариант решения задачи *"На дешевом виртуальном сервере с небольшим оверхедом установить окружение для развёртывания [docker](https://www.docker.com/)-приложений"*. Решение основано на [docker-compose](https://docs.docker.com/compose/) и состоит из <a href="/dcape">dcape</a>, инструмента создания среды для развёртывания, и адаптированных для этой среды <a href="https://github.com/topics/dcape2-compartible">настроек приложений</a>.
        <br><br>
        Замечательный [docker](https://www.docker.com/) и потрясающие приложения с открытым исходным кодом, размещенные на [github](https://github.com/), дают возможность очень легко получить CI/CD системы развёртывания приложений. В простейшем случае, достаточно скачать файл `docker-compose.yml` и выполнить команду `docker-compose up`. Но для того, чтобы удаленно разворачивать свои или сторонние приложения командой `git push`, необходимо что-то еще. Например - <a href="/dcape">dcape</a>.
        <br><br>
        <center><a href="/dcape">dcape</a> | <a href="https://github.com/topics/dcape2-compartible">приложения</a></center>
---
