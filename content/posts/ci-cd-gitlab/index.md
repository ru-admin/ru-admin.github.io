---
title: "CI/CD with GitLab + Kubernetes: deploy in 10 minutes"
description: "CI/CD automation: from 2 hours to 10 minutes per deploy"
hero: "hero.webp"
tags: ["gitlab", "kubernetes", "docker", "fluxcd", "helm"]
menu:
  sidebar:
    name: "CI/CD in k8s"
    identifier: cicdk8s
    weight: 10
---

## CI/CD automation: from 2 hours to 10 minutes per deploy

---

#### Client
E-commerce startup, 5-person engineering team

---

#### Challenge
- Manual deployment took 2 hours
- Frequent release errors
- No fast rollback path
- Required: CI/CD automation, GitOps, fast rollback

---

#### Solution
1. Self-hosted GitLab
2. GitLab CI pipeline (Build → Test → Deploy)
3. Managed Kubernetes in Yandex Cloud
4. Flux CD for GitOps

---

#### Technologies
{{< split 2 2 2 2 2 2 >}}
<div style="text-align: center;">

![GitLab](/icons/gitlab-original.svg)

<div>GitLab</div></div>

---
<div style="text-align: center;">

![Docker](/icons/docker-original.svg)
<div>Docker</div></div>

---
<div style="text-align: center;">

![Kubernetes](/icons/kubernetes-plain.svg)
<div>Kubernetes</div></div>

---
<div style="text-align: center;">

![Helm](/icons/helm-original.svg)
<div>Helm</div></div>

---
<div style="text-align: center;">

![Flux CD](/icons/flux-cd.svg)
<div>Flux CD</div></div>

{{< /split >}}

---

#### Results
✅ **Deploy time:** from 2 hours to 10 minutes (12x)  
✅ **Errors:** −90%  
✅ **Deploy frequency:** from 1/week to 10+/day  
✅ **Rollback time:** from 1 hour to 2 minutes  

---

#### Architecture
{{< mermaid align="center" >}}
graph LR
    A[GitLab CI] --> B[Harbor]
    B --> C[Flux CD]
    C --> D[Kubernetes]
{{< /mermaid >}}

---

#### Duration
5 days

---

#### Cost
from $500
