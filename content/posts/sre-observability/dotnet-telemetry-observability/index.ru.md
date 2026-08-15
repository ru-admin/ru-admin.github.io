---
title: "Observability и Telemetry для .NET Backend"
description: "Внедрение OpenTelemetry для распределённого трассинга, метрик и оптимизации производительности .NET 8 бэкенда"
hero: "hero.webp"
tags: ["kubernetes", "docker", "opentelemetry", "dotnet", "observability", "jaeger", "prometheus"]
menu:
  sidebar:
    name: ".NET Telemetry & Observability"
    identifier: dotnet-telemetry-observability
    parent: sre-observability
    weight: 51
categories:
- SRE
- Observability
---

## Observability для .NET Backend: интеграция телеметрии и оптимизация производительности

---

#### Клиент
E-commerce платформа с высоконагруженным .NET 8 бэкендом

---

#### Задача
У .NET бэкенда не было наблюдаемости — никакого распределённого трассинга, централизованных метрик, структурированного логирования. Поиск медленных запросов требовал ручного разбора логов. Команда нуждалась в полном внедрении OpenTelemetry: проброс трейсов от Ingress через .NET к downstream-сервисам (PostgreSQL, Redis, RabbitMQ, HTTP upstreams), автоматическое обнаружение медленных запросов и путь к оптимизации P99 latency.

---

#### Решение

###### 1. Интеграция OpenTelemetry в .NET 8
- Подключены NuGet-пакеты: `OpenTelemetry.Extensions.Hosting`, `Instrumentation.AspNetCore`, `Instrumentation.Http`, `Instrumentation.EntityFrameworkCore`, `Instrumentation.StackExchangeRedis`, `Instrumentation.Runtime`, `Instrumentation.Process`, `Exporter.OpenTelemetryProtocol`
- Настроен `ActivitySource` с инструментацией AspNetCore, HttpClient, EF Core, Redis
- Включён проброс W3C `traceparent`; добавлен middleware для возврата `X-Trace-Id` в заголовках ответа
- Экспорт через OTLP/gRPC в OTel Collector (`:4317`)

###### 2. Инфраструктурный слой (Kubernetes / Nginx / OTel Collector)
- **Nginx/Ingress**: инъекция заголовков `traceparent`/`tracestate` для проброса W3C контекста
- **K8s Deployment**: переменные окружения `OTEL_SERVICE_NAME`, `OTEL_EXPORTER_OTLP_ENDPOINT`, конфиг семплирования (`parentbased_traceidratio` 20%)
- **OTel Collector**: Tail-based sampling процессор — всегда захватываем ошибки (status_code: ERROR), медленные запросы (>1с latency), вероятностные 5% остальных; дропаем health checks

###### 3. Методология поиска медленных запросов
- **Top-N Slow Endpoints (P95/P99)**: Grafana дашборды с `histogram_quantile(0.99, sum(rate(http_server_request_duration_seconds_bucket[5m])) by (le, http_route))`
- **Flame Graphs & Span Waterfall**: Анализ разбивки трейса — Middleware → Controller → EF Core → Downstream API → Serialization
- **Runtime Metrics**: Мониторинг длины очереди ThreadPool, частоты Gen 2 GC, аллокаций для детекта sync-over-async блокировок
- **Live Profiling**: `dotnet-trace` + Speedscope/PerfView для CPU сэмплинга на проде под нагрузкой

###### 4. Чек-лист оптимизаций
| Категория | Применённые фиксы |
|---|---|
| **База данных** | Добавлены недостающие индексы (EXPLAIN ANALYZE), устранено N+1 через `.Include()`/`.Select()`, `.AsNoTracking()` для read-only, настроен connection pooling |
| **I/O & Сеть** | Полный перевод на async/await с CancellationToken, убраны `.Result`/`.Wait()` |
| **Аллокации и GC** | `ArrayPool<T>`, `Memory<T>`, `ReadOnlySpan<T>`, стриминг JSON в `Response.BodyWriter`, включён Server GC |
| **Кэширование** | Redis distributed cache (FusionCache), OutputCaching для HTTP-ответов |
| **HTTP Clients** | `IHttpClientFactory` / singleton `SocketsHttpHandler` с `PooledConnectionLifetime=15m` |

---

#### Технологии
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/dotnet.svg" alt=".NET"><div>.NET 8</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/opentelemetry.svg" alt="OpenTelemetry"><div>OpenTelemetry</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/kubernetes-plain.svg" alt="Kubernetes"><div>Kubernetes</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/prometheus-original.svg" alt="Prometheus"><div>Prometheus</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/grafana-original.svg" alt="Grafana"><div>Grafana</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/jaeger.svg" alt="Jaeger"><div>Jaeger</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/redis.svg" alt="Redis"><div>Redis</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/rabbitmq.svg" alt="RabbitMQ"><div>RabbitMQ</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/helm-original.svg" alt="Helm"><div>Helm</div></div>
</div>

---

#### Результаты
✅ **Полное покрытие трейсами:** 100% запросов имеют `traceparent`, `X-Trace-Id` возвращается клиентам  
✅ **P99 latency снижен:** с 2.3с до 420мс (улучшение 82%) после оптимизации топ-5 эндпоинтов  
✅ **Обнаружение ошибок:** Tail-sampling захватывает 100% 5xx ошибок и запросов >1с даже при 20% sample rate  
✅ **Проактивный алертинг:** Настроены алерты на P99 > SLA и скачки error-rate в Grafana/Alertmanager  
✅ **Оптимизация БД:** N+1 устранено, индексы добавлены, пул соединений настроен — EF Core спалы снизились на 65%  
✅ **ThreadPool здоров:** Длина очереди около нуля под пиковой нагрузкой; sync-over-async устранён  
✅ **Нагрузочное тестирование:** k6 soak-тест подтверждает стабильный P99 под 2x ожидаемого RPS  

---

#### Архитектура
{{< mermaid align="center" >}}
graph LR
    A[Ingress / Nginx] -->|W3C traceparent| B[.NET Backend]
    B -->|OTLP gRPC :4317| C[OTel Collector]
    C --> D[VictoriaMetrics / Prometheus]
    C --> E[Jaeger / Tempo]
    C --> F[Loki]
    B --> G[(PostgreSQL)]
    B --> H[(Redis)]
    B --> I[RabbitMQ]
    B --> J[HTTP Upstreams]
{{< /mermaid >}}

---

#### Длительность
Этап 1: Инфраструктура и пайплайны Observability  12 ч
Этап 2: Инструментация .NET приложения 10 ч
Этап 3: Дашбординг, Алертинг и Базовый аудит 8 ч
Этап 4: Оптимизация и устранение узких мест (Code & DB) 20 ч
Этап 5: Нагрузочное тестирование, Валидация и Документация 8 ч
Итого 58 часов / 10 дней (интеграция + конфиг коллектора + спринт оптимизаций + нагрузочное тестирование)

---

#### Стоимость
от 170 000 ₽