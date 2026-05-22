---
title: "Мониторинг Prometheus + Grafana"
description: "Внедрение полноценного observability стека для микросервисной архитектуры"
hero: "hero.webp"
tags: ["kubernetes", "docker", "prometheus", "grafana", "monitoring"]
menu:
  sidebar:
    name: "Мониторинг Prometheus"
    identifier: monitoring-prometheus-grafana
    parent: sre-observability
    weight: 40
categories:
- SRE
- Observability
---

## Observability стек для микросервисной архитектуры

---

#### Клиент
Начинающий стартап

---

#### Задача
Компания перешла на микросервисную архитектуру (15+ сервисов), но не имела централизованного мониторинга. Проблемы обнаруживались только по жалобам пользователей через 30+ минут. Требовалось внедрить полноценный observability стек для быстрого выявления и диагностики проблем.

---

#### Решение

###### 1. Архитектура мониторинга

- **Prometheus** для сбора метрик
- **Grafana** для визуализации
- **Loki** для централизованных логов
- **Jaeger** для distributed tracing
- **Alertmanager** для уведомлений

###### 2. Сбор метрик
- Автоматическое обнаружение сервисов в Kubernetes
- Метрики приложений (custom metrics)
- Системные метрики (node-exporter)
- Метрики БД (postgres-exporter, redis-exporter)

###### 3. Визуализация в Grafana
- Дашборды для каждого микросервиса
- Общий дашборд инфраструктуры
- SLA/SLO метрики
- Business метрики (RPS, конверсия)

###### 4. Централизованные логи (Loki)
- Агрегация логов всех сервисов
- Поиск по логам через Grafana
- Корреляция логов с метриками

###### 5. Distributed Tracing (Jaeger)
- Трейсинг HTTP запросов между сервисами
- Визуализация цепочек вызовов
- Поиск узких мест (bottlenecks)
- Анализ latency по сервисам

###### 6. Алертинг
- Алерты в Telegram
- Эскалация критичных проблем
- On-call ротация
- Автоматическое создание инцидентов

---

#### Технологии
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/prometheus-original.svg" alt="Prometheus"><div>Prometheus</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/grafana-original.svg" alt="Grafana"><div>Grafana</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/kubernetes-plain.svg" alt="Kubernetes"><div>Kubernetes</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/helm-original.svg" alt="Helm"><div>Helm</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
</div>

---

#### Результаты
✅ **MTTD:** обнаружение проблем с 30 минут до 1 минуты  
✅ **MTTR:** время восстановления сократилось на 60%  
✅ **Алерты:** автоматические уведомления в Telegram  
✅ **Visibility:** полная прозрачность работы всех сервисов  
✅ **Capacity planning:** данные для планирования ресурсов  

---

#### Архитектура
{{< mermaid align="center" >}}
graph LR
    A[Микросервисы] --> B[Prometheus]
    A --> C[Loki]
    A --> D[Jaeger]
    B --> E[Grafana]
    C --> E
    D --> E
    E --> F[Alertmanager]
    F --> G[Telegram]
{{< /mermaid >}}

---

#### Длительность
1 неделя (настройка + дашборды + алерты)

---

#### Стоимость
от 100 000 ₽