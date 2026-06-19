<p align="center">
  <img src="logo.png" alt="A1trategize Logo" width="280" />
</p>

<h1 align="center">A1trategize - Your AI Strategy Team</h1>

<p align="center">
  <strong>Enterprise-Grade Graph-based Strategic Report Generation System</strong>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Technical_Report_Sharing-blue.svg" alt="Technical Report Sharing License" /></a>
  <img src="https://img.shields.io/badge/Release-v0.5-lightgrey.svg" alt="Release v0.5" />
  <img src="https://img.shields.io/badge/Patent-KR_10--2026--0009508-green.svg" alt="Patent KR 10-2026-0009508" />
  <img src="https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white" alt="Python 3.11+" />
  <img src="https://img.shields.io/badge/Backend-FastAPI-009688?logo=fastapi&logoColor=white" alt="FastAPI" />
  <img src="https://img.shields.io/badge/Frontend-Vanilla_JS-F7DF1E?logo=javascript&logoColor=black" alt="Vanilla JS" />
  <img src="https://img.shields.io/badge/Output-DOCX%20%2F%20PDF%20%2F%20PPTX-orange" alt="DOCX PDF PPTX output" />
</p>

<p align="center">
  <em>Release: v0.5 · Author: Minseok Song &amp; Company</em>
</p>

<p align="center">
  <a href="README.md">한국어</a> | <strong>English</strong>
</p>

---

## Overview

**A1trategize** is an enterprise-grade AI consulting system that generates strategic reports across Business, Career, Intellectual Property (IP), and Semiconductor Process (NNFC) domains by orchestrating specialized AI agents.

Moving beyond simple unidirectional text generation, it operates on a **State Machine-based Pipeline Harness Engine composed of Nodes and Conditional Edges**. It analyzes user queries to allocate the optimal domain, combining dynamic prompts with massive knowledge bases (JSON DB) to execute research, critique, drafting, and iterative QA across independent agents.

This entire process is streamed in real-time to the frontend via Server-Sent Events (SSE) using FastAPI, ultimately culminating in professional DOCX, PDF, and PPTX documents.

### Why A1trategize?

| Legacy Single LLM | A1trategize v0.5 (Graph-based Harness) |
|---|---|
| Relies on a single model's response | Separates roles (Research, Critique, Draft, QA) and dynamically routes to the optimal model (Gemini, Solar, Sonar, etc.) |
| Simple prompt input | Harness engine that manages validation, telemetry, and state checkpoints per Node |
| Generic answers lacking domain specificity | 4 independent deep-tech domain modules (Business, Career, IP, Semiconductor) with dedicated Knowledge Bases |
| Blackbox waiting times | Real-time tracking of node progress on the frontend via SSE (Server-Sent Events) |
| Manual copy-pasting | Automatically generates watermarked DOCX, PDF, and presentation-ready PPTX files |

---

## 🚀 Recent Updates (Changelog)

### Deterministic NNFC/Link Vault Control (v0.9)
*Based on the harness engine diagnosis report (current_harness_diagnosis_2026-06-01), we transitioned areas previously dependent on LLM inference into deterministic system controls, significantly enhancing the stability of the pipeline architecture.*

- **Full DAG Integration of Link Vault**: Completely deprecated URL injection logic from LLM prompts (`DEPRECATED: URL MANAGEMENT NOW HANDLED BY LINK VAULT`) and natively integrated `link_vault` into the central `PipelineState`. LLMs no longer generate URLs; the backend securely and deterministically injects all citations.
- **Deterministic NNFC Equipment Matching**: Replaced the LLM-based equipment candidate selection with a programmatic, deterministic search (`search_combined`, `suggest_equipment`). If an equipment constraint (gas, temperature, wafer size) fails, the system automatically suggests DB-backed alternatives via `_nnfc_alternative_equipment()`.
- **Prohibition of Estimated Values (EST)**: Removed the `[EST: Standard Process Basis]` tag from NNFC prompts. LLMs are now strictly prohibited from inferring missing parameters. Any value not explicitly found in the DB must be labeled `N/A [DB not included - operator confirmation required]`, completely neutralizing technical hallucinations.
- **Alibaba DeepSeek & Qwen Reasoning Model Integration**: Added `AlibabaProvider` (`deepseek-v4-pro`, `qwen3.6-plus`) to the backend router (`llm_providers.py`) with `supports_reasoning=True` attribute, seamlessly integrating state-of-the-art models that provide reasoning traces.
- **Structured JSON Consolidation & Codebase Lightweighting**: Stripped away legacy XML prompt instructions in favor of strict JSON Schema-based QA and Review gates. Additionally, removed excessively verbose English docstrings across the Harness Engine to significantly lighten the codebase and improve maintainability.


---

## Core Features

### 4-Domain Deep-Tech Architecture

A1trategize analyzes user questions and loads one of four specialized consulting modules.

| Domain | Description | Key Capabilities |
|---|---|---|
| Business Strategy | MBB-style corporate strategy | Market research, competitor analysis, financial perspectives, execution plans |
| Career Analysis | Personal career & application strategy | Resume analysis, job fit, strengths diagnosis, interview prep |
| IP & Patent Strategy | Intellectual property strategy | Prior art review, specification direction, risk assessment |
| NNFC Process Recipe | Deep-tech semiconductor equipment | NNFC equipment spec validation, structured process recipe design |

