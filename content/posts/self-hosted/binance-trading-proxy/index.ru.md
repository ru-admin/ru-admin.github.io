---
title: "Прокси с низкой латентностью для торговли на Binance"
description: "Развёртывание цепочки из двух серверов (Хабаровск → Токио) с Hysteria 2, sing-box и Squid для снижения пинга терминала Binance с 150–250 мс до ~60 мс для трейдера из Беларуси"
hero: "hero.webp"
tags: ["hysteria2", "sing-box", "squid", "nginx", "vpn", "proxy", "linux", "networking"]
menu:
  sidebar:
    name: "Прокси для Binance трейдинга"
    identifier: binance-trading-proxy
    parent: self-hosted
    weight: 33
categories:
- Self-Hosted
---

## Прокси с низкой латентностью для торговли на Binance

---

#### Клиент
Профессиональный трейдер Binance из Беларуси, столкнувшийся с высокой латентностью в торговом терминале

---

#### Задача
Во время активной торговли пинг и латентность в терминале достигали 150–250 мс, что делало торговлю невозможной. Терминал принимает только HTTP/HTTPS прокси (SOCKS не поддерживает). Прямой трафик с Дальнего Востока РФ до серверов Binance часто идёт через Москву/Европу («крюк» с пингом 150–250 мс). Оптимальный маршрут — Хабаровск → Токио по тихоокеанским магистралям (~25–35 мс) → серверы Binance (AWS ap-northeast-1, ~1–3 мс) — не использовался. Российские ТСПУ блокируют нестандартный трафик.

---

#### Решение

###### 1. Архитектура из двух серверов
- **Сервер в Токио** (ближе к Binance AWS ap-northeast-1): развёрнут **Hysteria 2** VPN-сервер, **nginx** с сайтом-заглушкой, настроены TLS-сертификаты для nginx и Hysteria 2 с дополнительной обфускацией для обхода блокировок ТСПУ
- **Сервер в Хабаровске** (ближе к клиенту): развёрнут клиент **sing-box**, подключающийся к Токио по Hysteria 2, пробрасывающий трафик в локальный SOCKS-прокси; внешний прокси — **Squid** с basic-авторизацией для терминала

###### 2. Протокол и обфускация
- Hysteria 2 поверх QUIC для низкой латентности и устойчивости к потерям пакетов
- TLS-сертификаты общие для nginx (заглушка) и Hysteria 2
- Дополнительный слой обфускации для предотвращения блокировок DPI/ТСПУ

###### 3. Подбор и настройка прокси
- Проверены HAProxy, 3proxy и встроенный прокси sing-box — у всех были проблемы с авторизацией в торговом терминале или добавка сетевых задержек
- **Squid с basic-авторизацией** стал единственным решением, совместимым с терминалом и сохраняющим низкую латентность

---

#### Технологии
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/hysteria2.svg" alt="Hysteria 2"><div>Hysteria 2</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/sing-box.svg" alt="sing-box"><div>sing-box</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/squid.webp" alt="Squid"><div>Squid</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/nginx.svg" alt="Nginx"><div>Nginx</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
</div>

---

#### Результаты
✅ **Латентность снижена** с 150–250 мс до **~60 мс** средний пинг терминал-биржа  
✅ **Совместимость с терминалом** — Squid basic auth работает прозрачно для HTTP/HTTPS прокси терминала  
✅ **Оптимальная маршрутизация** — трафик идёт Хабаровск → Токио (тихоокеанские кабели) → Binance AWS ap-northeast-1  
✅ **Устойчивость** — Hysteria 2 QUIC справляется с потерями пакетов; обфускация предотвращает блокировки  
✅ **Без изменений в терминале** — трейдер продолжает пользоваться своим терминалом, меняя только настройки прокси  

---

#### Архитектура
{{< mermaid align="center" >}}
graph TB
    A[Торговый терминал<br/>Беларусь] -->|HTTP/HTTPS + Basic Auth| B[Squid Proxy<br/>Хабаровск]
    B -->|SOCKS| C[sing-box Client<br/>Хабаровск]
    C -->|Hysteria 2 / QUIC<br/>Обфусцированный TLS| D[Hysteria 2 Server<br/>Токио]
    D -->|~1-3 мс| E[Binance AWS<br/>ap-northeast-1]
    D -.->|Заглушка + TLS| F[nginx + Заглушка<br/>Токио]
{{< /mermaid >}}

---

#### Длительность
3 часа (подготовка серверов, настройка Hysteria 2 + nginx + сертификаты, sing-box + Squid, тестирование и тюнинг)

---

#### Стоимость
6 000 ₽