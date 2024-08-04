---
author: kuldeep
datetime: 2024-08-04
title: Sonarqube
slug: sonarqube
featured: false
draft: false
tags:
  - sonarqube
ogImage: ""
description: SonarQube is a self-managed, automatic code review tool that systematically helps you deliver Clean Code.
---

# Sonarqube
SonarQube is a self-managed, automatic code review tool that systematically helps you deliver Clean Code. As a core element of our Sonar solution, SonarQube integrates into your existing workflow and detects issues in your code to help you perform continuous code inspections of your projects. The product analyses 30+ different programming languages and integrates into your Continuous Integration (CI) pipeline of DevOps platforms to ensure that your code meets high-quality standards.

## Sonarqube using docker
```bash
sudo docker run -p 9000:9000 -d --rm --name sonar sonarqube
```

## Sonarqube with report plugin
Fall this plugin guide 
https://github.com/cnescatlab/sonar-cnes-report/releases

create an docker image and use that instead
