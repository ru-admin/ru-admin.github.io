---
title: "CI/CD с GitLab + Kubernetes: деплой за 10 минут"
description: "Автоматизация CI/CD: от 2 часов до 10 минут деплоя"
hero: "hero.webp"
tags: ["gitlab", "kubernetes", "docker", "fluxcd", "helm"]
menu:
  sidebar:
    name: "CI/CD в k8s"
    identifier: cicdk8s
    weight: 10
---

## Автоматизация CI/CD: от 2 часов до 10 минут деплоя

---

#### Клиент
Стартап в сфере e-commerce, команда разработки 5 человек

---

#### Задача
- Ручной деплой занимал 2 часа
- Частые ошибки при развертывании
- Невозможность быстро откатить изменения
- Требовалось: автоматизация CI/CD, GitOps, быстрый откат

---

#### Решение
1. Self-hosted GitLab
2. GitLab CI пайплайн (Build → Test → Deploy)
3. Managed Kubernetes в Yandex Cloud
4. Flux CD для GitOps

---

#### Технологии
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/gitlab-original.svg" alt="GitLab"><div>GitLab</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/kubernetes-plain.svg" alt="Kubernetes"><div>Kubernetes</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/helm-original.svg" alt="Helm"><div>Helm</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/flux-cd.svg" alt="Flux CD"><div>Flux CD</div></div>
</div>

---

#### Результаты
✅ **Время деплоя:** с 2 часов до 10 минут (12x)  
✅ **Ошибки:** −90%  
✅ **Частота деплоя:** с 1/неделя до 10+/день  
✅ **Время отката:** с 1 часа до 2 минут  

---

#### Архитектура
{{< mermaid align="center" >}}
graph LR
    A[GitLab CI] --> B[Harbor]
    B --> C[Flux CD]
    C --> D[Kubernetes]
{{< /mermaid >}}

---

#### Длительность
5 дней

---

#### Стоимость
от 50 000 ₽
