---
title: "Self-Hosted проект-менеджмент Plane.so"
description: "Развёртывание open-source Plane.so CE в Coolify с кастомным Docker Compose, интеграцией Traefik и автоматическими бэкапами в AWS S3"
hero: "hero.webp"
tags: ["plane", "coolify", "docker", "traefik", "postgresql", "minio", "aws-s3"]
menu:
  sidebar:
    name: "Plane.so проект-менеджмент"
    identifier: plane-so
    parent: self-hosted
    weight: 40
categories:
- Self-Hosted
---

## Self-Hosted проект-менеджмент Plane.so

---

#### Клиент
Компания с потребностью в собственном инструменте управления проектами, размещённом на офисной инфраструктуре

---

#### Задача
Клиент хотел получить open-source альтернативу Jira/Linear для управления проектами и задачами, развёрнутую на собственном сервере в существующем окружении Coolify. Требовалось: установить последнюю версию Plane.so CE, обеспечить корректную работу за Traefik reverse proxy, вынести базу данных как отдельный сервис Coolify для удобного архивирования, а также настроить регулярные бэкапы всех данных.

---

#### Решение

###### 1. Подготовка инфраструктуры
- Анализ существующего окружения **Coolify** и конфигурации **Traefik** на сервере клиента
- Выбор последней open-source версии **Plane.so CE** из [официального репозитория](https://github.com/makeplane/plane)
- Изучение [документации по self-hosting](https://developers.plane.so/self-hosting/methods/docker-compose) для формирования базовой конфигурации

###### 2. Кастомный Docker Compose
- На основе официального docker-compose создан **уникальный Docker Compose**, адаптированный под Coolify
- Сервисы Plane (web, space, api, worker, beat-worker) настроены для работы за **Traefik** — корректные labels, сети и маршрутизация
- **PostgreSQL** вынесен как отдельный сервис Coolify — это позволяет управлять базой данных независимо и использовать встроенный механизм архивирования Coolify
- **Redis** для кэширования и очередей задач
- **MinIO** как S3-совместимое объектное хранилище для вложений и ассетов

###### 3. Интеграция с Traefik
- Настроены **Traefik labels** для автоматической маршрутизации трафика к сервисам Plane
- Автоматическое получение и обновление **SSL/TLS** сертификатов через Traefik
- Корректная проброска заголовков (X-Forwarded-For, Host) для правильной работы приложения за reverse proxy

###### 4. Резервное копирование
- **PostgreSQL** — регулярные бэкапы средствами Coolify с выгрузкой на **AWS S3**
- **MinIO** — регулярная синхронизация объектного хранилища на отдельный бакет **AWS S3**
- Все бэкапы выполняются автоматически по расписанию без вмешательства

---

#### Технологии
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/aws.svg" alt="AWS S3"><div>AWS S3</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
</div>

---

#### Результаты
✅ **Проект-менеджмент:** мощный self-hosted инструмент управления проектами, не уступающий коммерческим аналогам  
✅ **Интеграция:** Plane надёжно интегрирован в Coolify и работает за Traefik без конфликтов  
✅ **Независимость БД:** PostgreSQL как отдельный сервис Coolify — удобное управление и архивирование  
✅ **Бэкапы:** все данные (БД + MinIO) регулярно копируются на AWS S3 автоматически  
✅ **Контроль данных:** все данные проектов хранятся на собственном сервере клиента  

---

#### Архитектура
{{< mermaid align="center" >}}
graph TB
    A[Пользователи] --> B[Traefik :443]
    B --> C[Plane Web]
    B --> D[Plane Space]
    B --> E[Plane API]
    E --> F[PostgreSQL — отдельный сервис Coolify]
    E --> G[Redis]
    E --> H[MinIO]
    E --> I[Worker / Beat-Worker]
    F --> J[AWS S3 — бэкап БД]
    H --> K[AWS S3 — бэкап MinIO]
{{< /mermaid >}}

---

#### Длительность
1 день (развёртывание + кастомизация Docker Compose + настройка бэкапов + тестирование)

---

#### Стоимость
от 18 000 ₽