### Graph-based Execution Engine
All processes run on the `PipelineHarness` engine. Each agent's task is defined as an independent Node, possessing a graph structure that routes to the next node or a Self-Correction node based on evaluations (Adequacy Scoring, QA).

### LLM Router & Dynamic Assignment
Abstracts hardcoded API calls. The `llm_router.py` dynamically assigns the most suitable LLM model for specific roles (Research, Critique, Draft) at runtime. Users can also change this in real-time via the interface.

---

## System Architecture

### High-Level Overview

```mermaid
graph TD
    User["Web Frontend (Vanilla JS)"] -->|HTTP Request| API["FastAPI Backend (server.py)"]
    API --> Router["LLM Router & Provider Abstraction"]
    API --> Engine["Pipeline Harness Engine (Graph-based)"]
    
    subgraph Harness [Pipeline Harness Engine]
        Init["Init & Domain Select"] --> Exp["Query Expansion"]
        Exp --> Res["Initial Research"]
        Res --> Rev["Research Review"]
        Rev --> Score{"Adequacy Score"}
        Score -->|Insufficient| Supp["Supplementary Research"]
        Score -->|Sufficient| Draft["Draft Report"]
        Supp --> Draft
        Draft --> QA{"Iterative QA Loop"}
        QA -->|Fail| Corr["Self-Correction"]
        Corr --> QA
        QA -->|Pass| Final["Final Validation"]
    end
    
    Engine -.->|SSE Telemetry| User
    Final --> Output["Document Generation (DOCX/PDF/PPTX)"]
    Output --> User
```

---

## Project Structure

```text
A1trategize/
|-- server.py                 # FastAPI backend entry point (REST API & SSE streaming)
|-- main.py                   # Headless runner for CLI testing
|-- harness_engine.py         # Graph-based pipeline workflow state machine
|-- pipeline_service.py       # Harness engine initialization and logic
|-- llm_router.py             # Dynamic router for role-based LLM assignment
|-- prompt_selector.py        # Domain auto-classification & prompt loader
|-- domains/                  # Core modules for the 4 major domains
|   |-- business/             # Prompts and KBs for Business Strategy
|   |-- career/               # Prompts and KBs for Career Analysis
|   |-- ip/                   # Prompts and KBs for IP/Patent Strategy
|   `-- nnfc/                 # Semiconductor prompts, equipment DB, structured engine
|-- static/                   # Frontend static files
|   |-- index.html            # Main Web UI
|   |-- app.js                # SSE tracker and dynamic model selector logic
|   `-- style.css             # Modern UI/UX styling
|-- requirements.txt          # Python dependencies
`-- LICENSE                   # Technical Report Sharing License
```

---

## Tech Stack

| Area | Stack | Role |
|---|---|---|
| Frontend | Vanilla JS, HTML5, CSS3 | Local web interface, SSE progress visualization |
| Backend | FastAPI, Uvicorn, SSE-Starlette | Async API server and real-time state streaming |
| Pipeline Engine | Python Dataclasses, Graph Logic | Node-based state machine (Harness Engine) |
| LLM Client | `google-genai`, `openai`, `requests` | Gemini, Solar, Sonar model routing |
| Document Gen | `python-docx`, `docx2pdf`, `python-pptx` | Document generation logic |

---

## Quality Controls

| Control | Description |
|---|---|
| Domain Selection & Routing | Automatic fallback to predefined keyword matching if LLM domain classification fails |
| Harness Telemetry | Real-time tracking and diagnostics of execution duration and error rates per Node in the Harness Engine |
| Adequacy Scoring | Evaluates the length, figures, references, and keyword frequency of collected data to trigger Supplementary Research |
| Checklist-First QA Gate | Moves beyond simple scores by prioritizing the passage of ALL CRITICAL items in a domain-specific JSON checklist |
| Domain Guardrail Node | Strictly post-validates domain-specific rules (e.g. no code blocks in patents), with NNFC constraints now unified directly into the QA Loop |
| Offline Benchmark Eval | Utilizes EvalHarness to benchmark QA scores, classification accuracy, keyword hits, and node latency independently from production |

---

## Public Technical Report Boundary

The public documentation in this repository serves as a technical report describing the system architecture and pipeline.

Included in Public Repository:

| Included | Purpose |
|---|---|
| `README.md` | Main technical documentation (Korean) |
| `README.en.md` | English technical documentation |
| `LICENSE` | Document sharing scope and restrictions |

Excluded from Public Repository:

| Excluded | Reason |
|---|---|
| Source code | IP and implementation detail protection |
| Full prompt text | Prompt asset protection |
| Provider credentials | Security |
| Private datasets | NNFC JSON and private data protection |
| Generated reports | Client/topic-specific output protection |

---

## License and Rights

- Documentation license: [Minseok Song & Company Technical Report Sharing License v1.0](LICENSE)
- Patent: Pending
- Copyright: 2025-2026 Minseok Song
- Author: Minseok Song & Company

<p align="center">
  <em>Built by Minseok Song &amp; Company</em>
</p>
