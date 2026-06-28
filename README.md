# ☁️ Azure Pricing Calculator Lab

![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)

🇬🇧 Hands-on cost analysis using the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator) — 6 real-world scenarios from single VM pricing to finding costly misconfigurations.  
🇩🇪 Kostenanalyse mit dem Azure Pricing Calculator — 6 Szenarien von der einfachen VM-Kalkulation bis zur Kostenfallen-Analyse.

📄 [Original Task Document (PDF)](docs/azure-cost-lab-tasks.pdf)

---

## 🗂️ Table of Contents

| # | Task | Topic | Difficulty |
|---|------|--------|------------|
| [A](#-aufgabe-a--eine-einzelne-vm-kalkulieren) | Aufgabe A | Single VM Pricing | 🟢 Easy |
| [B](#-aufgabe-b--lizenzvergleich) | Aufgabe B | Hybrid Benefit vs. Pay-as-you-go | 🟢 Easy |
| [C](#-aufgabe-c--regionsvergleich) | Aufgabe C | Region Price Comparison | 🟢 Easy |
| [D](#-aufgabe-d--vm--storage-account) | Aufgabe D | VM + Storage Account | 🟡 Medium |
| [E](#-aufgabe-e--vollständiges-szenario) | Aufgabe E | Full Team Environment | 🟡 Medium |
| [F](#-aufgabe-f--kostenfallen-finden) | Aufgabe F | Find the Cost Traps | 🟠 Medium+ |

---

## 💰 Cost Summary

| Task | Configuration | Monthly |
|------|--------------|---------|
| **A** | B2s · Windows · 730h · West Europe | $46.77 |
| **A** | B2s · Windows · 160h · West Europe | $14.85 |
| **B** | D2s v3 · Pay-as-you-go · West Europe | $154.76 |
| **B** | D2s v3 · Hybrid Benefit · West Europe | $87.60 |
| **C** | B2ms · Linux · East US *(cheapest)* | $60.74 |
| **C** | B2ms · Linux · Brazil South *(most expensive)* | $97.82 |
| **D** | B2s + Blob LRS 100 GB · West Europe | $52.52 |
| **E** | 2× VMs + Storage + SQL DB · North Europe | $110.94 |
| **F** | Original — bad config · Brazil South | $1,107.65 |
| **F** | Optimized · West Europe | $44.16 |

---

## 🅐 Aufgabe A — Eine einzelne VM kalkulieren

🇬🇧 Calculate the monthly cost of a Windows test server — then reduce runtime to weekdays only.  
🇩🇪 Monatliche Kosten eines Windows-Testservers berechnen — dann Laufzeit auf Werktage reduzieren.

**Config:** `B2s` · `Windows` · `West Europe` · `128 GB Standard HDD` · `Pay-as-you-go`

| Scenario | Hours | Monthly |
|----------|-------|---------|
| 24/7 full month | 730h | **$46.77** |
| Weekdays only (Mo–Fr, 8h/day) | 160h | **$14.85** |
| 💾 Disk only (S10) | — | **≈ $5.28** |

> 💡 🇬🇧 Reducing runtime from 730h → 160h saves ~68%. Disk cost stays the same whether the VM is on or off.  
> 💡 🇩🇪 Laufzeit von 730h → 160h reduzieren spart ~68%. Disk-Kosten laufen weiter, auch wenn die VM aus ist.

![Task A – Single VM cost comparison: $46.77 at 730h vs $14.85 at 160h in West Europe](tasks/A-single-vm/screenshots/task-a-results.png)
*🇬🇧 Two exports side by side: 730h (full month) vs 160h (weekdays only) · 🇩🇪 Zwei Exports: 730h (Vollbetrieb) vs 160h (nur Werktage)*

---

## 🅑 Aufgabe B — Lizenzvergleich

🇬🇧 Same VM, two licensing options — see how much Azure Hybrid Benefit saves.  
🇩🇪 Gleiche VM, zwei Lizenzmodelle — wie viel spart Azure Hybrid Benefit?

**Config:** `D2s v3` · `Windows` · `West Europe` · `730h`

| Option | Monthly | Annual |
|--------|---------|--------|
| Pay-as-you-go | $154.76 | $1,857.12 |
| Azure Hybrid Benefit | $87.60 | $1,051.20 |
| 💚 Saving | **$67.16 / mo** | **$805.92 / yr** |

> 💡 🇬🇧 AHB saves ~43% on Windows licensing. Best for orgs with existing Windows Server SA licenses.  
> 💡 🇩🇪 AHB spart ~43% bei der Windows-Lizenz. Sinnvoll für Unternehmen mit bestehenden SA-Lizenzen.

![Task B – Hybrid Benefit vs Pay-as-you-go: $154.76 vs $87.60, saving $67.16 per month on D2s v3](tasks/B-hybrid-benefit/screenshots/task-b-results.png)
*🇬🇧 Pay-as-you-go vs Azure Hybrid Benefit — $67.16/month difference · 🇩🇪 Lizenzvergleich — $67,16 Ersparnis pro Monat*

---

## 🅒 Aufgabe C — Regionsvergleich

🇬🇧 Azure prices vary by region. Compare the same Linux VM across 3 regions.  
🇩🇪 Azure-Preise unterscheiden sich je nach Region. Dieselbe Linux-VM in 3 Regionen vergleichen.

**Config:** `B2ms` · `Linux` · `730h` · No managed disk

| Region | Monthly | vs. West Europe |
|--------|---------|----------------|
| 🟢 **East US** | $60.74 | −13.3% |
| 🟡 **West Europe** | $70.08 | — |
| 🔴 **Brazil South** | $97.82 | **+39.6%** |

> 💡 🇬🇧 Brazil South is ~40% more expensive due to local taxes. East US benefits from Microsoft's largest data center footprint.  
> 💡 🇩🇪 Brazil South ist ~40% teurer wegen lokaler Steuern. East US profitiert vom größten Microsoft-Rechenzentrum.

![Task C – Region comparison: East US cheapest at $60.74, West Europe $70.08, Brazil South most expensive at $97.82](tasks/C-region-comparison/screenshots/task-c-results.png)
*🇬🇧 3 regions, same VM — up to 39.6% price difference · 🇩🇪 3 Regionen, gleiche VM — bis zu 39,6% Preisunterschied*

---

## 🅓 Aufgabe D — VM + Storage Account

🇬🇧 Build a small web app setup: VM with Blob Storage. Compare LRS vs GRS redundancy.  
🇩🇪 Kleine Web-App: VM mit Blob Storage. LRS vs GRS Redundanz vergleichen.

**Config:** `B2s` · `Windows` · `730h` · `128 GB Standard SSD` + `Blob 100 GB` · `West Europe`

| Component | Monthly |
|-----------|---------|
| VM (B2s + E10 SSD) | $50.48 |
| Storage LRS | $2.04 |
| **Total (LRS)** | **$52.52** |
| Total (GRS) | $54.53 |
| GRS extra | +$2.01/mo |

> 💡 🇬🇧 GRS doubles capacity cost AND write operation cost — every write is replicated to a second region.  
> 💡 🇩🇪 GRS verdoppelt die Kapazitätskosten UND die Schreibkosten — jeder Write wird in eine zweite Region repliziert.

![Task D – VM plus Storage: $52.52/month total with LRS, rising to $54.53 with GRS redundancy](tasks/D-vm-storage/screenshots/task-d-results.png)
*🇬🇧 VM + Blob Storage — LRS vs GRS cost breakdown · 🇩🇪 VM + Blob Storage — LRS vs GRS Kostenvergleich*

---

## 🅔 Aufgabe E — Vollständiges Szenario

🇬🇧 Full environment for a team of 10: 2 VMs, Storage, SQL Database. Find the biggest cost driver and calculate Reserved Instance savings.  
🇩🇪 Vollständige Umgebung für 10 Personen: 2 VMs, Storage, SQL-Datenbank. Größten Kostentreiber finden und Reserved Instance Ersparnis berechnen.

**Config:** `North Europe` · D2s v3 (AHB) · B1ms · LRS 200 GB · SQL Basic

| Component | Monthly | % of Total |
|-----------|---------|-----------|
| VM1 — D2s v3 (AHB) | $97.31 | 87.7% |
| VM2 — B1ms (160h) | $4.19 | 3.8% |
| Storage LRS 200 GB | $4.54 | 4.1% |
| SQL DB Basic | $4.90 | 4.4% |
| **Total (PAYG)** | **$110.94** | |
| **Total (VM1 Reserved 1yr)** | **$84.00** | |
| 💚 Annual saving | — | **$323.28 (24.3%)** |

> 💡 🇬🇧 VM compute accounts for 87.7% of the bill. Reserving just VM1 saves $323/year.  
> 💡 🇩🇪 VM-Compute macht 87,7% der Kosten aus. Nur VM1 zu reservieren spart $323 pro Jahr.

![Task E – Full team environment: $110.94/month across 4 services, VM1 dominates at 87.7% of total cost](tasks/E-full-scenario/screenshots/task-e-results.png)
*🇬🇧 4 services, VM1 = 87.7% of cost · 🇩🇪 4 Dienste, VM1 = 87,7% der Gesamtkosten*

---

## 🅕 Aufgabe F — Kostenfallen finden

🇬🇧 A poorly configured environment with 6 cost traps. Find them, fix them, measure the saving.  
🇩🇪 Eine schlecht konfigurierte Umgebung mit 6 Kostenfallen. Finden, beheben, Ersparnis messen.

| # | Trap | Original | Fixed |
|---|------|----------|-------|
| 🔴 1 | Region | Brazil South | West Europe |
| 🔴 2 | VM Size | D8s v3 (8 vCPU) | D4s v3 (4 vCPU) |
| 🔴 3 | Runtime | 730h (24/7) | 180h (Mo–Fr 09–18) |
| 🔴 4 | License | Pay-as-you-go | Hybrid Benefit |
| 🔴 5 | Disk | 1 TB Premium SSD | 128 GB Standard SSD |
| 🔴 6 | Storage | GRS + 1000 GB transfer | LRS 50 GB |

| | Monthly | Annual |
|--|---------|--------|
| 🔴 Original | $1,107.65 | $13,291.80 |
| 🟢 Optimized | $44.16 | $529.92 |
| 💚 **Saving** | **$1,063.49** | **$12,761.88** |

## 🏆 96% cheaper — 6 traps fixed

> 💡 🇬🇧 One bad config costs 25× more than an optimized equivalent.  
> 💡 🇩🇪 Eine schlechte Konfiguration kostet 25× mehr als eine optimierte.

![Task F – Original config: $1,107.65/month vs optimized: $44.16/month — both exported from Azure Pricing Calculator](tasks/F-cost-traps/screenshots/task-f-original-config.png)
*🇬🇧 Original vs Optimized — Pricing Calculator exports · 🇩🇪 Original vs Optimiert — Calculator-Exporte*

![Task F – Cost trap analysis table showing 6 identified issues and 96% cost reduction achieved](tasks/F-cost-traps/screenshots/task-f-cost-trap-analysis.png)
*🇬🇧 All 6 cost traps identified and fixed · 🇩🇪 Alle 6 Kostenfallen identifiziert und behoben*

---

## 🧠 Key Takeaways

| # | 🇬🇧 Lesson | 🇩🇪 Lektion |
|---|-----------|------------|
| 1 | 🌍 Region matters — up to 40% price difference | Region ist wichtig — bis zu 40% Preisunterschied |
| 2 | 🪟 Hybrid Benefit saves ~43% on Windows licensing | Hybrid Benefit spart ~43% bei Windows-Lizenzen |
| 3 | ⏱️ Shutting down idle VMs is the fastest win | Idle VMs stoppen ist der schnellste Spareffekt |
| 4 | 📦 Oversized VMs and disks are silent cost killers | Überdimensionierte VMs und Disks sind stille Kostenkiller |
| 5 | 💾 GRS doubles storage AND write costs | GRS verdoppelt Storage- UND Schreibkosten |
| 6 | 📅 Reserved Instances save ~34% on stable workloads | Reserved Instances sparen ~34% bei stabilen Workloads |
| 7 | 🔍 One bad config can cost 25× more | Eine schlechte Konfiguration kann 25× teurer sein |

---

## 🛠️ Tools Used

![Azure Pricing Calculator](https://img.shields.io/badge/Azure_Pricing_Calculator-0078D4?style=flat-square&logo=microsoft-azure&logoColor=white)
![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=flat-square&logo=microsoft-excel&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)

---

## 📁 Repository Structure

```
azure-pricing-lab/
├── 📄 README.md
├── 📁 docs/
│   └── azure-cost-lab-tasks.pdf
└── 📁 tasks/
    ├── A-single-vm/screenshots/
    ├── B-hybrid-benefit/screenshots/ + exports/
    ├── C-region-comparison/screenshots/ + exports/
    ├── D-vm-storage/screenshots/ + exports/
    ├── E-full-scenario/screenshots/ + exports/
    └── F-cost-traps/screenshots/ + exports/
```

---

> 🇬🇧 *Prices in USD — Azure Pricing Calculator, June 2026. Prices may vary.*  
> 🇩🇪 *Preise in USD — Azure Pricing Calculator, Juni 2026. Preisänderungen vorbehalten.*
