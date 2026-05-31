# Branch Digital Payment Performance Dashboard

**Project 2 — Excel + Power BI**
*Q1 2025 | 15 Branches | 3 Months Daily Data | 3 Payment Channels*

\---

## Project Overview

This project simulates and analyses digital payment performance across 15 bank branches over Q1 2025 (January–March). It covers 1.44 million+ simulated transactions across Mobile Money, Card Payment, and Bank Transfer channels, structured for direct import into Power BI.

The deliverable supports management reporting on digital payment health, specifically:

* Monitoring transaction success, failure, and delay rates per branch
* Identifying recurring system issues and underperforming branches
* Tracking channel-level trends over time
* Flagging branches that require operational intervention

\---

## File Structure

```
├── Branch\_Digital\_Payment\_Performance\_Q1\_2025.xlsx   ← Source data (this file)
├── Branch\_Payment\_Dashboard.pbix                     ← Power BI dashboard
├── Branch\_Payment\_Dashboard\_Export.pdf               ← Dashboard PDF export
└── README\_Payment\_Dashboard.md                       ← This file
```

\---

## Excel Workbook — Sheet Guide

|Sheet|Contents|
|-|-|
|**Raw Data**|4,050 rows of daily branch × channel records (the source of truth)|
|**Branch Summary**|Q1 aggregate KPIs per branch, sorted by failure rate descending|
|**Channel Breakdown**|Performance by channel + branch × channel matrix|
|**Monthly Trends**|Jan / Feb / Mar comparison per branch (volume, failures, failure rate)|
|**Weekly Trend**|13-week failure and delay rate trend across all branches|
|**Flagging Panel**|Branch status report with 🔴🟠🟡🟢 severity classification|
|**KPI Summary**|Top-level executive KPIs with targets and status indicators|

\---

## KPIs Chosen — and Why

### 1\. Transaction Volume (Total Transactions)

**Why:** The foundational count. Without knowing volume, rates are meaningless — a branch processing 50 transactions at 20% failure is far less critical than one processing 5,000 at 20% failure. Volume also exposes underutilised branches and channel adoption gaps.

### 2\. Success Rate

**Why:** The primary health signal for any payment system. It is the inverse of failure rate, and tracking both gives management a complete view. A target of ≥90% success is standard in digital banking operations; anything below 80% indicates systemic issues.

### 3\. Failure Rate

**Why:** The most actionable KPI for the operations and IT teams. High failure rates directly impact customer satisfaction, revenue leakage, and regulatory standing. It is also the primary trigger for the branch flagging system. Thresholds:

* **>20%** = 🔴 Critical (immediate escalation)
* **12–20%** = 🟠 Warning (monitoring and root cause analysis)
* **<12%** = Baseline acceptable range

### 4\. Delay Rate

**Why:** Delays (transactions that succeed but breach processing time SLAs) are often early indicators of infrastructure stress before failures begin. Tracking delay rate separately from failure rate allows teams to intervene before the situation deteriorates to outright failures. Target: <8%.

### 5\. Channel-Level Breakdown (Mobile Money vs Card vs Bank Transfer)

**Why:** Different channels have different failure profiles, infrastructure dependencies, and customer segments. Mobile Money failures may indicate network/USSD issues; Card failures may point to POS or switch problems; Bank Transfer failures often relate to interbank clearing or NIBSS/GhIPSS connectivity. Channel separation enables targeted diagnostics rather than broad system blaming.

### 6\. Branch Flagging (Recurring Issues Monitor)

**Why:** A single bad day is noise. Recurring elevated failure rates across weeks signal structural problems — understaffed IT support, poor connectivity infrastructure, hardware degradation, or high-volume branch overload. The flagging panel aggregates the entire quarter to surface chronic underperformers, not just outliers.

### 7\. Monthly and Weekly Trend Lines

**Why:** Time-series context transforms a snapshot into a story. A branch with 15% failure rate in March is very different depending on whether that rate was 8% in January (deteriorating) or 22% in January (recovering). Trend lines also make the February system outage simulation (Bolgatanga and Wa West) immediately visible.

\---

## Flagging Logic

```
Avg Failure Rate > 20%         → 🔴 Critical  (immediate management alert)
Avg Failure Rate 12% – 20%     → 🟠 Warning   (escalate to branch IT)
Avg Delay Rate > 10%           → 🟡 Watch     (monitor SLA adherence)
All metrics within target       → 🟢 Healthy
```

\---

## Data Simulation Notes

|Parameter|Details|
|-|-|
|Branches|15 (Greater Accra, Ashanti, Western, Central, Eastern, Volta, Northern, Upper East, Upper West, Bono, Bono East)|
|Date range|1 Jan 2025 – 31 Mar 2025 (90 days)|
|Channels|Mobile Money, Card Payment, Bank Transfer|
|Volume|Driven by branch tier (high/medium/low) and weekend factor (0.6×)|
|Success rates|High-tier: 92–98%, Medium: 82–93%, Low: 68–84%|
|Simulated outage|Bolgatanga \& Wa West, 10–17 Feb 2025 (volume ×0.4, success rate ×0.5–0.7)|
|Delay rate|Random 3–15% of successful transactions per day|

\---

## Power BI Dashboard — Recommended Visuals

### Page 1: Executive Overview

* KPI cards (Total Transactions, Overall Success Rate, Failure Rate, Delay Rate)
* Clustered bar chart: Failure Rate by Branch
* Donut chart: Transaction Volume by Channel

### Page 2: Branch Deep-Dive

* Slicer: Branch selector
* Line chart: Daily failure rate over time (Jan–Mar)
* Stacked column: Transactions by channel per month
* Data table: Branch × Channel breakdown

### Page 3: Failure Trend Analysis

* Line chart: Weekly failure rate trend (all branches combined)
* Area chart: Monthly volume vs failures overlay
* Scatter plot: Volume vs Failure Rate (size = Delayed)

### Page 4: Flagging Panel

* Matrix/table: Branch status with conditional formatting
* Gauge charts: Overall success rate vs 90% target
* Colour-coded status table (Critical / Warning / Watch / Healthy)

\---

## How to Connect Power BI

1. Open Power BI Desktop
2. **Get Data → Excel Workbook** → select `Branch\_Digital\_Payment\_Performance\_Q1\_2025.xlsx`
3. Load the **Raw Data** sheet as the primary fact table
4. Load **Branch Summary**, **Channel Breakdown**, **Weekly Trend**, and **Flagging Panel** as supporting tables
5. Create relationships on `Branch` and `Channel` fields
6. Build calculated measures for dynamic Success Rate, Failure Rate, and Delay Rate using DAX

### Key DAX Measures

```dax
Success Rate = DIVIDE(SUM(RawData\[Successful]), SUM(RawData\[Total\_Transactions]))

Failure Rate = DIVIDE(SUM(RawData\[Failed]), SUM(RawData\[Total\_Transactions]))

Delay Rate = DIVIDE(SUM(RawData\[Delayed]), SUM(RawData\[Total\_Transactions]))

Flagged Branches = 
CALCULATE(DISTINCTCOUNT(FlaggingPanel\[Branch]),
          FlaggingPanel\[Status] IN {"🔴 Critical","🟠 Warning"})
```

\---

## KPI Targets Summary

|KPI|Target|Rationale|
|-|-|-|
|Success Rate|≥ 90%|Industry standard for digital payment rails|
|Failure Rate|≤ 10%|Inverse of success rate target|
|Delay Rate|≤ 8%|Internal SLA for same-session settlement|
|Critical branches|0|Zero tolerance for chronic high-failure branches|

\---



