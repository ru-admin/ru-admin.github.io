---
title: "Self-Hosted Corporate Messenger (Matrix)"
description: "Deployment of a secure self-hosted messaging platform for enterprise communications"
hero: "hero.webp"
tags: ["matrix", "docker", "caddy", "postgresql", "element"]
menu:
  sidebar:
    name: "Matrix messenger"
    identifier: matrix-messenger
    weight: 50
---

## Self-Hosted Secure Messenger for Enterprise Communications

---

#### Client
Mid-size business with strict data privacy and security requirements

---

#### Challenge
The company required full control over its internal communications — no third-party servers, no data leakage risks. The solution had to support end-to-end encryption, voice and video calls, file sharing, and seamless integration with existing corporate infrastructure, all manageable by an internal team.

---

#### Solution

###### 1. Server Stack
- **Matrix Synapse** as the core messaging server
- **PostgreSQL 16** for persistent data storage
- **Caddy** as reverse proxy with automatic SSL/TLS
- **Docker Compose** for service orchestration

###### 2. Client Applications
- **Element Web** for browser access
- **Element Desktop** for Windows/macOS/Linux
- **Element Mobile** for iOS/Android
- Consistent interface across all platforms

###### 3. Voice & Video Calls
- **Coturn** (TURN/STUN server) for NAT traversal
- Group video call support
- UDP ports 49160–49200 for media traffic
- Automatic configuration via environment variables

###### 4. Administration
- **Synapse Admin** web UI for user and room management
- Usage statistics and monitoring
- Accessible on a dedicated port (8888)

###### 5. Security
- End-to-end encrypted messages
- Automatic SSL/TLS certificates via Caddy
- Public registration disabled
- Optional federation with other Matrix servers
- Healthchecks on all services

###### 6. Automation
- Single Bash script for full stack initialization
- Automatic Synapse config generation
- Automated admin user creation via `expect`
- Docker Compose with dependency ordering and healthchecks

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/caddy.svg" alt="Caddy"><div>Caddy</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/element.svg" alt="Element"><div>Element + Matrix + Synapse</div></div>
</div>

---

#### Results
✅ **Data ownership:** full control over messages and user data — no third-party servers  
✅ **Scale:** 100+ concurrent users  
✅ **Features:** text, voice, video, file sharing up to 1.5 GB, E2E encryption  
✅ **Speed:** full deployment in 5 minutes with a single script  
✅ **Reliability:** automatic SSL certificates, healthchecks, auto-restart  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[Users] --> B[Caddy :443]
    B --> C[Matrix Synapse :8008]
    B --> D[Element Web :80]
    B --> E[Synapse Admin :8888]
    C --> F[PostgreSQL :5432]
    A --> G[Coturn :3478/5349]
    G --> A
{{< /mermaid >}}

---

#### Duration
1 day (installation + configuration + testing)

---

#### Cost
from $150
