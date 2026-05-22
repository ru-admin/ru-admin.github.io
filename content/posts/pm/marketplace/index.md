---
title: "Project Management: recovery and launch of a multi-vendor marketplace"
description: "Tech Lead and Project Manager role: team, contractors, requirements, budget, roadmap from freeze to production launch"
hero: "hero.webp"
tags: ["project-management", "team-lead", "cs-cart", "tech-lead", "ecommerce"]
menu:
  sidebar:
    name: "Marketplace PM"
    identifier: pm-marketplace
    parent: pm
    weight: 10
categories:
- PM
---

## Development Management for a Multi-Vendor E-commerce Project

---

#### Client
Startup — a multi-vendor marketplace.

---

#### Challenge
The marketplace had been deeply frozen for about two years. Before the freeze, the team attempted to build a custom template, but it remained unfinished. My initial goal as Tech Lead / Project Manager was to recover the project, hire and coordinate a distributed team (1C developers, web developers, SEO contractors, marketers), and bring the product to a working state end-to-end: from storefront to checkout.

---

#### Solution

###### 1. Audit and technical direction reset
- Conducted a full audit of the inherited codebase and unfinished custom template.
- Identified that the template was outdated, conflicted with the CS-Cart core (including prior core modifications by the previous team), and did not support required plugins.
- Made and aligned with stakeholders a decisive move: drop legacy code debt, deploy a clean CS-Cart version, and purchase a modern ready-made theme. This saved a significant portion of the development budget.

###### 2. Team setup and contractor management
- Hired missing web developers, ran onboarding assessments, and distributed infrastructure access (GitLab, hosting, servers).
- Structured collaboration with external vendors: SEO agency, marketers, and business analysts.
- Implemented task management workflow (issues in tracking systems), code review, acceptance process, and testing on a dedicated dev environment before production merges.

###### 3. Feature and product management (Product Management)
- **Tokenization and security:** implemented mandatory SMS-based registration to reduce fake accounts. To minimize SMS gateway costs from bot traffic, initiated anti-spam integration (Cloudflare Turnstile).
- **Marketplace UX/UI:** initiated development of an AJAX dynamic product loading plugin for catalog pages, as the standard theme functionality did not support it. Supervised the full cycle: requirements → development → debugging → deployment.
- **1C integration and logistics:** worked closely with a 1C engineer. Initiated creation of a separate supplier-side 1C test database (sole proprietor) for realistic shipping and vendor flow testing, without mixing data with the primary accounting database (LLC). Oversaw logistics exchange setup with CDEK and Russian Post.
- Drove migration from shared hosting to cloud with containerization to reduce TCO and simplify deployments.

###### 4. Documentation and operating procedures
- Created a dedicated subdomain with a lightweight wiki engine to centralize project knowledge. Approved documentation standards for feature updates, infrastructure, and store business processes.
- Developed detailed procedures and instructions for marketers and content managers.

###### 5. Requirements and quality control
- Prepared technical requirements for key streams: SMS registration, AJAX catalog, checkout, 1C exchange, UX/UI module, SEO.
- Managed phased acceptance, code review, dev environment testing, and repository delivery.
- Defined end-to-end test scenarios: registration → order → payment → shipping → status update in 1C.
- Executed multiple backup-update-debug-restore cycles during platform upgrades.

###### 6. Vendor and service operations
- Handled communication and procurement: commercial theme, search modules, SEO, UX/UI, AI-generated descriptions, 1C exchange.
- Managed invoices, payments, documentation, and licensing through the full cycle.
- Coordinated with support teams across hosting, CMS, theme vendor, CDEK, sms.ru, Cloudflare, and Yandex Cloud.
- Procured office infrastructure hardware: Synology NAS, office-to-office relocation, and initial setup.

###### 7. Marketing and SEO
- Coordinated SEO agency work: requirements, Yandex.Metrica setup, access handover, recommendation tracking.
- Supported launch of Yandex.Direct campaigns: briefs, video calls, and access handover to marketing team.
- Managed content plan: blog generation via API, descriptions and images generated through n8n + ChatGPT API.
- Aligned integrated work plan and commercial proposal from marketing contractors.

###### 8. Communications
- Conducted regular on-site sync meetings with the owner and 1C specialist.
- Organized online conferences with contractors, SEO team, marketers, and module vendors.
- Managed cross-functional requirements: accounting ↔ 1C ↔ website ↔ shipping.
- Documented decisions in the corporate wiki.

###### 9. Final phase and project archival
- Brought the platform to production readiness: new theme, shipping, sales, checkout, 1C exchange.
- Per owner decision, the project was frozen again.
- Procured office NAS, deployed self-hosted Git (Forgejo), and migrated all artifacts.
- Handed over the archived project with full documentation, credentials, repositories, and restart instructions.

---

#### Responsibility areas
<div class="row">
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/computer-svgrepo-com.svg" alt="Tech Lead"><div>Tech Lead</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/forgejo-original.svg" alt="Trackers"><div>Trackers</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/gitlab-original.svg" alt="GitLab"><div>Code Review</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/grafana-original.svg" alt="Metrics"><div>Metrics</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/linux-original.svg" alt="Infra"><div>Infrastructure</div></div>
<div class="col-4 col-lg-2 pt-2" style="text-align: center;"><img src="/icons/sun-svgrepo-com.svg" alt="People"><div>Team</div></div>
</div>

---

#### Results
✅ **Team:** built an execution setup with 6+ contractors across responsibility areas  
✅ **Platform:** transitioned from a modified core to a clean CMS + commercial theme, reducing maintenance costs  
✅ **Launch:** delivered a production marketplace with sales, checkout, shipping, and 1C data exchange  
✅ **Documentation:** corporate wiki, repositories, password file, and operational instructions  
✅ **Budget:** optimization through module procurement instead of building everything from scratch  
✅ **Handover:** project archived with full documentation and access for future restart  

---

#### Project map
{{< mermaid align="center" >}}
graph LR
    OWN[Owner] <--> PM[Tech Lead / PM]
    PM <--> DEV[CS-Cart Developers]
    PM <--> ODIN[1C Developer]
    PM <--> SEO[SEO Agency]
    PM <--> MKT[Marketers]
    PM <--> UX[UX/UI Contractor]
    PM <--> DOC[Technical Writer]
    DEV <--> REPO[Git Repositories]
    ODIN <--> EXCH[1C ↔ Website Exchange]
    SEO <--> METR[Yandex.Metrica]
    MKT <--> ADS[Yandex.Direct]
    UX <--> THEME[Theme and Modules]
    DOC <--> WIKI[Wiki docs.*]
{{< /mermaid >}}

---

#### Duration
>Around 12 months as Tech Lead / Project Manager</mark>

> 200+ hours of management activities</mark>
