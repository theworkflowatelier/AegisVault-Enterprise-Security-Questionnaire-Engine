# 🛡️ AegisVault — Enterprise Security Questionnaire Engine

[![License: Commercial](https://img.shields.io/badge/License-Commercial%20Enterprise-blue.svg)](https://gumroad.com/l/aegisvault-enterprise)
[![Platform: n8n](https://img.shields.io/badge/Platform-n8n%20v1.0+-ff6600.svg)](https://n8n.io)
[![AI Engine: Google Gemini](https://img.shields.io/badge/AI%20Engine-Gemini%201.5%20Pro%20%2F%202.0%20Flash-4285F4.svg)](https://ai.google.dev/)
[![Security: SOC 2 Aligned](https://img.shields.io/badge/Security-SOC%202%20%2F%20ISO%2027001%20Aligned-00cc44.svg)](#data-security--privacy-standards)

## 📌 Executive Overview
**AegisVault** is an enterprise-grade automation architecture designed to eliminate the manual engineering overhead of answering B2B vendor security assessments (SOC 2 Type II, ISO/IEC 27001, GDPR, NIST, SIG).

Instead of pulling senior developers away from core product roadmaps to copy-paste spreadsheet answers for 15 hours per deal, AegisVault automates the end-to-end evaluation process in under **3 minutes** using self-hosted [n8n](https://n8n.io) and Google Gemini AI.

---

## ⚡ Key Architectural Highlights
* **15-Row Throttled Execution:** Prevents API rate-limit crashes (HTTP 429) and output token truncation by executing multi-format spreadsheets (Excel, CSV, Google Sheets) in micro-batches.
* **Deterministic Confidence Gating:** Eliminates LLM hallucinations. If an incoming security question cannot be verified against internal company policy text, it is tagged as `Knowledge_Match: "Missing"` and routed to a Human-in-the-Loop (HITL) review queue.
* **Zero Model Training:** Executes via private Google Cloud Enterprise API endpoints. In accordance with our Data Security Addendum, customer payloads are never retained, logged, or utilized to train foundational LLM models.

---

### 📐 System Topology
The engine operates across 5 isolated asynchronous processing zones:

| Processing Zone | Architectural Function | Output / Routing Action |
| :--- | :--- | :--- |
| **🔵 Zone 1: Ground Truth Memory** | Ingests SOC 2, ISO, and NIST policy documents. | Aggregates text into ephemeral execution memory. |
| **🟣 Zone 2: Multi-Format Parser** | Reads Excel (`.xlsx`), CSV, and Google Sheets. | Normalizes varied column headers into structured JSON arrays. |
| **🟡 Zone 3: AI Evaluation Core** | Throttles data into 15-row execution batches. | Evaluates answers via Gemini 1.5 Pro / 2.0 Flash JSON schema. |
| **🟢 Zone 4: Confidence Gate** | Evaluates verbatim citation match confidence. | **Present:** Appends to Master Output.<br>**Missing:** Routes to HITL Review Queue. |
| **🔴 Zone 5: Audit & Error Log** | Monitors API rate limits and execution faults. | Appends immutable error logs to the System Audit dashboard. |



*(View the complete, color-coded interactive Mermaid diagram in our `/Docs/` directory).*

---

## 🔒 Data Security & Privacy Standards
AegisVault is built for zero-data-retention compliance. All processing occurs within your self-hosted or cloud-isolated n8n workspace. 
* Review our formal **[Data Security Addendum (DSA)](./Docs/Data_Security_Addendum.md)** for complete technical specifications regarding AES-256 encryption at rest, TLS 1.3 transit security, and scoped OAuth 2.0 access governance.

---

## 🚀 Get the Production Automation Engine
The complete, pre-configured **AegisVault production software bundle** is distributed via Commercial Enterprise License.

### What is Included in the Production Release:
1. **`AegisVault_Production_Engine.json`**: The complete 20-node n8n workflow asset ready for 1-click import.
2. **15-Minute Deployment Manual**: Step-by-step visual configuration guide for non-technical administrators.
3. **Sandbox Testing Kit**: Pre-packaged test policies and benchmark answer keys to verify 100% accuracy out of the box.
4. **Lifetime Updates & Enterprise Standards**: No recurring SaaS seat fees, no vendor lock-in, and zero per-user billing.

👉 **[Download the Complete AegisVault Enterprise Engine](https://theworkflowatelier.gumroad.com/l/aegisvault-enterprise)**

---
*For enterprise custom deployments, custom integrations, or security reviews, contact our technical team via Gumroad.*
