# SAP Business ByDesign — Automated Customer Statement Service

![SAP ByDesign](https://img.shields.io/badge/SAP-ByDesign-0FAAFF?style=flat-square&logo=sap&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Windows%20Service%20%7C%20Middleware-informational?style=flat-square)
![Status](https://img.shields.io/badge/Release-Showcase%20Edition-green?style=flat-square)

An automated integration service and document generation engine built for **SAP Business ByDesign**. This solution automates the extraction of customer ledger transactions, balances, and payment allocations via SAP web services and compiles branded, production-ready Customer Account Statements for distribution.

> **Notice:** This repository serves as a **technical architecture showcase**. Proprietary extraction routines, PDF rendering logic, and production credentials have been stubbed or omitted.

---

## 📌 Business Context & Operational Impact

* **The Problem:** Standard manual statement runs in SAP ByDesign require repetitive manual exports, lack dynamic document customization, and can lead to reconciliation delays during high-volume month-end billing cycles.
* **The Solution:** A decoupled background service that runs on schedule or via CLI trigger, authenticating directly against SAP ByDesign endpoints, calculating aging intervals, and compiling customer-ready statements.
* **Operational Impact:** Eliminates manual data compilation, ensures consistent aging calculations across customer accounts, and provides clean audit trails for statement distribution.

---

## 🏗 System Architecture

```text
[ Trigger Layer ]
  │  • Scheduled Windows Service poll (pywin32) / CLI trigger
  ▼
[ SAP ByDesign Integration Layer ]
  │  • Authenticates against SAP tenant endpoints
  │  • Fetches CSRF security tokens & manages session state
  │  • Queries Customer Receivables Open Items via OData / SOAP
  │  • Handles query pagination ($top, $skip) and network retries
  ▼
[ Financial Processing & Reconciliation Core ]
  │  • Normalizes multi-currency line items (Invoices, Credit Memos, Payments)
  │  • Reconciles cleared vs. unallocated balances
  │  • Computes running balances and aging intervals (Current, 30, 60, 90, 120+ days)
  ▼
[ Document Compilation & Distribution ]
  │  • Generates multi-page PDF statements with dynamic table pagination
  │  • Outputs to local archive directories and dispatches via SMTP
  │  • Writes execution logs to Windows Event Viewer & rotating log files
