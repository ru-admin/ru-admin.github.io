---
title: "Low-Latency Trading Proxy for Binance"
description: "Deploying a two-server proxy chain (Khabarovsk → Tokyo) with Hysteria 2, sing-box, and Squid to reduce Binance terminal latency from 150–250 ms to ~60 ms for a trader in Belarus"
hero: "hero.webp"
tags: ["hysteria2", "sing-box", "squid", "nginx", "vpn", "proxy", "linux", "networking"]
menu:
  sidebar:
    name: "Binance Trading Proxy"
    identifier: binance-trading-proxy
    parent: self-hosted
    weight: 33
categories:
- Self-Hosted
---

## Low-Latency Trading Proxy for Binance

---

#### Client
A professional Binance trader based in Belarus experiencing high latency in their trading terminal

---

#### Challenge
The trader faced severe ping and latency spikes (150–250 ms) during active trading sessions, making trading impossible. The terminal only accepts HTTP/HTTPS proxies (no SOCKS support). Direct traffic from the Russian Far East to Binance servers often routes via Moscow/Europe, adding significant latency. The optimal path — Khabarovsk → Tokyo (Pacific cables, ~25–35 ms) → Binance AWS ap-northeast-1 (~1–3 ms) — was not being used.

---

#### Solution

###### 1. Two-Server Architecture
- **Tokyo server** (closest to Binance AWS ap-northeast-1): deployed **Hysteria 2** VPN server with **nginx** serving a decoy website, configured TLS certificates for both nginx and Hysteria 2 with additional obfuscation to prevent traffic blocking
- **Khabarovsk server** (closest to client): deployed **sing-box** client connecting to Tokyo Hysteria 2, exposing a local SOCKS proxy, with **Squid** HTTP/HTTPS proxy (basic auth) as the terminal-facing endpoint

###### 2. Protocol & Obfuscation
- Hysteria 2 over QUIC for low-latency, loss-resilient transport
- TLS certificates shared between nginx (decoy site) and Hysteria 2
- Additional obfuscation layer to bypass DPI/blocking

###### 3. Proxy Selection & Tuning
- Tested HAProxy, 3proxy, and sing-box built-in proxy — all had either terminal authentication issues or added network latency
- **Squid with basic auth** proved to be the only solution compatible with the trading terminal while maintaining low latency

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/hysteria2.svg" alt="Hysteria 2"><div>Hysteria 2</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/sing-box.svg" alt="sing-box"><div>sing-box</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/squid.webp" alt="Squid"><div>Squid</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/nginx.svg" alt="Nginx"><div>Nginx</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
</div>

---

#### Results
✅ **Latency reduced** from 150–250 ms to **~60 ms average** terminal-to-exchange ping  
✅ **Trading terminal compatibility** — Squid basic auth works seamlessly with the terminal's HTTP/HTTPS proxy requirement  
✅ **Optimal routing** — traffic flows Khabarovsk → Tokyo (Pacific cables) → Binance AWS ap-northeast-1  
✅ **Resilience** — Hysteria 2 QUIC handles packet loss gracefully; obfuscation prevents blocking  
✅ **Zero terminal changes** — trader continues using their existing terminal with only proxy settings updated  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[Trading Terminal<br/>Belarus] -->|HTTP/HTTPS + Basic Auth| B[Squid Proxy<br/>Khabarovsk]
    B -->|SOCKS| C[sing-box Client<br/>Khabarovsk]
    C -->|Hysteria 2 / QUIC<br/>Obfuscated TLS| D[Hysteria 2 Server<br/>Tokyo]
    D -->|~1-3 ms| E[Binance AWS<br/>ap-northeast-1]
    D -.->|Decoy Site + TLS| F[nginx + Decoy Site<br/>Tokyo]
{{< /mermaid >}}

---

#### Duration
3 hours (server provisioning, Hysteria 2 + nginx + certs setup, sing-box + Squid configuration, testing & tuning)

---

#### Pricing
$80