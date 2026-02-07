---
title: "Миграция AWS → Yandex Cloud"
description: "Перенос инфраструктуры компании из AWS в Yandex Cloud в рамках импортозамещения"
hero: "hero.webp"
tags: ["aws", "yandex"]
menu:
  sidebar:
    name: "Миграция в Yandex"
    identifier: migration-yandex-cloud
    weight: 20
---

## Перенос инфраструктуры компании из AWS в Yandex Cloud в рамках импортозамещения

---

#### Клиент
Конфиденциально

---

#### Задача
Компания столкнулась с необходимостью срочной миграции из AWS в российское облако из-за санкционных рисков. Требовалось перенести всю инфраструктуру с минимальным простоем и без потери функциональности.

---

#### Решение

###### 1. Аудит и планирование

- Инвентаризация всех ресурсов AWS (EC2, RDS, S3, VPC)
- Маппинг сервисов AWS → Yandex Cloud
- Разработка поэтапного плана миграции
- Подготовка rollback стратегии

###### 2. Подготовка инфраструктуры
- Terraform для IaC в Yandex Cloud
- Настройка VPC, подсетей, security groups
- Развертывание Managed PostgreSQL и Redis
- Настройка Object Storage (аналог S3)

###### 3. Миграция данных
- Репликация баз данных через DMS
- Синхронизация S3 → Object Storage
- Перенос Docker образов в Container Registry
- Тестирование на staging окружении

###### 4. Переключение production
- DNS failover для постепенного переключения
- Мониторинг метрик в реальном времени
- Откат на AWS в случае критических проблем
- Финальное переключение за 2 часа

---

#### Технологии
{{< split 2 2 2 2 2 2 >}}
![AWS](/icons/aws.svg) AWS
---
![Yandex Cloud](/icons/yandex.svg) Yandex Cloud
---
![Terraform](/icons/terraform-original.svg) Terraform
---
![Prometheus](/icons/prometheus-original.svg) Prometheus
---
![Grafana](/icons/grafana-original.svg) Grafana
---
![GitLab CI](/icons/gitlab-original.svg) GitLab CI
{{< /split >}}

---

#### Результаты
✅ **Затраты:** снижение на 40% (с $15k до $9k/месяц)  
✅ **Downtime:** всего 2 часа вместо планируемых 8  
✅ **Независимость:** полный переход на российскую инфраструктуру  
✅ **Производительность:** сохранена на том же уровне  
✅ **Безопасность:** соответствие 152-ФЗ  

---

#### Архитектура
```
AWS (EC2, RDS, S3) → Terraform → Yandex Cloud (Compute, Managed PostgreSQL, Object Storage)
```

---

#### Длительность
2 недели (планирование + миграция)

---

#### Стоимость
от 150 000 ₽
