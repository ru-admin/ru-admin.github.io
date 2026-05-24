---
title: "Self-Hosted Project Management with Plane"
description: "Deploying open-source Plane CE in Coolify with a custom Docker Compose, Traefik integration, and automated backups to AWS S3"
hero: "hero.webp"
tags: ["plane", "coolify", "docker", "traefik", "postgresql", "minio", "aws-s3"]
menu:
  sidebar:
    name: "Plane Project Management"
    identifier: plane-so
    parent: self-hosted
    weight: 40
categories:
- Self-Hosted
---

## Self-Hosted Project Management with Plane So

---

#### Client
A company seeking its own project management tool hosted on their office infrastructure

---

#### Challenge
The client wanted an open-source alternative to Jira/Linear for project and task management, deployed on their own server within an existing Coolify environment. Requirements included: installing the latest version of Plane CE, ensuring proper operation behind Traefik reverse proxy, extracting the database as a separate Coolify service for convenient archiving, and setting up regular backups for all data.

---

#### Solution

###### 1. Infrastructure Preparation
- Analysis of the existing **Coolify** environment and **Traefik** configuration on the client's server
- Selection of the latest open-source version of **Plane CE** from the [official repository](https://github.com/makeplane/plane)
- Review of the [self-hosting documentation](https://developers.plane.so/self-hosting/methods/docker-compose) to establish the base configuration

###### 2. Custom Docker Compose
- Based on the official docker-compose, a **custom Docker Compose** was created, adapted for Coolify
- Plane services (web, space, api, worker, beat-worker) configured to work behind **Traefik** — proper labels, networks, and routing
- **PostgreSQL** extracted as a separate Coolify service — allowing independent database management and leveraging Coolify's built-in archiving mechanism
- **Redis** for caching and task queues
- **MinIO** as an S3-compatible object storage for attachments and assets

###### 3. Traefik Integration
- **Traefik labels** configured for automatic traffic routing to Plane services
- Automatic **SSL/TLS** certificate provisioning and renewal via Traefik
- Proper header forwarding (X-Forwarded-For, Host) for correct application behavior behind reverse proxy

###### 4. Backup Strategy
- **PostgreSQL** — regular backups via Coolify with export to **AWS S3**
- **MinIO** — regular object storage synchronization to a dedicated **AWS S3** bucket
- All backups run automatically on schedule without manual intervention

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/aws.svg" alt="AWS S3"><div>AWS S3</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
</div>

---

#### Results
✅ **Project Management:** a powerful self-hosted project management tool on par with commercial alternatives  
✅ **Integration:** Plane seamlessly integrated into Coolify and running behind Traefik without conflicts  
✅ **Database Independence:** PostgreSQL as a separate Coolify service — convenient management and archiving  
✅ **Backups:** all data (DB + MinIO) regularly copied to AWS S3 automatically  
✅ **Data Control:** all project data stored on the client's own server  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[Users] --> B[Traefik :443]
    B --> C[Plane Web]
    B --> D[Plane Space]
    B --> E[Plane API]
    E --> F[PostgreSQL — Separate Coolify Service]
    E --> G[Redis]
    E --> H[MinIO]
    E --> I[Worker / Beat-Worker]
    F --> J[AWS S3 — DB Backup]
    H --> K[AWS S3 — MinIO Backup]
{{< /mermaid >}}

---

#### Duration
1 day (deployment + Docker Compose customization + backup configuration + testing)

---

#### Pricing
from $180
