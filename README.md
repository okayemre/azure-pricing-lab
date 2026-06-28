# ☁️ Azure Pricing Calculator Lab

![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Calculator](https://img.shields.io/badge/Tool-Pricing_Calculator-FF6B35?style=for-the-badge)

---

> **[Deutsch](#deutsch)** &nbsp;·&nbsp; **[English](#english)**

---

<br>
<br>

# Deutsch

Hands-on Kostenanalyse mit dem [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator).  
6 Szenarien — von der einfachen VM bis zur Kostenfallen-Jagd.

📄 **[Aufgabendokument öffnen (PDF)](docs/azure-cost-lab-tasks.pdf)**

---

## 📋 Inhaltsverzeichnis

| | Aufgabe | Thema | Niveau |
|--|---------|-------|--------|
| 🔵 | [A — Einzelne VM](#a--einzelne-vm-kalkulieren) | VM-Kosten berechnen | 🟢 Leicht |
| 🔵 | [B — Lizenzvergleich](#b--lizenzvergleich) | Hybrid Benefit vs. Pay-as-you-go | 🟢 Leicht |
| 🔵 | [C — Regionsvergleich](#c--regionsvergleich) | Preisunterschiede je Region | 🟢 Leicht |
| 🔵 | [D — VM + Storage](#d--vm--storage-account) | Kombination aus VM und Blob Storage | 🟡 Mittel |
| 🔵 | [E — Team-Szenario](#e--vollständiges-team-szenario) | Vollständige Produktivumgebung | 🟡 Mittel |
| 🔵 | [F — Kostenfallen](#f--kostenfallen-finden) | Fehler finden & Kosten senken | 🟠 Mittel+ |

---

## 💰 Kostenübersicht auf einen Blick

| Aufgabe | Konfiguration | Monatlich |
|---------|--------------|-----------|
| **A** | B2s · Windows · 730h · West Europe | **$46.77** |
| **A** | B2s · Windows · 160h (nur Werktage) | **$14.85** |
| **B** | D2s v3 · Pay-as-you-go | **$154.76** |
| **B** | D2s v3 · Azure Hybrid Benefit | **$87.60** |
| **C** | B2ms · Linux · East US 🏆 günstigste | **$60.74** |
| **C** | B2ms · Linux · Brazil South 💸 teuerste | **$97.82** |
| **D** | B2s + Blob LRS 100 GB · West Europe | **$52.52** |
| **E** | 2× VMs + Storage + SQL · North Europe | **$110.94** |
| **F** | ❌ Original — schlechte Konfiguration | **$1,107.65** |
| **F** | ✅ Optimiert | **$44.16** |

---

## A — Einzelne VM kalkulieren

> 💡 Wie viel kostet ein Windows-Testserver — und was passiert, wenn man ihn nur werktags laufen lässt?

**Konfiguration:** `B2s` · `Windows` · `West Europe` · `128 GB Standard HDD` · `Pay-as-you-go`

| Szenario | Stunden | Monatlich | Jährlich |
|----------|---------|-----------|----------|
| 🕐 24/7 ganzer Monat | 730h | **$46.77** | $561.24 |
| 📅 Nur Werktage (Mo–Fr, 8h/Tag) | 160h | **$14.85** | $178.20 |
| 💾 Nur Disk (S10, ohne VM) | — | **≈ $5.28** | $63.36 |

> 🔑 **Erkenntnis:** Laufzeit von 730h → 160h reduzieren spart **~68%** auf Compute-Kosten.
> Die Disk-Kosten laufen weiter — egal ob die VM an oder aus ist!

![Task A – Kostenvergleich: $46.77 bei 730h Vollbetrieb vs $14.85 bei 160h Werktage, West Europe, B2s Windows VM](tasks/A-single-vm/screenshots/task-a-results.png)

*Zwei Exports nebeneinander: 730h (Vollbetrieb) vs 160h (nur Werktage)*

---

## B — Lizenzvergleich

> 💡 Gleiche VM, zwei Lizenzmodelle — Azure Hybrid Benefit kann Hunderte von Euro pro Jahr sparen.

**Konfiguration:** `D2s v3` · `Windows` · `West Europe` · `730h`

| Lizenzmodell | Monatlich | Jährlich |
|-------------|-----------|----------|
| ❌ Pay-as-you-go (Lizenz inklusive) | $154.76 | $1,857.12 |
| ✅ Azure Hybrid Benefit | $87.60 | $1,051.20 |
| 💚 **Ersparnis** | **$67.16 / Monat** | **$805.92 / Jahr** |

> 🔑 **Erkenntnis:** Azure Hybrid Benefit spart **~43%** bei der Windows-Lizenz.
> Lohnt sich für Unternehmen mit bestehenden Windows Server Software Assurance Lizenzen.

![Task B – Hybrid Benefit vs Pay-as-you-go Lizenzvergleich: $154.76 vs $87.60 monatlich, D2s v3 Windows VM, West Europe](tasks/B-hybrid-benefit/screenshots/task-b-results.png)

*Pay-as-you-go vs Azure Hybrid Benefit — $67.16 Ersparnis pro Monat*

---

## C — Regionsvergleich

> 💡 Azure-Preise sind nicht überall gleich — manchmal macht die Wahl der Region den größten Unterschied.

**Konfiguration:** `B2ms` · `Linux` · `730h` · Kein Managed Disk

| Region | Monatlich | vs. West Europe | vs. East US |
|--------|-----------|----------------|-------------|
| 🏆 **East US** | $60.74 | −13.3% | — günstigste |
| **West Europe** | $70.08 | — | +15.4% teurer |
| 💸 **Brazil South** | $97.82 | **+39.6% teurer** | +61.1% teurer |

> 🔑 **Erkenntnis:** Brazil South ist ~40% teurer als West Europe — wegen lokaler Steuern und begrenzter Infrastrukturkapazität.
> East US ist günstig, weil dort das größte Microsoft-Rechenzentrum steht.

![Task C – Regionsvergleich: East US günstigste Region $60.74, West Europe $70.08, Brazil South teuerste $97.82, B2ms Linux VM](tasks/C-region-comparison/screenshots/task-c-results.png)

*3 Regionen, gleiche VM — bis zu 39.6% Preisunterschied*

---

## D — VM + Storage Account

> 💡 Eine kleine Web-App: VM mit Blob Storage kombinieren — und LRS vs. GRS vergleichen.

**Konfiguration:** `B2s` · `Windows` · `730h` · `128 GB Standard SSD (E10)` + `Blob 100 GB` · `West Europe`

| Komponente | Monatlich | Jährlich |
|------------|-----------|----------|
| 🖥️ VM (B2s + E10 SSD) | $50.48 | $605.76 |
| 💾 Storage (Blob LRS) | $2.04 | $24.48 |
| **Gesamt (LRS)** | **$52.52** | **$630.24** |

| Redundanz | Kopien | Monatlich | Mehrkosten |
|-----------|--------|-----------|------------|
| LRS (lokal) | 3 Kopien · 1 Rechenzentrum | $52.52 | — |
| GRS (geografisch) | 6 Kopien · 2 Regionen | $54.53 | +$2.01 / Monat |

> 🔑 **Erkenntnis:** GRS verdoppelt nicht nur die Kapazitätskosten — auch die Schreibkosten verdoppeln sich,
> da jeder Write in die sekundäre Region repliziert wird ($0.054 → $0.108 pro 10K Operationen).

![Task D – VM plus Blob Storage Kombination: $52.52 Gesamt mit LRS, $54.53 mit GRS, West Europe, B2s Windows VM](tasks/D-vm-storage/screenshots/task-d-results.png)

*VM + Blob Storage — LRS vs. GRS Kostenaufschlüsselung*

---

## E — Vollständiges Team-Szenario

> 💡 Eine echte Produktivumgebung für 10 Personen: 2 VMs, Storage und SQL-Datenbank — welche Komponente dominiert die Kosten?

**Konfiguration:** `North Europe` · D2s v3 (AHB) · B1ms · LRS 200 GB · SQL Basic

| Komponente | Monatlich | % der Gesamtkosten | Jährlich |
|------------|-----------|-------------------|----------|
| 🔴 VM1 — D2s v3 (AHB, 730h, 256 GB SSD) | $97.31 | **87.7%** | $1,167.72 |
| VM2 — B1ms (PAYG, nur 160h) | $4.19 | 3.8% | $50.28 |
| Storage — LRS 200 GB | $4.54 | 4.1% | $54.48 |
| SQL DB — Basic 5 DTU | $4.90 | 4.4% | $58.80 |
| **Gesamt (Pay-as-you-go)** | **$110.94** | 100% | **$1,331.28** |
| **Gesamt (VM1 Reserved 1 Jahr)** | **$84.00** | — | **$1,007.99** |
| 💚 Ersparnis durch Reservierung | $26.94 | — | **$323.28 (24.3%)** |

> 🔑 **Erkenntnis:** VM1 allein macht **87.7%** der Gesamtrechnung aus.
> Nur VM1 auf Reserved (1 Jahr) umstellen → **$323 jährliche Ersparnis** bei nur einer Änderung!

![Task E – Vollständiges Team-Szenario: $110.94 pro Monat über 4 Azure-Dienste, VM1 dominiert mit 87.7% der Gesamtkosten, North Europe](tasks/E-full-scenario/screenshots/task-e-results.png)

*4 Dienste, VM1 = 87.7% der Gesamtkosten — Reserved Instance spart $323.28/Jahr*

---

## F — Kostenfallen finden

> 💡 Jemand hat eine Umgebung gebaut — mit 6 teuren Fehlern. Alles gefunden, alles gefixt.

**❌ Originalkonfiguration — 6 Kostenfallen:**

| # | Kostenfalle | Original | Problem |
|---|------------|----------|---------|
| 🔴 1 | Region | Brazil South | Teuerste Region — ohne Notwendigkeit |
| 🔴 2 | VM-Größe | D8s v3 (8 vCPU) | App läuft nur Mo–Fr 09–18 — massiv überdimensioniert |
| 🔴 3 | Laufzeit | 730h (24/7) | VM läuft 550 Stunden unnötig pro Monat |
| 🔴 4 | Lizenz | Pay-as-you-go | Kein Hybrid Benefit — doppelt für Lizenz zahlen |
| 🔴 5 | Disk | 1 TB Premium SSD | 20× zu groß für 50 GB Bedarf |
| 🔴 6 | Storage | GRS + 1000 GB Geo-Transfer | $141/Monat nur für 50 GB Storage! |

| Parameter | ❌ Original | ✅ Optimiert |
|-----------|-----------|------------|
| Region | Brazil South | West Europe |
| VM | D8s v3 · 8 vCPU | D4s v3 · 4 vCPU |
| Laufzeit | 730h (24/7) | 180h (Mo–Fr 09–18) |
| Lizenz | Pay-as-you-go | Azure Hybrid Benefit |
| Disk | 1 TB Premium SSD | 128 GB Standard SSD |
| Storage | GRS + 1000 GB Transfer | LRS · 50 GB |

| | Monatlich | Jährlich |
|--|-----------|----------|
| ❌ Original | $1,107.65 | $13,291.80 |
| ✅ Optimiert | $44.16 | $529.92 |
| 💚 **Ersparnis** | **$1,063.49** | **$12,761.88** |

# 🏆 96% günstiger — durch 6 einfache Fixes!

> 🔑 **Erkenntnis:** Eine schlechte Konfiguration kann **25× teurer** sein als eine optimierte.
> Region + VM-Größe + Laufzeit sind die drei größten Hebel.

![Task F – Original Konfiguration $1,107.65 pro Monat in Brazil South vs optimierte Version $44.16 pro Monat in West Europe](tasks/F-cost-traps/screenshots/task-f-original-config.png)

*Original vs. Optimiert — Exporte aus dem Pricing Calculator*

![Task F – Kostenfallen Analyse: 6 identifizierte Probleme und 96% Kostenreduktion durch Optimierung der Azure Konfiguration](tasks/F-cost-traps/screenshots/task-f-cost-trap-analysis.png)

*Alle 6 Kostenfallen identifiziert und behoben — 96% Einsparung*

---

## 🧠 Die wichtigsten Lektionen

| # | Lektion | Warum wichtig? |
|---|---------|----------------|
| 1 | 🌍 Region ist entscheidend | Bis zu 40% Preisunterschied — immer vergleichen |
| 2 | 🪟 Hybrid Benefit nutzen | ~43% Ersparnis bei Windows-Lizenzen — oft vergessen |
| 3 | ⏱️ VM-Laufzeit optimieren | Idle VMs stoppen = sofortige Einsparung |
| 4 | 📦 Richtig dimensionieren | Überdimensionierte VMs & Disks fressen Budget |
| 5 | 💾 LRS vs. GRS abwägen | GRS verdoppelt Kosten — nur wo nötig einsetzen |
| 6 | 📅 Reserved Instances | 1-Jahr-Bindung → ~34% Ersparnis bei stabilen Workloads |
| 7 | 🔍 Alles hinterfragen | Eine schlechte Config kann 25× teurer sein |

---

<br>
<br>
<br>

# English

Hands-on cost analysis using the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator).  
6 scenarios — from a single VM all the way to hunting down costly misconfigurations.

📄 **[Open Task Document (PDF)](docs/azure-cost-lab-tasks.pdf)**

---

## 📋 Table of Contents

| | Task | Topic | Difficulty |
|--|------|--------|------------|
| 🔵 | [A — Single VM](#a--single-vm-pricing) | Calculate VM costs | 🟢 Easy |
| 🔵 | [B — License Comparison](#b--license-comparison) | Hybrid Benefit vs. Pay-as-you-go | 🟢 Easy |
| 🔵 | [C — Region Comparison](#c--region-comparison) | Price differences per region | 🟢 Easy |
| 🔵 | [D — VM + Storage](#d--vm--storage-account-1) | VM combined with Blob Storage | 🟡 Medium |
| 🔵 | [E — Team Scenario](#e--full-team-scenario) | Complete productivity environment | 🟡 Medium |
| 🔵 | [F — Cost Traps](#f--find-the-cost-traps) | Find mistakes & cut costs | 🟠 Medium+ |

---

## 💰 Cost Summary at a Glance

| Task | Configuration | Monthly |
|------|--------------|---------|
| **A** | B2s · Windows · 730h · West Europe | **$46.77** |
| **A** | B2s · Windows · 160h (weekdays only) | **$14.85** |
| **B** | D2s v3 · Pay-as-you-go | **$154.76** |
| **B** | D2s v3 · Azure Hybrid Benefit | **$87.60** |
| **C** | B2ms · Linux · East US 🏆 cheapest | **$60.74** |
| **C** | B2ms · Linux · Brazil South 💸 most expensive | **$97.82** |
| **D** | B2s + Blob LRS 100 GB · West Europe | **$52.52** |
| **E** | 2× VMs + Storage + SQL · North Europe | **$110.94** |
| **F** | ❌ Original — bad configuration | **$1,107.65** |
| **F** | ✅ Optimized | **$44.16** |

---

## A — Single VM Pricing

> 💡 How much does a Windows test server cost — and what happens when you only run it on weekdays?

**Config:** `B2s` · `Windows` · `West Europe` · `128 GB Standard HDD` · `Pay-as-you-go`

| Scenario | Hours | Monthly | Annual |
|----------|-------|---------|--------|
| 🕐 24/7 full month | 730h | **$46.77** | $561.24 |
| 📅 Weekdays only (Mo–Fr, 8h/day) | 160h | **$14.85** | $178.20 |
| 💾 Disk only (S10, no VM) | — | **≈ $5.28** | $63.36 |

> 🔑 **Key insight:** Reducing runtime from 730h → 160h saves **~68%** on compute costs.
> However, disk costs keep running regardless of whether the VM is on or off!

![Task A – Cost comparison: $46.77 at 730h full runtime vs $14.85 at 160h weekdays only, West Europe, B2s Windows VM](tasks/A-single-vm/screenshots/task-a-results.png)

*Two exports side by side: 730h (full month) vs 160h (weekdays only)*

---

## B — License Comparison

> 💡 Same VM, two licensing models — Azure Hybrid Benefit can save hundreds of dollars per year.

**Config:** `D2s v3` · `Windows` · `West Europe` · `730h`

| License Model | Monthly | Annual |
|--------------|---------|--------|
| ❌ Pay-as-you-go (License included) | $154.76 | $1,857.12 |
| ✅ Azure Hybrid Benefit | $87.60 | $1,051.20 |
| 💚 **Saving** | **$67.16 / month** | **$805.92 / year** |

> 🔑 **Key insight:** Azure Hybrid Benefit saves **~43%** on the Windows license.
> Best for organizations with existing Windows Server Software Assurance licenses.

![Task B – Hybrid Benefit vs Pay-as-you-go license comparison: $154.76 vs $87.60 monthly, D2s v3 Windows VM, West Europe](tasks/B-hybrid-benefit/screenshots/task-b-results.png)

*Pay-as-you-go vs Azure Hybrid Benefit — $67.16/month difference*

---

## C — Region Comparison

> 💡 Azure prices are not the same everywhere — sometimes the choice of region makes the biggest difference.

**Config:** `B2ms` · `Linux` · `730h` · No Managed Disk

| Region | Monthly | vs. West Europe | vs. East US |
|--------|---------|----------------|-------------|
| 🏆 **East US** | $60.74 | −13.3% | — cheapest |
| **West Europe** | $70.08 | — | +15.4% more |
| 💸 **Brazil South** | $97.82 | **+39.6% more** | +61.1% more |

> 🔑 **Key insight:** Brazil South is ~40% more expensive than West Europe — due to local taxes and limited infrastructure capacity.
> East US is cheap because Microsoft's largest data center is located there.

![Task C – Region comparison: East US cheapest $60.74, West Europe $70.08, Brazil South most expensive $97.82, B2ms Linux VM](tasks/C-region-comparison/screenshots/task-c-results.png)

*3 regions, same VM — up to 39.6% price difference*

---

## D — VM + Storage Account

> 💡 A small web app: combine a VM with Blob Storage — and compare LRS vs. GRS redundancy.

**Config:** `B2s` · `Windows` · `730h` · `128 GB Standard SSD (E10)` + `Blob 100 GB` · `West Europe`

| Component | Monthly | Annual |
|-----------|---------|--------|
| 🖥️ VM (B2s + E10 SSD) | $50.48 | $605.76 |
| 💾 Storage (Blob LRS) | $2.04 | $24.48 |
| **Total (LRS)** | **$52.52** | **$630.24** |

| Redundancy | Copies | Monthly | Extra Cost |
|------------|--------|---------|------------|
| LRS (local) | 3 copies · 1 data center | $52.52 | — |
| GRS (geo) | 6 copies · 2 regions | $54.53 | +$2.01 / month |

> 🔑 **Key insight:** GRS doesn't just double capacity costs — write operation costs double too,
> because every write gets replicated to the secondary region ($0.054 → $0.108 per 10K ops).

![Task D – VM plus Blob Storage combination: $52.52 total with LRS, $54.53 with GRS, West Europe, B2s Windows VM](tasks/D-vm-storage/screenshots/task-d-results.png)

*VM + Blob Storage — LRS vs. GRS cost breakdown*

---

## E — Full Team Scenario

> 💡 A real productivity environment for 10 people: 2 VMs, Storage and SQL Database — which component dominates the costs?

**Config:** `North Europe` · D2s v3 (AHB) · B1ms · LRS 200 GB · SQL Basic

| Component | Monthly | % of Total | Annual |
|-----------|---------|-----------|--------|
| 🔴 VM1 — D2s v3 (AHB, 730h, 256 GB SSD) | $97.31 | **87.7%** | $1,167.72 |
| VM2 — B1ms (PAYG, 160h only) | $4.19 | 3.8% | $50.28 |
| Storage — LRS 200 GB | $4.54 | 4.1% | $54.48 |
| SQL DB — Basic 5 DTU | $4.90 | 4.4% | $58.80 |
| **Total (Pay-as-you-go)** | **$110.94** | 100% | **$1,331.28** |
| **Total (VM1 Reserved 1 Year)** | **$84.00** | — | **$1,007.99** |
| 💚 Saving from reservation | $26.94 | — | **$323.28 (24.3%)** |

> 🔑 **Key insight:** VM1 alone makes up **87.7%** of the total bill.
> Switch only VM1 to Reserved (1 Year) → **$323 annual saving** with just one change!

![Task E – Full team scenario: $110.94 per month across 4 Azure services, VM1 dominates at 87.7% of total cost, North Europe](tasks/E-full-scenario/screenshots/task-e-results.png)

*4 services, VM1 = 87.7% of total cost — Reserved Instance saves $323.28/year*

---

## F — Find the Cost Traps

> 💡 Someone built an environment — with 6 expensive mistakes. All found, all fixed.

**❌ Original Configuration — 6 Cost Traps:**

| # | Cost Trap | Original | Problem |
|---|-----------|----------|---------|
| 🔴 1 | Region | Brazil South | Most expensive region — for no reason |
| 🔴 2 | VM Size | D8s v3 (8 vCPU) | App runs Mo–Fr 09–18 only — massively oversized |
| 🔴 3 | Runtime | 730h (24/7) | VM runs 550 unnecessary hours per month |
| 🔴 4 | License | Pay-as-you-go | No Hybrid Benefit — paying double for the license |
| 🔴 5 | Disk | 1 TB Premium SSD | 20× too large for a 50 GB need |
| 🔴 6 | Storage | GRS + 1000 GB Geo-Transfer | $141/month just for 50 GB of storage! |

| Parameter | ❌ Original | ✅ Optimized |
|-----------|-----------|------------|
| Region | Brazil South | West Europe |
| VM | D8s v3 · 8 vCPU | D4s v3 · 4 vCPU |
| Runtime | 730h (24/7) | 180h (Mo–Fr 09–18) |
| License | Pay-as-you-go | Azure Hybrid Benefit |
| Disk | 1 TB Premium SSD | 128 GB Standard SSD |
| Storage | GRS + 1000 GB Transfer | LRS · 50 GB |

| | Monthly | Annual |
|--|---------|--------|
| ❌ Original | $1,107.65 | $13,291.80 |
| ✅ Optimized | $44.16 | $529.92 |
| 💚 **Saving** | **$1,063.49** | **$12,761.88** |

# 🏆 96% cheaper — with 6 simple fixes!

> 🔑 **Key insight:** A bad configuration can be **25× more expensive** than an optimized one.
> Region + VM size + runtime are the three biggest levers.

![Task F – Original configuration $1,107.65 per month in Brazil South vs optimized version $44.16 per month in West Europe](tasks/F-cost-traps/screenshots/task-f-original-config.png)

*Original vs. Optimized — exports from the Pricing Calculator*

![Task F – Cost trap analysis: 6 identified problems and 96% cost reduction through Azure configuration optimization](tasks/F-cost-traps/screenshots/task-f-cost-trap-analysis.png)

*All 6 cost traps identified and fixed — 96% saving*

---

## 🧠 Key Takeaways

| # | Lesson | Why it matters |
|---|--------|----------------|
| 1 | 🌍 Region is everything | Up to 40% price difference — always compare |
| 2 | 🪟 Use Hybrid Benefit | ~43% saving on Windows licenses — often overlooked |
| 3 | ⏱️ Optimize VM runtime | Stopping idle VMs = instant saving |
| 4 | 📦 Right-size everything | Oversized VMs & disks silently drain your budget |
| 5 | 💾 Think before choosing GRS | GRS doubles costs — only use it where truly needed |
| 6 | 📅 Use Reserved Instances | 1-year commitment → ~34% saving on stable workloads |
| 7 | 🔍 Question everything | One bad config can cost 25× more |

---

## 🛠️ Tools

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

> *Prices in USD — Azure Pricing Calculator, June 2026. Prices may vary.*
> *Preise in USD — Azure Pricing Calculator, Juni 2026. Preisänderungen vorbehalten.*
