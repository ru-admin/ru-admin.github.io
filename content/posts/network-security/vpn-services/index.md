---
title: "WireGuard VPN Infrastructure"
description: "Deployment of a WireGuard VPN solution for secure and reliable access to global IT services (OpenAI, GitHub Copilot)"
hero: "hero.webp"
tags: ["docker","ansible", "vpn"]
menu:
  sidebar:
    name: "VPN WireGuard"
    identifier: vpn-wg
    parent: network-security
    weight: 30
categories:
- Network Security
---

## WireGuard VPN Infrastructure

---

#### Challenge
A distributed team of 15+ engineers working across multiple locations needed secure, low-latency access to internal corporate infrastructure — without relying on heavyweight legacy VPN appliances. The existing setup introduced significant overhead, was difficult to maintain, and caused frequent connectivity issues for remote employees. The goal was to replace it with a lightweight, self-hosted solution that is fast, auditable, and easy to scale.

---

#### Solution

###### 1. Technology Selection
- Protocol evaluation: OpenVPN, WireGuard, Outline
- WireGuard chosen for its speed, simplicity, and modern cryptography
- Docker for isolation and portability
- Ansible for automated provisioning

###### 2. Infrastructure
- VPS hosted in a neutral jurisdiction (Netherlands)
- Docker Compose for service orchestration
- WireGuard running in a container
- Nginx for the web management panel
- Prometheus + Grafana for monitoring

###### 3. Automation
```yaml
# Ansible playbook for deployment
- name: Deploy WireGuard VPN
  hosts: vpn_servers
  roles:
    - docker
    - wireguard
    - monitoring
    - backup
```

###### 4. Security
- Automatic key rotation
- Firewall rules (UFW)
- Fail2ban for brute-force protection
- ChaCha20-Poly1305 traffic encryption

###### 5. Monitoring
- Bandwidth and throughput metrics
- Downtime alerts
- Connection logging
- Automatic restart on failure

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/wireguard.svg" alt="WireGuard"><div>WireGuard</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/ansible-original.svg" alt="Ansible"><div>Ansible</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/prometheus-original.svg" alt="Prometheus"><div>Prometheus</div></div>
</div>


---

#### Results
✅ **Uptime:** 99.8% over 6+ months of operation  
✅ **Speed:** stable 100+ Mbps  
✅ **Access:** OpenAI API, ChatGPT, GitHub Copilot, npm registry  
✅ **Deployment time:** ~20 minutes per new server  
✅ **Users:** 15+ developers with zero connectivity issues  

---

#### Architecture
{{< mermaid align="center" >}}
graph LR
    A[Clients] --> B[WireGuard Docker]
    B --> C[VPS NL]
    C --> D[External Services]
    B --> E[Prometheus + Grafana]
{{< /mermaid >}}

---

#### Duration
1 day (setup + automation)

---

#### Cost
from $150
