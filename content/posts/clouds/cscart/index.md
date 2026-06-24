---
title: "Migration and DevOps for a CS-Cart Marketplace"
description: "Cloud infrastructure migration, CS-Cart Multi-Vendor reinstallation, theme and add-ons setup, wiki documentation, and n8n automation"
hero: "hero.webp"
tags: ["cs-cart", "docker", "terraform", "yandex", "cloudflare", "n8n", "forgejo"]
menu:
  sidebar:
    name: "DevOps for CS-Cart"
    identifier: cscart-devops
    parent: clouds
    weight: 20
categories:
- E-commerce
---

## Infrastructure and Deployment of a Multi-Vendor CS-Cart Marketplace

---

#### Client
Multi-vendor marketplace

---

#### Challenge
Revive a legacy marketplace project and migrate it from standard shared hosting to a modern cloud environment. Ensure high availability, build CI/CD infrastructure, set up test environments, integrate third-party delivery services, and automate routine operations with AI. At the final stage, prepare an on-premise server (NAS) on the client side for cold repository storage.

---

#### Solution

###### 1. Infrastructure in Yandex Cloud (Terraform)
- Built IaC configuration (Terraform) to provision Yandex Cloud infrastructure (virtual networks, VMs).
- Configured separate virtual machines for production and development environments.
- Attached and partitioned additional disks, deployed automated backups for databases and files to Yandex Object Storage (S3).
- Used Yandex Cloud Postbox for reliable service email delivery (domain, SPF, DKIM configured).

###### 2. Containerization and Microservices
- Migrated the marketplace from classic hosting to a Docker-based VPS environment.
- Built a custom `Dockerfile` for CS-Cart and a `docker-compose` stack with a microservice-oriented web server architecture.
- Established full-featured dev and prod environments.
- Configured deployment pipelines via a local Git server (Forgejo).

###### 3. Marketplace Platform (CS-Cart)
- Upgraded the CS-Cart core and optimized the database by removing obsolete, unused plugins.
- Deployed a clean platform installation with a new commercial theme.
- Configured multi-vendor business logic: role separation and seller dashboards.
- Integrated and configured modules: dynamic product loading, AI capabilities, smart live search, and SEO module.
- Integrated shipping (CDEK) and payment systems. Launched automated data exchange with 1C accounting systems.

###### 4. Automation and n8n
- Deployed n8n process orchestration.
- Integrated ChatGPT API to automate repetitive operations such as content generation and normalization.
- Configured workers and triggers for store business workflows.

###### 5. Security and Cloudflare
- Configured Cloudflare Proxy DNS.
- Implemented Cloudflare Turnstile to reduce spam during sign-up and checkout flows.
- Added bot protection: disabled direct registrations, configured strict caching, and filtered low-quality traffic.
- Installed SSL certificates and configured proper redirects.

###### 6. On-Premise Infrastructure and Documentation
- Set up a dedicated subdomain with a wiki engine for technical and user documentation.
- Implemented local infrastructure in the client’s office based on Synology NAS: Docker, S3 buckets, and backups.
- Deployed a local Git server (Forgejo) with a CI/CD runner for version control and long-term preservation of project assets.
- Collected a complete backup of Terraform states, passwords, codebases, and configurations.

###### 7. Project Archival and Repositories
- Migrated the production site to backup hosting for archival mode and disabled active synchronizations.
- Procured and configured Synology NAS in the office: external access, Docker, S3 buckets.
- Deployed Forgejo with a runner under the `git.*` domain.
- Repositories included: website source code, theme and add-ons, documentation, n8n workflows, Telegram bots, DB backups, terraform-yandex, terraform-cloudflare, and password vault.

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/terraform-original.svg" alt="Terraform"><div>Terraform</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/yandex.svg" alt="Yandex Cloud"><div>Yandex Cloud</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/nginx.svg" alt="Nginx"><div>Nginx</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/forgejo-original.svg" alt="Forgejo"><div>Forgejo</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
</div>

---

#### Results
✅ **Infrastructure:** shared hosting → VPS in Yandex Cloud via Terraform, fully containerized with Docker  
✅ **Platform:** clean CS-Cart Multi-Vendor installation replaced a heavily modified legacy core  
✅ **Theme and modules:** commercial theme, live search, UX/UI improvements, SMS authentication, AJAX catalog  
✅ **Integrations:** CDEK, Boxberry, Russian Post, Faster Payments System (SBP), and dual-environment 1C exchange  
✅ **Documentation:** wiki on `docs.*` subdomain with a complete project structure  
✅ **Automation:** n8n + ChatGPT API for routine operations  
✅ **Backups:** Yandex Object Storage + mirrored copy on office Synology NAS  
✅ **Git:** self-hosted Forgejo on NAS with runner and full project repositories  
✅ **Archival handover:** project fully transferred to the client’s on-premise Synology NAS, including a private Forgejo Git server.

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    Client[Users] --> CF[Cloudflare Proxy / Turnstile]
    CF --> YC[Yandex Cloud VPS]
    YC --> Nginx[Nginx Reverse Proxy]
    Nginx --> CSCart[CS-Cart / Docker]
    CSCart --> DB[(MySQL/MariaDB)]
    Nginx --> Docs[Wiki Docs]
    Nginx --> n8n[n8n Automation]
    n8n --> OpenAI[ChatGPT API]
    CSCart <--> CDEK[CDEK API]
    CSCart <--> 1C[1C Integration]
    YC --> S3[Yandex S3 Backups]
{{< /mermaid >}}

---

#### Duration
Approximately 12 months of support, ~400 hours of infrastructure, deployment, and integration work

---

#### Cost
from  $3,500