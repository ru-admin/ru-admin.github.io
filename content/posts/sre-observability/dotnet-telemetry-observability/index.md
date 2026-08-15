---
title: ".NET Backend Observability & Telemetry Integration"
description: "OpenTelemetry implementation for distributed tracing, metrics, and performance optimization of .NET 8 backend"
hero: "hero.webp"
tags: ["kubernetes", "docker", "opentelemetry", "dotnet", "observability", "jaeger", "prometheus"]
menu:
  sidebar:
    name: ".NET Telemetry & Observability"
    identifier: dotnet-telemetry-observability
    parent: sre-observability
    weight: 50
categories:
- SRE
- Observability
---

## .NET Backend Observability: Telemetry Integration & Performance Optimization

---

#### Client
E-commerce platform with high-load .NET 8 backend

---

#### Challenge
The .NET backend had no observability — no distributed tracing, no centralized metrics, no structured logging. Finding slow requests required manual log digging. The team needed full OpenTelemetry integration: trace propagation from Ingress through .NET to downstream services (PostgreSQL, Redis, RabbitMQ, HTTP upstreams), automatic slow-query detection, and a path to optimize P99 latency.

---

#### Solution

###### 1. OpenTelemetry Integration in .NET 8
- Added OpenTelemetry NuGet packages: `OpenTelemetry.Extensions.Hosting`, `Instrumentation.AspNetCore`, `Instrumentation.Http`, `Instrumentation.EntityFrameworkCore`, `Instrumentation.StackExchangeRedis`, `Instrumentation.Runtime`, `Instrumentation.Process`, `Exporter.OpenTelemetryProtocol`
- Configured `ActivitySource` with AspNetCore, HttpClient, EF Core, Redis instrumentations
- Enabled W3C `traceparent` propagation; added middleware to return `X-Trace-Id` in response headers
- Exported via OTLP/gRPC to OTel Collector (`:4317`)

###### 2. Infrastructure Layer (Kubernetes / Nginx / OTel Collector)
- **Nginx/Ingress**: injected `traceparent`/`tracestate` headers for W3C context propagation
- **K8s Deployment**: environment variables for `OTEL_SERVICE_NAME`, `OTEL_EXPORTER_OTLP_ENDPOINT`, sampling config (`parentbased_traceidratio` at 20%)
- **OTel Collector**: Tail-based sampling processor — always capture errors (status_code: ERROR), slow requests (>1s latency), probabilistic 5% for the rest; drop health checks

###### 3. Methodology for Finding Slow Requests
- **Top-N Slow Endpoints (P95/P99)**: Grafana dashboards with `histogram_quantile(0.99, sum(rate(http_server_request_duration_seconds_bucket[5m])) by (le, http_route))`
- **Flame Graphs & Span Waterfall**: Analyze trace breakdown — Middleware → Controller → EF Core → Downstream API → Serialization
- **Runtime Metrics**: Monitor ThreadPool queue length, Gen 2 GC frequency, allocations to detect sync-over-async blocking
- **Live Profiling**: `dotnet-trace` + Speedscope/PerfView for CPU sampling on production under load

###### 4. Optimization Checklist Applied
| Category | Fixes Applied |
|---|---|
| **Database** | Added missing indexes (EXPLAIN ANALYZE), eliminated N+1 with `.Include()`/`.Select()`, `.AsNoTracking()` for read-only, tuned connection pooling |
| **I/O & Network** | Full async/await with CancellationToken, removed `.Result`/`.Wait()` |
| **Allocations & GC** | `ArrayPool<T>`, `Memory<T>`, `ReadOnlySpan<T>`, streaming JSON to `Response.BodyWriter`, enabled Server GC |
| **Caching** | Redis distributed cache (FusionCache), OutputCaching for HTTP responses |
| **HTTP Clients** | `IHttpClientFactory` / singleton `SocketsHttpHandler` with `PooledConnectionLifetime=15m` |

---

#### Technologies
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

#### Results
✅ **Full trace coverage:** 100% of requests have `traceparent`, `X-Trace-Id` returned to clients  
✅ **P99 latency reduced:** from 2.3s → 420ms (82% improvement) after optimizing top-5 endpoints  
✅ **Error detection:** Tail-sampling captures 100% of 5xx errors and >1s requests even at 20% sample rate  
✅ **Proactive alerting:** P99 > SLA alerts + error-rate spikes configured in Grafana/Alertmanager  
✅ **Database optimization:** N+1 eliminated, missing indexes added, connection pool tuned — EF Core spans down 65%  
✅ **ThreadPool health:** Queue length near zero under peak load; sync-over-async eliminated  
✅ **Load test validated:** k6 soak test confirms stable P99 under 2x expected RPS  

---

#### Architecture
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

#### Duration
2 weeks (integration + collector config + optimization sprint + load testing)

---

#### Cost
from $2,000