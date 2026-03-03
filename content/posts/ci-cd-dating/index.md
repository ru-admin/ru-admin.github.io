---
title: "CI/CD и инфраструктура для Dating-сервиса"
description: "Полный DevOps-стек для продакшен запуска: Docker, GitLab CI/CD, мониторинг, бэкапы"
hero: "hero.webp"
tags: ["gitlab", "docker", "ansible", "prometheus", "nginx", "postgresql"]
menu:
  sidebar:
    name: "CI/CD Dating-сервис"
    identifier: cicd-dating-service
    weight: 60
---

## Инфраструктура и CI/CD для продакшен запуска Dating-сервиса

---

#### Клиент
Dating-сервис Puzzle Master

---

#### Задача
Стартап разработал бэкенд на Nest.js + фронтенд на Angular и был готов к запуску, но не имел никакой инфраструктуры: деплой был ручным, не было CI/CD, мониторинга, бэкапов и разделения dev/prod окружений. Требовалось выстроить полный DevOps-стек с нуля под продакшен.

---

#### Решение

###### 1. Контейнеризация приложения
- Multi-stage Dockerfile для backend (Nest.js + Prisma, non-root пользователь)
- Multi-stage Dockerfile для frontend (Angular 12, legacy OpenSSL, Nginx для статики)
- Docker Compose с полным стеком: PostgreSQL 15, Redis 7, imgproxy, Nginx
- Healthchecks и depends_on для правильного порядка запуска
- Раздельные окружения dev и prod в /opt/dev и /opt/prod

###### 2. GitLab CI/CD
- Миграция репозиториев с Bitbucket на GitLab
- Пайплайны для backend и frontend: build → push → deploy
- GitLab Container Registry для хранения Docker образов
- Автоматический деплой в dev, ручной trigger для prod
- SSH деплой на VPS через SSH_PRIVATE_KEY

###### 3. Nginx Reverse Proxy
- Универсальный конфиг через envsubst для dev/prod
- SSL/TLS (TLSv1.2, TLSv1.3) с сертификатами Cloudflare
- Проксирование /api/* → backend:4000, /* → frontend:80
- Редирект www → основной домен (301)
- Отдельный стек imgproxy с SSL терминацией на img.pm.life

###### 4. Безопасность (Ansible)
- Настройка сервера через Ansible: SSH только по ключам, отключён root
- UFW Firewall: открыты только порты 80, 443, кастомный SSH
- Доступ к БД только через SSH Tunnel (Beekeeper Studio)
- Секреты в переменных GitLab CI/CD

###### 5. Мониторинг
- Prometheus + Grafana с автоматическим провижинингом дашбордов
- Exporters: Node, cAdvisor, Postgres, Redis, Nginx, Blackbox
- 5 Grafana дашбордов: сервер, Docker контейнеры, PostgreSQL, Redis, Nginx
- Alertmanager с интеграцией в Telegram, алерты на CPU/RAM/Disk/API/SSL

###### 6. Бэкапы БД
- Автоматический pg_dump каждый час
- Сжатие gzip и загрузка в Cloudflare R2 (S3-compatible)
- Prometheus метрики бэкапов: успех, размер, timestamp
- Алерты: DatabaseBackupMissing, DatabaseBackupFailed, DatabaseBackupSizeAnomaly

---

#### Технологии
{{< split 2 2 2 2 2 2 >}}
<div style="text-align: center;">

![GitLab](/icons/gitlab-original.svg)

<div>GitLab CI</div></div>

---
<div style="text-align: center;">

![Docker](/icons/docker-original.svg)

<div>Docker</div></div>

---
<div style="text-align: center;">

![Ansible](/icons/ansible-original.svg)

<div>Ansible</div></div>

---
<div style="text-align: center;">

![Prometheus](/icons/prometheus-original.svg)

<div>Prometheus</div></div>

---
<div style="text-align: center;">

![Nginx](/icons/nginx.svg)

<div>Nginx</div></div>

---
<div style="text-align: center;">

![PostgreSQL](/icons/postgresql.svg)

<div>PostgreSQL</div></div>

{{< /split >}}

---

#### Результаты
✅ **Деплой:** git push в main → автоматическая сборка и деплой на сервер  
✅ **Окружения:** полное разделение dev и prod на одном VPS  
✅ **Мониторинг:** 5 дашбордов, алерты в Telegram по 6 категориям  
✅ **Бэкапы:** автоматический pg_dump каждый час в Cloudflare R2  
✅ **Безопасность:** UFW, SSH по ключам, БД закрыта извне  
✅ **Масштабируемость:** архитектура готова к выносу БД на отдельный сервер  

---

#### Архитектура
{{< mermaid align="center" >}}
graph TB
    A[git push] --> B[GitLab CI/CD]
    B --> C[Container Registry]
    B --> D[VPS via SSH]
    D --> E[Nginx :443]
    E --> F[Backend Nest.js]
    E --> G[Frontend Angular]
    F --> H[PostgreSQL]
    F --> I[Redis]
    E --> J[imgproxy img.pm.life]
    J --> K[Cloudflare R2]
    L[Prometheus] --> F
    L --> H
    L --> I
    L --> E
    L --> M[Grafana]
    L --> N[Alertmanager]
    N --> O[Telegram]
{{< /mermaid >}}

---

#### Длительность
42 часа (Docker, CI/CD, мониторинг, алертинг, бэкапы, документация)

---

#### Стоимость
от 105 000 ₽
