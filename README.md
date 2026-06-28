# ☁️ Azure Pricing Calculator Lab

> **Hands-on cost analysis using the Azure Pricing Calculator**
> *Hands-on Kostenanalyse mit dem Azure Pricing Calculator*

![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)

---

## 📌 About This Lab

This lab explores **Azure cloud pricing models** through 6 hands-on exercises using the [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator).  
Each task focuses on a real-world scenario — from single VM pricing to identifying costly misconfigurations.

> 🇩🇪 Die Aufgaben stammen aus einem deutschen Kursmodul.  
> 🇬🇧 All answers and analysis are documented in English.

📄 **[View Original Task Document (PDF)](docs/azure-cost-lab-tasks.pdf)**

---

## 🗂️ Table of Contents

| # | Task | Topic | Difficulty |
|---|------|--------|------------|
| [A](#-aufgabe-a--eine-einzelne-vm-kalkulieren) | Aufgabe A | Single VM Pricing | 🟢 Easy |
| [B](#-aufgabe-b--lizenzvergleich-hybrid-benefit-vs-ohne) | Aufgabe B | Hybrid Benefit vs. Pay-as-you-go | 🟢 Easy |
| [C](#-aufgabe-c--regionsvergleich) | Aufgabe C | Region Price Comparison | 🟢 Easy |
| [D](#-aufgabe-d--vm--storage-account) | Aufgabe D | VM + Storage Account | 🟡 Medium |
| [E](#-aufgabe-e--vollständiges-kleines-szenario) | Aufgabe E | Full Team Environment | 🟡 Medium |
| [F](#-aufgabe-f--kostenfalle-finden) | Aufgabe F | Find the Cost Traps | 🟠 Medium+ |

---

## 💰 Cost Summary — All Tasks at a Glance

| Task | Configuration | Monthly Cost |
|------|--------------|-------------|
| **A** | B2s · Windows · 730h · 128 GB HDD · West Europe | **$46.77** |
| **A** | B2s · Windows · 160h · 128 GB HDD · West Europe | **$14.85** |
| **B** | D2s v3 · Pay-as-you-go · West Europe | **$154.76** |
| **B** | D2s v3 · Hybrid Benefit · West Europe | **$87.60** |
| **C** | B2ms · Linux · East US *(cheapest)* | **$60.74** |
| **C** | B2ms · Linux · Brazil South *(most expensive)* | **$97.82** |
| **D** | B2s + 100 GB Blob LRS · West Europe | **$52.52** |
| **E** | 2× VMs + Storage + SQL DB · North Europe | **$110.94** |
| **F** | Original (bad config) · Brazil South | **$1,107.65** |
| **F** | Optimized · West Europe | **$44.16** |

---

## 🅐 Aufgabe A — Eine einzelne VM kalkulieren
> *Calculate the cost of a single Windows test server*

**Config:** `B2s` · `Windows` · `West Europe` · `128 GB Standard HDD` · `Pay-as-you-go`

| Scenario | Hours | Monthly Cost |
|----------|-------|-------------|
| 24/7 full month | 730h | **$46.77** |
| Weekdays only (Mo–Fr, 8h/day) | 160h | **$14.85** |
| 💾 Disk only (S10) | — | **≈ $5.28** |

> 💡 **Key insight:** Reducing runtime from 730h → 160h saves ~68% on compute costs. Disk cost remains unchanged regardless of VM state.

📊 **[View Estimate Screenshot](tasks/A-single-vm/screenshots/task-a-results.png)**

<details>
<summary>🖼️ Screenshot</summary>

![Task A – Single VM Pricing: 730h vs 160h runtime comparison in West Europe](tasks/A-single-vm/screenshots/task-a-results.png)

</details>

---

## 🅑 Aufgabe B — Lizenzvergleich: Hybrid Benefit vs. ohne
> *License comparison: Azure Hybrid Benefit vs. Pay-as-you-go*

**Config:** `D2s v3` · `Windows` · `West Europe` · `730h`

| Option | Monthly | Annual |
|--------|---------|--------|
| Pay-as-you-go (License included) | $154.76 | $1,857.12 |
| Azure Hybrid Benefit (AHB) | $87.60 | $1,051.20 |
| 💚 **Saving** | **$67.16 / mo** | **$805.92 / yr** |

> 💡 **Key insight:** AHB saves **~43%** on the Windows license fee. Best for organizations with existing Windows Server SA licenses on long-running production VMs.

📊 **[Download Answers (Excel)](tasks/B-hybrid-benefit/exports/task-b-answers.xlsx)**

<details>
<summary>🖼️ Screenshot</summary>

![Task B – Hybrid Benefit vs Pay-as-you-go: $67.16/month savings on D2s v3 Windows VM](tasks/B-hybrid-benefit/screenshots/task-b-results.png)

</details>

---

## 🅒 Aufgabe C — Regionsvergleich
> *Azure pricing varies by region — find the difference across 3 regions*

**Config:** `B2ms` · `Linux` · `730h` · No managed disk

| Region | Monthly Cost | vs. West Europe |
|--------|-------------|----------------|
| 🟢 **East US** *(cheapest)* | $60.74 | −13.3% |
| 🟡 **West Europe** | $70.08 | — |
| 🔴 **Brazil South** *(most expensive)* | $97.82 | **+39.6%** |

> 💡 **Key insight:** Brazil South is ~40% more expensive than West Europe due to local taxes and limited infrastructure. East US benefits from Microsoft's largest data center footprint.

📊 **[Download Answers (Excel)](tasks/C-region-comparison/exports/task-c-answers.xlsx)**

<details>
<summary>🖼️ Screenshot</summary>

![Task C – Region Comparison: East US cheapest at $60.74, Brazil South most expensive at $97.82](tasks/C-region-comparison/screenshots/task-c-results.png)

</details>

---

## 🅓 Aufgabe D — VM + Storage Account
> *Small web app: VM with Blob Storage cost breakdown + redundancy comparison*

**Config:** `B2s` · `Windows` · `730h` · `128 GB Standard SSD` + `Blob LRS 100 GB` · `West Europe`

| Component | Monthly | Annual |
|-----------|---------|--------|
| Virtual Machine (B2s + E10 SSD) | $50.48 | $605.76 |
| Storage Account (LRS) | $2.04 | $24.48 |
| **Total (LRS)** | **$52.52** | **$630.24** |
| Total (GRS) | $54.53 | $654.36 |
| GRS extra cost | +$2.01/mo | +$24.12/yr |

> 💡 **Key insight:** Switching from LRS → GRS doubles capacity cost AND write operation cost (every write is replicated to a secondary region).

📊 **[Download Answers (Excel)](tasks/D-vm-storage/exports/task-d-answers.xlsx)**

<details>
<summary>🖼️ Screenshot</summary>

![Task D – VM plus Storage Account: $52.52/month total, LRS vs GRS comparison showing $2.01 difference](tasks/D-vm-storage/screenshots/task-d-results.png)

</details>

---

## 🅔 Aufgabe E — Vollständiges kleines Szenario
> *Full productivity environment for a team of 10 — multi-service cost breakdown*

**Config:** `North Europe` · 2× VMs · Storage · Azure SQL Database

| Component | Config | Monthly | % of Total |
|-----------|--------|---------|-----------|
| VM1 | D2s v3 · AHB · 730h · 256 GB SSD | $97.31 | 87.7% |
| VM2 | B1ms · PAYG · 160h | $4.19 | 3.8% |
| Storage | LRS · 200 GB | $4.54 | 4.1% |
| SQL DB | Basic · 5 DTU · 2 GB | $4.90 | 4.4% |
| **Total (PAYG)** | | **$110.94** | |
| **Total (VM1 Reserved 1yr)** | | **$84.00** | |

💚 **Switching VM1 to 1-Year Reserved saves $323.28/year (24.3%)**

> 💡 **Key insight:** VM compute accounts for 87.7% of the total bill. Targeting the biggest cost driver with a reservation delivers maximum ROI.

📊 **[Download Answers (Excel)](tasks/E-full-scenario/exports/task-e-answers.xlsx)**

<details>
<summary>🖼️ Screenshot</summary>

![Task E – Complete Team Environment: $110.94/month total across 4 Azure services, VM1 dominates at 87.7%](tasks/E-full-scenario/screenshots/task-e-results.png)

</details>

---

## 🅕 Aufgabe F — Kostenfalle finden
> *Spot the waste: find cost traps and build a cheaper alternative*

### 🔴 Original (Problematic) Configuration

| Parameter | Value | Problem |
|-----------|-------|---------|
| Region | Brazil South | 🔴 Most expensive region |
| VM | D8s v3 · 8 vCPUs | 🔴 Massively oversized |
| Runtime | 730h (24/7) | 🔴 App runs only Mo–Fr 09–18 |
| License | Pay-as-you-go | 🔴 No Hybrid Benefit |
| Disk | 1 TB Premium SSD | 🔴 20× too large |
| Storage | GRS + 1000 GB transfer | 🔴 $141/mo for 50 GB |

### 🟢 Optimized Configuration

| Parameter | Change | Saving |
|-----------|--------|--------|
| Region | West Europe | ✅ |
| VM | D4s v3 · 4 vCPUs | ✅ |
| Runtime | 180h (scheduled) | ✅ |
| License | Azure Hybrid Benefit | ✅ |
| Disk | 128 GB Standard SSD | ✅ |
| Storage | LRS · 50 GB | ✅ |

### 💰 Result

| | Monthly | Annual |
|--|---------|--------|
| 🔴 Original | $1,107.65 | $13,291.80 |
| 🟢 Optimized | $44.16 | $529.92 |
| 💚 **Saving** | **$1,063.49** | **$12,761.88** |

## 🏆 96% cheaper — by fixing 6 cost traps

📊 **[Download Answers (Excel)](tasks/F-cost-traps/exports/task-f-answers.xlsx)**

<details>
<summary>🖼️ Screenshot 1 — Original vs. Optimized Exports</summary>

![Task F – Original config at $1,107.65/month in Brazil South vs optimized at $44.16/month in West Europe](tasks/F-cost-traps/screenshots/task-f-original-config.png)

</details>

<details>
<summary>🖼️ Screenshot 2 — Cost Trap Analysis</summary>

![Task F – Cost trap analysis showing 6 identified waste areas and 96% cost reduction achieved](tasks/F-cost-traps/screenshots/task-f-cost-trap-analysis.png)

</details>

---

## 🧠 Key Takeaways

| # | Lesson |
|---|--------|
| 1 | 🌍 **Region matters** — Brazil South can be 40%+ more expensive than East US |
| 2 | 🪟 **Hybrid Benefit** saves ~43% on Windows licensing for eligible orgs |
| 3 | ⏱️ **Runtime optimization** — shutting down idle VMs is the fastest win |
| 4 | 📦 **Right-size everything** — oversized VMs and disks are silent budget killers |
| 5 | 💾 **LRS vs GRS** — geo-redundancy doubles storage cost AND write costs |
| 6 | 📅 **Reserved Instances** — 1-year commitment saves ~34% on stable workloads |
| 7 | 🔍 **One bad config** can cost 25× more than an optimized equivalent |

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| [Azure Pricing Calculator](https://azure.microsoft.com/pricing/calculator) | Cost estimation |
| Microsoft Excel | Result documentation & analysis |
| GitHub | Lab documentation |

---

## 📁 Repository Structure

```
azure-pricing-lab/
├── 📄 README.md
├── 📁 docs/
│   └── azure-cost-lab-tasks.pdf        ← Original task document
└── 📁 tasks/
    ├── A-single-vm/
    │   ├── exports/                     ← (manual estimates)
    │   └── screenshots/
    ├── B-hybrid-benefit/
    │   ├── exports/task-b-answers.xlsx
    │   └── screenshots/
    ├── C-region-comparison/
    │   ├── exports/task-c-answers.xlsx
    │   └── screenshots/
    ├── D-vm-storage/
    │   ├── exports/task-d-answers.xlsx
    │   └── screenshots/
    ├── E-full-scenario/
    │   ├── exports/task-e-answers.xlsx
    │   └── screenshots/
    └── F-cost-traps/
        ├── exports/task-f-answers.xlsx
        └── screenshots/
```

---

> *All prices shown are in USD and reflect Azure Pricing Calculator values at the time of completion (June 2026). Prices may vary.*  
> *Alle Preise in USD, Stand: Juni 2026. Preisänderungen vorbehalten.*
