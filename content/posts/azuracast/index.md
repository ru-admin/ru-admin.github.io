---
title: "Internet Radio on AzuraCast"
description: "Deployment and maintenance of an internet radio station for a community of independent artists"
hero: "hero.webp"
tags: ["azuracast", "linux", "joomla", "hestia", "nginx", "icecast", "liquidsoap"]
menu:
  sidebar:
    name: "AzuraCast Internet Radio"
    identifier: azuracast
    weight: 60
---

## Recovery and deployment of internet radio from scratch

---

#### Client
Community of independent artists with a mobile application and an audience of several dozen daily listeners

---

#### Challenge
After losing a rented VPS (along with the active configuration, website, and broadcast history), it was necessary to completely restore broadcasting for two internet radio stations. The only input data was a set of mp3 files. The task required deploying a new server, setting up an automated broadcasting platform, recreating a landing page with an embedded player, and ensuring stable 24/7 operation.

---

#### Solution

###### 1. Infrastructure and control panel
- Rented a new VPS and installed **Hestia Control Panel** as the base server management layer
- Hestia provided: basic protection (fail2ban, firewall), automatic acquisition and renewal of **Let's Encrypt** certificates, domain management, and website backup
- Created a separate domain in Hestia for AzuraCast — for obtaining SSL certificates; the domain was configured as a **reverse proxy** to the AzuraCast port
- Nginx within Hestia acted as a frontend proxy: terminated TLS and forwarded traffic to the internal service

###### 2. AzuraCast — broadcasting platform
- Installed **AzuraCast** (self-hosted, Docker variant) from scratch on the prepared server
- Deployed two independent radio stations within a single installation:
  - **Station 1** — meditative and ethnic music, long compositions (including tracks up to 30 minutes in duration)
  - **Station 2** — original music by independent artists from the community
- Configured the broadcasting stack: **Liquidsoap** (automation and rotation) + **Icecast** (streaming server for listeners)
- Uploaded and cataloged the entire media library from the provided mp3 files

###### 3. Rotation and broadcast scheduling
- Configured **smart rotation** taking into account track duration: long compositions (20–30 minutes) were moved to the night schedule to avoid disrupting the daytime listening experience
- Daytime broadcast composed of short and medium tracks with a comfortable rhythm
- Added **jingles (station IDs)** at 30-minute intervals — to create a radio feel and station branding
- Configured playlists with different rotation modes (sequential, shuffled, scheduled)

###### 4. Statistics and monitoring
- Activated the built-in AzuraCast statistics module (**AzuraCast Analytics**) based on **InfluxDB**
- Data collection: real-time listener count, connection history, listener geography, popular tracks, peak loads
- The data confirmed the audience: 20–30 unique listeners daily, sessions lasting several hours, peaks up to 50 simultaneous connections

###### 5. Broadcasting formats and streams
- Configured multiple **Mount Points** for each station with different bitrates:
  - High quality: **MP3 320 kbps** for desktop clients
  - Medium quality: **MP3 128 kbps** for mobile devices and the embedded player in the application
  - **AAC / OGG** as alternative formats
- Configured stream metadata: station name, current track, cover art — all transmitted in ICY metadata

###### 6. Landing page on Joomla
- On the same server (separate domain via Hestia) deployed **Joomla CMS** with bilingual configuration (ru / en)
- Used **Helix3 Ultimate Framework** + **SP Page Builder** page constructor — allowed building a landing page without manual coding
- Site structure: homepage with community and radio station descriptions, embedded **HTML5 player from AzuraCast** (iframe embed) for both stations, contact information
- The site is accessible via direct links and serves as an entry point for listeners outside the mobile application

###### 7. Backup
- **AzuraCast backup** configured on schedule using the built-in script: full backup (configuration, database, media library) saved locally
- Configured **backup replication via SSH** to a remote server — to protect against data loss in case of primary host failure (this scenario had already occurred previously)
- Website backup (Joomla) — via the built-in **Hestia** mechanism

---

#### Technologies
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Linux"><div>Linux</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/nginx.svg" alt="Nginx"><div>Nginx</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/docker-original.svg" alt="Docker"><div>Docker</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/bash.svg" alt="Bash"><div>Bash</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/joomla.svg" alt="Joomla"><div>Joomla</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/lets-encrypt.svg" alt="Let's Encrypt"><div>Let's Encrypt</div></div>
</div>

---

#### Results
✅ **Recovery:** broadcasting resumed from scratch using the only source data — a set of mp3 files  
✅ **Two stations:** independent broadcasts with different formats and rotation schedules  
✅ **Audience:** 20–30 daily listeners, sessions lasting several hours, peaks up to 50 simultaneous connections  
✅ **Reliability:** 24/7 operation, multi-level backup (local + remote SSH)  
✅ **Security:** SSL/TLS for all entry points, fail2ban, isolated domains  
✅ **Maintenance:** technical support for ~2 years — updates, adding tracks, website edits  

---

#### Architecture
{{< mermaid align="center" >}}
graph TB
    A[Listeners / Mobile App] --> B[Nginx / Hestia :443]
    B --> C[AzuraCast Docker]
    C --> D[Liquidsoap — automation]
    D --> E[Icecast — streaming]
    E --> A
    B --> F[Joomla — landing page]
    F --> G[Embedded AzuraCast player]
    C --> H[InfluxDB — statistics]
    C --> I[Local backup]
    I --> J[Remote server via SSH]
{{< /mermaid >}}

---

#### Duration
2–3 days (deployment + configuration + media library population)

\+ 2 years of technical support

---

#### Price
from $200
