---
title: "CI/CD & Production Infrastructure for a Web Application"
description: "Full DevOps stack from scratch: GitLab CI/CD, Docker Compose, two environments (test/prod), monitoring, Telegram alerting, automated backups"
hero: "hero.webp"
tags: ["gitlab", "docker", "prometheus", "grafana", "nginx", "postgresql"]
menu:
  sidebar:
    name: "CI/CD Web Application"
    identifier: cicd-web-app
    parent: cicd
    weight: 12
categories:
- CI/CD
---

## Production Infrastructure & CI/CD for a Company Website

---

#### Client
A manufacturer of branded research products that needed a website to present its product catalog to customers

---

#### Challenge
The developers had written the code of a conventional web application (NestJS backend + Next.js frontend), but infrastructure was completely missing: manual deployments, no CI/CD, no server setup, no monitoring, no backups and no separation between test and production environments. The goal was to build a complete DevOps stack from scratch: GitLab CI/CD, a self-hosted server, two isolated environments, HTTPS, monitoring, alerting and automated backups.

---

#### Solution

###### 1. Server Provisioning & Security
- Ubuntu 24.04 VPS: 4 vCPU, 6 GB RAM, 120 GB disk
- SSH key-only authentication, root login disabled, dedicated deployment user (sudo + docker)
- UFW firewall: only ports 80, 443, 22 and SFTP (2222/2223) open
- Automated security updates (unattended-upgrades)

###### 2. GitLab CI/CD
- Self-hosted GitLab with a group-level Container Registry
- Pipelines for backend and frontend: build → push → deploy
- Branch mapping: `test` → image `:test` → test environment; `master` → image `:latest` → prod environment; `v*` tags → release builds; `dev` → build only
- SSH deployment to the server via `SSH_PRIVATE_KEY`, protected CI variables for secrets
- Infrastructure repo `server-devops` with a `deploy-all` job: hard reset to origin + `docker compose up -d --pull always`

###### 3. Docker Compose Environments
- Three compose files: `app/` (test), `app-prod/` (prod), `monitoring/` on a shared Docker network
- Services per environment: NestJS backend, Next.js frontend, PostgreSQL 16, pgAdmin, SFTP
- Restart policy `on-failure:10` for all application containers
- All configs, dashboards and rules stored in git, applied by pipeline

###### 4. Reverse Proxy & HTTPS
- Single `nginx-proxy` + `acme-companion` reverse proxy for 80/443
- Automatic Let's Encrypt certificates with auto-renewal (acme-companion)
- All entry points HTTPS-only: main site, test environment, database admin panel, monitoring

###### 5. Monitoring & Alerting
- Prometheus + Grafana with automated dashboard provisioning from git (4 dashboards: cAdvisor, Node Exporter, Logs/App, PostgreSQL)
- Exporters: node, cAdvisor, postgres, nginx (stub_status + access log: RPS, 4xx/5xx, response/upstream times), blackbox HTTP probes for frontend/backend
- Alertmanager with Telegram integration — one bot, separate chats per environment (test / prod)
- Alert rules: service down, high CPU/RAM/disk (>80%), restart loops, backend 5xx errors

###### 6. Logging & Backups
- Loki + Promtail: logs of all containers collected, available in Grafana (Logs Drilldown)
- Automated backup script: `pg_dump` of prod DB + tar-stream of the prod uploads volume into a single archive
- 30-day rotation, daily cron at 03:00, restore procedure verified and documented

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/gitlab-original.svg" alt="GitLab"><div>GitLab CI</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/prometheus-original.svg" alt="Prometheus"><div>Prometheus</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/grafana-original.svg" alt="Grafana"><div>Grafana</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/nginx.svg" alt="Nginx"><div>Nginx</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
</div>

---

#### Results
✅ **Deploy:** merge to master → automatic build, publish and deploy to server  
✅ **Environments:** full test/prod isolation on a single VPS via a shared Docker network  
✅ **HTTPS:** all services only over HTTPS with automatic Let's Encrypt certificates  
✅ **Monitoring:** 4 dashboards, health probes, alerts in Telegram (separate chats per environment)  
✅ **Backups:** automated daily backup of prod DB + uploads volume, 30-day rotation, restore verified  
✅ **Security:** SSH key-only, root disabled, UFW firewall, secrets in Protected CI variables  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[Merge to master / test] --> B[GitLab CI/CD]
    B --> C[GitLab Container Registry]
    B --> D[VPS via SSH]
    D --> E[nginx-proxy :443 + acme]
    E --> F[Frontend Next.js test/prod]
    E --> G[Backend NestJS test/prod]
    G --> H[PostgreSQL test/prod]
    E --> I[pgAdmin + SFTP]
    L[Prometheus] --> G
    L --> H
    L --> E
    L --> M[Grafana]
    L --> N[Alertmanager]
    N --> O[Telegram test/prod]
    P[Loki + Promtail] --> M
    Q[Backup script] --> R[Backups]
    H --> Q
{{< /mermaid >}}

---

#### Duration
10 hours (CI/CD, Docker Compose, monitoring, alerting, backups, documentation)

---

#### Cost
$400