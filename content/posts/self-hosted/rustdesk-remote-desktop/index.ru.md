---
title: "Self-Hosted удалённый доступ с RustDesk"
description: "Развёртывание RustDesk OSS сервера для компании поддержки 1С для организации безопасного удалённого доступа к рабочим станциям клиентов"
hero: "hero.webp"
tags: ["rustdesk", "remote-desktop", "self-hosted", "docker", "linux", "1c"]
menu:
  sidebar:
    name: "RustDesk удалённый доступ"
    identifier: rustdesk-remote-desktop
    parent: self-hosted
    weight: 34
categories:
- Self-Hosted
---

## Self-Hosted удалённый доступ с RustDesk

---

#### Клиент
Компания, оказывающая поддержку и сопровождение платформы 1С

---

#### Задача
Клиенту требовалось безопасное self-hosted решение для удалённого доступа, чтобы инженеры поддержки могли подключаться к рабочим станциям и серверам клиентов. Необходимы были: полный контроль над данными, отсутствие зависимости от сторонних облачных сервисов, надёжная работа в корпоративных сетях.

---

#### Решение
- Развёрнут **RustDesk OSS** (open-source) сервер через Docker Compose
- Настроены компоненты **hbbs** (ID/relay-сервер) и **hbbr** (relay-сервер)
- Настроены правила файрвола и сетевой доступ для безопасных подключений
- Подготовлены конфигурационные файлы клиента для быстрого развёртывания у команды поддержки
- Описана процедура обслуживания и стратегия бэкапов

---

#### Технологии
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/rustdesk.svg" alt="RustDesk"><div>RustDesk</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
</div>

---

#### Результаты
✅ **Полный суверенитет данных** — все метаданные соединений и релейный трафик остаются на инфраструктуре клиента  
✅ **Нет лицензионных затрат** — open-source RustDesk исключает плату за рабочие места  
✅ **Надёжное подключение** — инженеры получают доступ к машинам клиентов из любой точки  
✅ **Простой roll-out клиента** — готовые конфиги для быстрого онбординга команды  
✅ **Дружелюбно к корпоративным сетям** — работает через NAT/файрволы благодаря relay-серверам  

---

#### Архитектура
{{< mermaid align="center" >}}
graph TB
    A[Инженеры поддержки] -->|RustDesk Client| B[Traefik :443]
    B --> C[hbbs — ID/Relay сервер]
    B --> D[hbbr — Relay сервер]
    C --> E[(PostgreSQL<br/>Метаданные)]
    D --> F[Рабочие станции<br/>& Серверы клиентов]
{{< /mermaid >}}

---

#### Длительность
12 часов (подготовка сервера, развёртывание RustDesk, SSL, тестирование, документация)

---

#### Стоимость
36 000 ₽ / $430