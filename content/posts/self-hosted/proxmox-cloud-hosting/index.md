---
title: "Cloud Hosting for 1C:Enterprise on Proxmox"
description: "Building and supporting a Proxmox-based hosting platform delivering ready-to-use cloud 1C:Enterprise instances with Windows Server, PostgreSQL, and Apache"
hero: "hero.webp"
tags: ["proxmox", "windows-server", "1c", "postgresql", "routeros", "haproxy", "virtualization", "hosting"]
menu:
  sidebar:
    name: "Proxmox Cloud Hosting"
    identifier: proxmox-cloud-hosting
    parent: self-hosted
    weight: 35
categories:
- Self-Hosted
---

## Cloud Hosting for 1C:Enterprise on Proxmox

---

#### Client
A mid-tier hosting provider offering infrastructure-as-a-service to business customers

---

#### Challenge
The client needed a reliable, self-hosted virtualization platform to deliver ready-to-use cloud 1C:Enterprise instances as a managed product. The stack required: Proxmox VE cluster for virtualization, Windows Server VMs running 1C:Enterprise server + PostgreSQL + Apache, a RouterOS (MikroTik) VM as the edge router with HAProxy for load balancing, network connectivity between nodes, storage replication, and automated backups — all maintained as a commercial service for dozens of end customers.

---

#### Solution

###### 1. Proxmox VE Cluster
- Deployed and configured Proxmox VE cluster across multiple physical servers
- Shared storage and live migration for high availability
- Resource isolation and quotas per customer VM
- Regular cluster maintenance, updates, and capacity planning

###### 2. Network & Routing
- RouterOS (MikroTik) VM as the edge router and firewall
- HAProxy for load balancing and reverse proxying to backend VMs
- VLAN segmentation for customer isolation
- VPN access for secure administrative connections

###### 3. Windows Server VM Templates
- Golden image template: Windows Server + 1C:Enterprise + PostgreSQL + Apache
- Automated VM provisioning from template for new customers
- Scheduled Windows Updates and security hardening
- Per-customer resource allocation (CPU, RAM, disk)

###### 4. Replication & High Availability
- Storage-level replication between Proxmox nodes
- VM live migration for maintenance without downtime
- Failover configuration for critical customer workloads

###### 5. Backups & Disaster Recovery
- Automated daily VM backups with retention policy
- Off-site backup replication
- Documented restore procedures, periodically tested
- Backup monitoring and alerting on failures

###### 6. Monitoring & Support
- Infrastructure monitoring (host health, VM status, storage, network)
- Proactive alerting on resource exhaustion and service degradation
- Tiered support for end customers: incident handling, performance tuning, scaling

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/proxmox.svg" alt="Proxmox"><div>Proxmox</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/1c.svg" alt="1C"><div>1C</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/windows8-original.svg" alt="Windows Server"><div>Windows Server</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/postgresql.svg" alt="PostgreSQL"><div>PostgreSQL</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/nginx.svg" alt="Apache"><div>Apache</div></div>
</div>

---

#### Results
✅ **Service delivery:** dozens of customers receiving ready-to-use cloud 1C:Enterprise instances  
✅ **High availability:** live migration and replication eliminate single points of failure  
✅ **Rapid provisioning:** new customer VMs deployed from template in minutes  
✅ **Network isolation:** VLANs and RouterOS firewall keep customer traffic separated  
✅ **Backup confidence:** automated daily backups with tested restore procedures  
✅ **Proactive operations:** monitoring and alerting catch issues before customers notice  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[End Customers] --> B[RouterOS VM: Edge Router + Firewall]
    B --> C[HAProxy: Load Balancer]
    C --> D[Proxmox VE Cluster]
    D --> E[VM: Windows Server
1C:Enterprise + PostgreSQL + Apache]
    D --> F[VM: Windows Server
1C:Enterprise + PostgreSQL + Apache]
    D --> G[VM: Windows Server
1C:Enterprise + PostgreSQL + Apache]
    H[Shared Storage] --> D
    I[Backup Server] --> D
    J[Monitoring: Prometheus + Grafana] --> D
    J --> B
{{< /mermaid >}}

---

#### Duration
Continuous technical support for more than 3 years

---

#### Cost
from $300 / month
