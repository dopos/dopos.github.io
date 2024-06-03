---
title: App template
description: Шаблон приложения для dcape v3
draft: false
weight: 1
---

> Repo: [dcape-app-template](https://github.com/dopos/dcape-app-template)

[upstream_name](https://upstream_url) application package for [dcape](https://github.com/dopos/dcape).

## Upstream

* Project: [upstream_name](https://upstream_url)
* Docker: [template](https://hub.docker.com/r/template)

## Requirements

* linux 64bit with git, make, sed installed
* [docker](http://docker.io) with [compose plugin](https://docs.docker.com/compose/install/linux/)
* [dcape](https://github.com/dopos/dcape) v3
* VCS service like [Gitea](https://gitea.io)
* CI/CD service like [Woodpecker CI](https://woodpecker-ci.org/)

## Install

### Via CI/CD

* VCS: Fork or mirror this repo in your Git service
* CI/CD: Activate repo
* VCS: "Test delivery", config sample will be saved to config service (enfist in dcape)
* Config: Edit config vars and remove .sample from config name
* VCS: "Test delivery" again (or CI/CD: "Restart") - app will be installed and started on CI/CD host
* After that just change source and do `git push` - app will be reinstalled and restarted on CI/CD host

### Via terminal

Run commands on deploy host with [dcape](https://github.com/dopos/dcape) installed:
```bash
git clone https://github.com/dopos/dcape-app-template.git
cd dcape-app-template
make config-if
... <edit .env>
make up
```

