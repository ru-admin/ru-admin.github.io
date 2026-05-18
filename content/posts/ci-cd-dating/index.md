---
title: "CI/CD & Production Infrastructure for a Social App"
description: "Full production DevOps stack from scratch: Docker, GitLab CI/CD, monitoring, automated backups"
hero: "hero.webp"
tags: ["gitlab", "docker", "ansible", "prometheus", "nginx", "postgresql"]
menu:
  sidebar:
    name: "CI/CD Dating service"
    identifier: cicd-dating-service
    weight: 60
---

## Production Infrastructure & CI/CD for a Social App Launch

---

#### Client
Puzzle Master — a social matching platform

---

#### Challenge
The startup had a production-ready Nest.js backend and Angular frontend, but zero infrastructure: deployments were manual, there was no CI/CD, no monitoring, no backups, and no separation between dev and prod environments. The goal was to build a complete DevOps stack from scratch before the public launch.

---

#### Solution

###### 1. Application Containerization
- Multi-stage Dockerfile for backend (Nest.js + Prisma, non-root user)
- Multi-stage Dockerfile for frontend (Angular 12, legacy OpenSSL, Nginx for static assets)
- Docker Compose full stack: PostgreSQL 15, Redis 7, imgproxy, Nginx
- Healthchecks and `depends_on` for correct startup ordering
- Isolated dev and prod environments in `/opt/dev` and `/opt/prod`

###### 2. GitLab CI/CD
- Migration of repository from Bitbucket to GitLab
- Pipeline for backend and frontend: build → push → deploy
- GitLab Container Registry for Docker image storage
- Automatic deploy to dev on every push; manual trigger for prod
- SSH deployment to VPS via `SSH_PRIVATE_KEY`

###### 3. Nginx Reverse Proxy
- Environment-agnostic config via `envsubst` for dev/prod parity
- SSL/TLS (TLSv1.2, TLSv1.3) with Cloudflare certificates
- Routing: `/api/*` → backend:4000, `/*` → frontend:80
- www → root domain redirect (301)
- Separate imgproxy stack with SSL termination

###### 4. Security (Ansible)
- Server hardening via Ansible: SSH key-only auth, root login disabled
- UFW Firewall: only ports 80, 443, and custom SSH open
- Database accessible only via SSH tunnel
- All secrets stored in GitLab CI/CD variables

###### 5. Monitoring
- Prometheus + Grafana with automated dashboard provisioning
- Exporters: Node, cAdvisor, Postgres, Redis, Nginx, Blackbox
- 5 Grafana dashboards: server, Docker containers, PostgreSQL, Redis, Nginx
- Alertmanager with Slack/webhook integration; alerts on CPU/RAM/Disk/API/SSL

###### 6. Database Backups
- Automated `pg_dump` every hour
- gzip compression and upload to S3-compatible object storage (Cloudflare R2)
- Prometheus backup metrics: success status, size, timestamp
- Alerts: `DatabaseBackupMissing`, `DatabaseBackupFailed`, `DatabaseBackupSizeAnomaly`

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/gitlab-original.svg" alt="GitLab"><div>GitLab CI</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/ansible-original.svg" alt="Ansible"><div>Ansible</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/prometheus-original.svg" alt="Prometheus"><div>Prometheus</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/nginx.svg" alt="Nginx"><div>Nginx</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
</div>

---

#### Results
✅ **Deploy:** git push to main → automatic build and deploy to server  
✅ **Environments:** full dev/prod isolation on a single VPS  
✅ **Monitoring:** 5 dashboards, alerts across 6 categories  
✅ **Backups:** automated hourly `pg_dump` to Cloudflare R2  
✅ **Security:** UFW, key-based SSH, database inaccessible from outside  
✅ **Scalability:** architecture ready for database extraction to a dedicated server  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[git push] --> B[GitLab CI/CD]
    B --> C[Container Registry]
    B --> D[VPS via SSH]
    D --> E[Nginx :443]
    E --> F[Backend Nest.js]
    E --> G[Frontend Angular]
    F --> H[PostgreSQL]
    F --> I[Redis]
    E --> J[imgproxy]
    J --> K[Cloudflare R2]
    L[Prometheus] --> F
    L --> H
    L --> I
    L --> E
    L --> M[Grafana]
    L --> N[Alertmanager]
    N --> O[Slack / Webhook]
{{< /mermaid >}}

---

#### Duration
42 hours (Docker, CI/CD, monitoring, alerting, backups, documentation)

---

#### Cost
from $1,300
