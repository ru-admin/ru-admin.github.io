---
title: "Корпоративный мессенджер Matrix"
description: "Развертывание собственного защищенного мессенджера для компании"
hero: "hero.webp"
tags: ["matrix", "docker", "caddy", "postgresql", "element"]
menu:
  sidebar:
    name: "Matrix мессенджер"
    identifier: matrix-messenger
    weight: 50
---

## Альтернатива Telegram для корпоративных коммуникаций

---

#### Клиент
Средний бизнес с требованиями к безопасности данных

---

#### Задача
Компания нуждалась в собственном защищенном мессенджере из-за требований безопасности и необходимости полного контроля над корпоративными коммуникациями. Требовалось решение с шифрованием, видеозвонками и интеграцией с корпоративной инфраструктурой.

---

#### Решение

###### 1. Серверная часть
- **Matrix Synapse** как основной сервер
- **PostgreSQL 16** для хранения данных
- **Caddy** как reverse proxy с автоматическим SSL
- **Docker Compose** для оркестрации всех сервисов

###### 2. Клиентские приложения
- **Element Web** для браузера
- **Element Desktop** для Windows/macOS/Linux
- **Element Mobile** для iOS/Android
- Единый интерфейс на всех платформах

###### 3. Видеозвонки
- **Coturn** (TURN/STUN сервер) для NAT traversal
- Поддержка групповых видеозвонков
- UDP порты 49160-49200 для медиа-трафика
- Автоматическая конфигурация через переменные окружения

###### 4. Администрирование
- **Synapse Admin** - веб-интерфейс управления
- Управление пользователями и комнатами
- Статистика и мониторинг
- Доступ через отдельный порт 8888

###### 5. Безопасность
- End-to-end шифрование сообщений
- Автоматические SSL/TLS сертификаты через Caddy
- Отключена публичная регистрация
- Федерация с другими Matrix серверами
- Healthcheck для всех сервисов

###### 6. Автоматизация
- Bash скрипт для полной инициализации
- Автоматическая генерация конфигурации Synapse
- Автоматическое создание admin пользователя через expect
- Docker Compose с зависимостями и healthchecks

---

#### Технологии
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/caddy.svg" alt="Caddy"><div>Caddy</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/element.svg" alt="Element"><div>Element + Matrix + Synapse</div></div>
</div>

---

#### Результаты
✅ **Независимость:** полный контроль над данными и коммуникациями  
✅ **Масштаб:** 100+ пользователей одновременно  
✅ **Функциональность:** текст, голос, видео, файлы до 1.5GB, шифрование  
✅ **Скорость:** развертывание за 5 минут одним скриптом  
✅ **Надежность:** автоматические SSL сертификаты, healthchecks, auto-restart  

---

#### Архитектура
{{< mermaid align="center" >}}
graph TB
    A[Пользователи] --> B[Caddy :443]
    B --> C[Matrix Synapse :8008]
    B --> D[Element Web :80]
    B --> E[Synapse Admin :8888]
    C --> F[PostgreSQL :5432]
    A --> G[Coturn :3478/5349]
    G --> A
{{< /mermaid >}}

---

#### Длительность
1 день (установка + настройка + тестирование)

---

#### Стоимость
от 15 000 ₽