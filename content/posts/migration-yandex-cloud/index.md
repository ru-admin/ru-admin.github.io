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
<div style="text-align: center;">

![AWS](/icons/aws.svg)

<div>AWS</div></div>

---
<div style="text-align: center;">

![Yandex Cloud](/icons/yandex.svg)
<div>Yandex Cloud</div></div>

---
<div style="text-align: center;">

![Terraform](/icons/terraform-original.svg)
<div>Terraform</div></div>

---
<div style="text-align: center;">

![Prometheus](/icons/prometheus-original.svg)
<div>Prometheus</div></div>

---
<div style="text-align: center;">

![Grafana](/icons/grafana-original.svg)
<div>Grafana</div></div>

---
<div style="text-align: center;">

![GitLab CI](/icons/gitlab-original.svg)
<div>GitLab CI</div></div>
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
