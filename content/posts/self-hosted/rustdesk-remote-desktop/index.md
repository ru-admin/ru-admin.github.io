---
title: "Self-Hosted Remote Desktop with RustDesk"
description: "Deploying RustDesk OSS server for a 1C platform support company to provide secure remote desktop access for their team"
hero: "hero.webp"
tags: ["rustdesk", "remote-desktop", "self-hosted", "docker", "linux", "1c"]
menu:
  sidebar:
    name: "RustDesk Remote Desktop"
    identifier: rustdesk-remote-desktop
    parent: self-hosted
    weight: 34
categories:
- Self-Hosted
---

## Self-Hosted Remote Desktop with RustDesk

---

#### Client
A company providing support and maintenance for the 1C platform

---

#### Challenge
The client needed a secure, self-hosted remote desktop solution to allow their support engineers to connect to client workstations and servers. They required full control over data, no reliance on third-party cloud services, and a solution that works reliably within corporate networks.

---

#### Solution
- Deployed **RustDesk OSS** (open-source) server using Docker Compose
- Configured **hbbs** (ID/relay server) and **hbbr** (relay server) components
- Configured firewall rules and network access for secure connections
- Provided client configuration files for easy deployment across the support team
- Documented maintenance procedures and backup strategy

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/rustdesk.svg" alt="RustDesk"><div>RustDesk</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
</div>

---

#### Results
✅ **Full data sovereignty** — all connection metadata and relay traffic stays on client's infrastructure  
✅ **Zero licensing costs** — open-source RustDesk eliminates per-seat fees  
✅ **Reliable connectivity** — engineers can access client machines from anywhere  
✅ **Simple client rollout** — pre-configured client configs for quick team onboarding  
✅ **Corporate network friendly** — works through NAT/firewalls with relay servers  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[Support Engineers] -->|RustDesk Client| B[Traefik :443]
    B --> C[hbbs — ID/Relay Server]
    B --> D[hbbr — Relay Server]
    C --> E[(PostgreSQL<br/>Metadata)]
    D --> F[Client Workstations<br/>& Servers]
{{< /mermaid >}}

---

#### Duration
12 hours (server provisioning, RustDesk, SSL, testing, documentation)

---

#### Pricing
$430 / 36,000 ₽