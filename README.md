# SAP Business ByDesign — Automated Customer Statement Service

![SAP ByDesign](https://img.shields.io/badge/SAP-ByDesign-0FAAFF?style=flat-square&logo=sap&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Windows%20Service%20%7C%20Middleware-informational?style=flat-square)
![Status](https://img.shields.io/badge/Release-Showcase%20Edition-green?style=flat-square)

An automated integration service and document generation engine built for **SAP Business ByDesign**. This solution automates the extraction of customer ledger transactions, balances, and payment allocations via SAP web services and compiles branded, production-ready Customer Account Statements for distribution.

> **Notice:** This repository serves as a **technical architecture showcase**. Proprietary extraction routines, PDF rendering logic, and production credentials have been stubbed or omitted. All rights reserved.

---

## 📌 Business Challenge & Impact

* **The Problem:** Standard ERP manual statement generation is time-consuming, prone to omission during billing cycles, and difficult to format dynamically to custom client specifications.
* **The Solution:** A decoupled background service that runs on schedule or via API trigger, querying SAP ByDesign financial data, computing aging intervals, and generating consolidated statements automatically.
* **Impact:** Eliminates manual billing extraction, ensures accurate aging balances, and reduces month-end reconciliation overhead.

---

## 🏗 System Architecture

\\\	ext
+----------------------------+
|   SAP Business ByDesign    |
|  (Financials & Receivables)|
+--------------+-------------+
               |
        OData / SOAP APIs
        (SSL / Basic / OAuth)
               |
               v
+----------------------------+
| Python Middleware Service  |
|  - Auth & Session Mgmt     |
|  - Ledger Parser & Aging   |
|  - Windows Service Daemon  |
+--------------+-------------+
               |
               v
+----------------------------+
|  Statement Document Engine |
|  - Dynamic Layout Builder  |
|  - Custom Statement PDF    |
|  - Automated Dispatch/Save |
+----------------------------+
\\\

---

## ⚙️ Key Technical Capabilities

* **SAP ByDesign Service Consumption:** Communicates with SAP ByDesign customer ledger and open item services via REST/OData protocols.
* **Financial Ledger Aggregation:** Parses debit/credit lines, open item clearing states, down payments, and invoice balancing.
* **Aging Analysis Pipeline:** Dynamically computes standard accounting aging buckets (Current, 30, 60, 90+ days) from transaction due dates.
* **Windows Service Architecture:** Packaged as an autonomous background daemon with automated restart capabilities, event logging, and status monitoring.
* **Defensive Error Handling:** Built-in network retry backoffs, API rate handling, and structured diagnostic logging.

---

## 🧩 Interface & Data Contract (Sanitized Sample)

\\\python
from dataclasses import dataclass
from typing import List, Optional
from datetime import date

@dataclass
class LedgerEntry:
    posting_date: date
    document_type: str
    document_number: str
    reference: Optional[str]
    debit_amount: float
    credit_amount: float
    balance: float

@dataclass
class StatementPayload:
    account_id: str
    customer_name: str
    statement_date: date
    opening_balance: float
    closing_balance: float
    entries: List[LedgerEntry]
    aging_buckets: dict

class ByDesignStatementService:
    """
    Middleware service orchestrating SAP ByDesign data extraction
    and statement document compilation.
    """
    def __init__(self, tenant_url: str, credential_store):
        self.tenant_url = tenant_url
        self.credentials = credential_store

    def build_statement(self, account_id: str, period_start: date, period_end: date) -> bytes:
        """
        Extracts open items, applies reconciliation rules, and returns compiled PDF bytes.
        
        NOTE: Core financial logic and document layout routines are proprietary.
        Contact karanidenis068@gmail.com for implementation details.
        """
        raise NotImplementedError("Core pipeline omitted in showcase release.")
\\\

---

## 🛠 Tech Stack

* **Language:** Python 3.x
* **Enterprise ERP:** SAP Business ByDesign
* **Protocols & Data:** OData, SOAP Web Services, XML/JSON, REST
* **Infrastructure:** Windows Server, pywin32 Service Runner, Structured Logging

---

## 🔒 Commercial Notice & License

Copyright © 2026 Denis Karani Mwangi. All Rights Reserved.

This repository is published strictly for demonstration, code-style evaluation, and portfolio purposes. No authorization is granted to copy, alter, distribute, or execute this software for commercial or private deployment without explicit written permission.

---

## 📬 Contact & Inquiries

For consulting engagements, SAP enterprise integrations, or custom extension development:

* **Developer:** Denis Karani Mwangi
* **LinkedIn:** [Denis Karani](https://www.linkedin.com/in/denis-karani-2a8b4b270/)
* **Email:** [karanidenis068@gmail.com](mailto:karanidenis068@gmail.com)
