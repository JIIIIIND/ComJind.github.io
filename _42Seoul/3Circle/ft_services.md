---
title: "ft_services"
excerpt: "42Seoul ft_services Guide documents"
date: 2020-09-25 13:51:11 +0900
categories: 42Seoul
---

Kubernetes를 사용하여 wordpress를 동작하게 만드는 과제

각각의 서비스는 독립적인 docker 컨테이너 위에서 동작함

컨테이너는 alpine 리눅스를 사용

setup.sh를 통해 설치가 진행 될 수 있게 스크립트 작성

## 서비스 목록

- nginx
- mysql
- phpmyadmin
- wordpress
- ftps
- influxdb
- grafana