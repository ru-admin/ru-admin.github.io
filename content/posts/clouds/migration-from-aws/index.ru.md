---
title: "Миграция AWS → Yandex Cloud"
description: "Перенос инфраструктуры компании из AWS в Yandex Cloud в рамках импортозамещения"
hero: "hero.webp"
tags: ["aws", "yandex"]
menu:
  sidebar:
    name: "Миграция в Yandex"
    identifier: migration-from-aws
    parent: clouds
    weight: 10
categories:
- Cloud Migration
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
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/aws.svg" alt="AWS"><div>AWS</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/terraform-original.svg" alt="Terraform"><div>Terraform</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/prometheus-original.svg" alt="Prometheus"><div>Prometheus</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/grafana-original.svg" alt="Grafana"><div>Grafana</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/gitlab-original.svg" alt="GitLab CI"><div>GitLab CI</div></div>
</div>

---

#### Результаты
✅ **Затраты:** снижение на 40% (с $15k до $9k/месяц)  
✅ **Downtime:** всего 2 часа вместо планируемых 8  
✅ **Независимость:** полный переход на российскую инфраструктуру  
✅ **Производительность:** сохранена на том же уровне  
✅ **Безопасность:** соответствие 152-ФЗ  

---

#### Архитектура
{{< mermaid align="center" >}}
graph LR
    A[AWS: EC2, RDS, S3] --> B[Terraform]
    B --> C[Yandex Cloud: Compute, PostgreSQL, Object Storage]
{{< /mermaid >}}

---

#### Длительность
2 недели (планирование + миграция)

---

#### Стоимость
от 150 000 ₽
