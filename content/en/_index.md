---
title: "dopos"
description: "Docker powered servers"
date: 2020-01-11T14:09:21+09:00
draft: false
landing:
  height: 530
  title:
    - Docker powered servers
  text:
  -  Setup and deploy for docker applications on a dedicated server using dcape
  titleColor:
  textColor:
  spaceBetweenTitleText: 25
  buttons:
    - link: https://github.com/dopos/dcape
      text: DCAPE
      color: primary
    - link: https://github.com/topics/dcape2-compartible
      text: applications
      color: primary
    - link: dcape
      text: Docs
      color: default
sections:
  - bgcolor: teal
    type: card
    description: "The dopos technology uses docker images of opensource applications and does not add resource requirements after they are deployed on a local, test, or production server. Key Applications:"
    header: 
      title: Why dopos
      hlcolor: DarkKhaki #"#8bc34a"
      color: landing-button-primary
      fontSize: 32
      width: 200
    cards:
      - subtitlePosition: center
        description: "Docker-applications proxy and TLS-certificate management"
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
        description: "code hosting solution"
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
        description: "Container-Native, Continuous Delivery Platform"
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
        description: "tool for running multi-container applications on Docker"
        image: https://github.com/docker/compose/blob/master/logo.png?raw=true
        color: white
        button: 
          name: docker-compose
          link: https://docs.docker.com/compose/
          size: large
          target: _blank
          color: 'white'
          bgcolor: '#283593'
      - subtitlePosition: center
        description: "build docker-compose.yml and .env files"
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
