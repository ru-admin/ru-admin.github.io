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
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/gitlab-original.svg" alt="GitLab"><div>GitLab</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/kubernetes-plain.svg" alt="Kubernetes"><div>Kubernetes</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/helm-original.svg" alt="Helm"><div>Helm</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/flux-cd.svg" alt="Flux CD"><div>Flux CD</div></div>
</div>

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
