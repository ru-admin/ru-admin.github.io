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
{{< split 2 2 2 2 2 2 >}}
![GitLab](/icons/gitlab-original.svg) GitLab
---
![Docker](/icons/docker-original.svg) Docker
---
![Kubernetes](/icons/kubernetes-plain.svg) K8s
---
![Helm](/icons/helm-original.svg) Helm
---
![Flux CD](/icons/flux-cd.svg) Flux CD
{{< /split >}}

---

#### Результаты
✅ **Время деплоя:** с 2 часов до 10 минут (12x)  
✅ **Ошибки:** −90%  
✅ **Частота деплоя:** с 1/неделя до 10+/день  
✅ **Время отката:** с 1 часа до 2 минут  

---

#### Архитектура
```
GitLab CI → Harbor → Flux CD → Kubernetes
```

---

#### Длительность
5 дней

---

#### Стоимость
от 40 000 ₽

