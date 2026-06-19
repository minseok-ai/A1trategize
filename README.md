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
  <strong>한국어</strong> | <a href="README.en.md">English</a>
</p>

---

## Overview

**A1trategize**는 전문화된 AI 에이전트들을 조합하여 비즈니스, 커리어, 지식재산(IP), 그리고 반도체 공정(NNFC) 도메인의 전략 보고서를 생성하는 엔터프라이즈급 AI 컨설팅 시스템입니다.

단순한 일방향 텍스트 생성을 넘어, **노드(Node)와 조건부 엣지(Conditional Edge)로 구성된 상태 머신 기반의 파이프라인 하네스 엔진(Harness Engine)** 을 통해 작동합니다. 사용자의 질문을 분석해 최적의 도메인을 할당하고, 동적 프롬프트와 방대한 지식 베이스(JSON DB)를 결합하여 조사, 비평, 초안 작성, 반복 QA 과정을 독립적인 에이전트들이 수행합니다.

이러한 전 과정은 FastAPI를 통해 Server-Sent Events(SSE)로 프론트엔드에 실시간 스트리밍되며, 최종적으로 전문적인 DOCX, PDF, PPTX 문서로 변환됩니다.

### Why A1trategize?

| 기존 단일 LLM 방식 | A1trategize v0.5 (Graph-based Harness) |
|---|---|
| 단일 모델 응답에 의존 | 조사, 비평, 초안, QA 역할을 분리하고 최적 모델(Gemini, Solar, Sonar 등)에 동적 라우팅 |
| 단순 프롬프트 입력 | 노드(Node) 단위로 검증, 텔레메트리, 상태 체크포인트를 관리하는 하네스 엔진 |
| 도메인 구분 없는 일반적 답변 | 4개의 독립된 딥테크 도메인 모듈(비즈니스, 커리어, IP, 반도체 공정)과 전용 지식 베이스 |
| 블랙박스 형태의 대기 시간 | SSE(Server-Sent Events)를 통해 프론트엔드에서 노드별 진행 상황을 실시간 트래킹 |
| 텍스트 복사/붙여넣기 | 워터마크가 적용된 DOCX, PDF, 프레젠테이션용 PPTX 자동 생성 |

---

## 🚀 최근 업데이트 (Changelog)

### Link Vault 및 NNFC 참조 정규화 시스템 구축 (v0.8)
*LLM의 환각(Hallucination)으로 인한 가짜 URL과 존재하지 않는 장비 참조를 원천 차단하기 위해 Link Vault와 정규화(Canonicalization) 엔진을 도입했습니다.*

- **Link Vault 시스템 도입**: 비즈니스, 커리어, 특허 도메인에서 LLM이 본문에 직접 URL을 생성하는 것을 금지했습니다. 대신 백엔드의 Link Vault 시스템이 검증된 URL만 사후에 자동 주입하도록 프롬프트 엔진을 전면 개편하여 "가짜 링크(Dead Link)" 문제를 근본적으로 해결했습니다.
- **NNFC 장비 참조 정규화 (Canonicalization)**: `NNFCEquipmentEngine`이 단순히 장비 ID를 검증하는 것을 넘어, 본문에 등장하는 장비명을 `장비명 (ID: 123) [NNFC DB: 123]` 형태의 표준 포맷으로 자동 정규화합니다. DB에 없는 허위 ID가 발견될 경우 맹목적으로 수정하지 않고 `[NNFC equipment reference requires engineer confirmation]` 형태의 알림으로 부드럽게 치환하여 엔지니어의 확정을 유도합니다.
- **Alibaba LLM 네이티브 연동 및 Reasoning Block 정제**: 프론트엔드 라우터에 Alibaba 모델 지원을 공식 추가했습니다. 또한 DeepSeek, Solar 등 추론형(Reasoning) 모델들이 출력하는 `<think>`, `<details>`, ````thinking```` 등 내부 사고 과정을 파싱 단계에서 자동으로 제거하여, 보고서 결과물에 불필요한 메타 텍스트가 노출되지 않도록 UI 파이프라인을 정제했습니다.
- **시간대(Timezone) 라이브러리 현대화**: 외부 의존성인 `pytz` 모듈을 완전히 제거하고, Python 3.9+ 표준 라이브러리인 `zoneinfo.ZoneInfo`로 모든 도메인 프롬프트의 시간대 처리 로직을 마이그레이션하여 성능과 호환성을 높였습니다.


---

## Core Features

### 4-Domain Deep-Tech Architecture

A1trategize는 사용자의 질문을 분석하여 4가지 전문 컨설팅 모듈 중 하나를 로드합니다.

| Domain | Description | Key Capabilities |
|---|---|---|
| Business Strategy | 기업 경영 전략 및 시장 분석 | 시장 조사, 경쟁사 분석, 재무 관점, 실행 계획 |
| Career Analysis | 개인 커리어 및 지원 전략 | 이력/자소서 분석, 직무 적합성, 강점 진단, 면접 대비 |
| IP & Patent Strategy | 지식재산 및 특허 전략 문서 | 선행기술 조사, 명세서 방향, 권리화 리스크 검토 |
| NNFC Process Recipe | 딥테크 반도체 장비 및 공정 | 국가나노인프라협의체 장비 스펙 정합성, 공정 레시피 설계 |

### Graph-based Execution Engine
모든 과정은 `PipelineHarness` 엔진 위에서 실행됩니다. 각 에이전트의 작업은 독립된 노드(Node)로 정의되며, 평가(Adequacy Scoring, QA) 결과에 따라 다음 노드 또는 재수정(Self-Correction) 노드로 라우팅되는 그래프 구조를 가집니다.

### LLM Router & Dynamic Assignment
하드코딩된 API 호출을 탈피하고 `llm_router.py`를 통해 역할(Research, Critique, Draft)에 가장 적합한 LLM 모델을 런타임에 동적으로 할당합니다. 사용자 인터페이스에서 이를 실시간으로 변경할 수도 있습니다.

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
|-- server.py                 # FastAPI 백엔드 진입점 (REST API 및 SSE 스트리밍)
|-- main.py                   # CLI 환경 테스트용 헤드리스 실행기
|-- harness_engine.py         # Graph 기반 파이프라인 워크플로우 상태 머신
|-- pipeline_service.py       # 하네스 엔진 초기화 및 컨설팅 파이프라인 로직
|-- llm_router.py             # 역할별 LLM 모델 동적 할당 라우터
|-- prompt_selector.py        # 도메인 자동 분류 및 프롬프트 동적 로더
|-- domains/                  # 4대 도메인(Business, Career, IP, NNFC) 핵심 모듈 폴더
|   |-- business/             # 비즈니스 전용 프롬프트 및 지식 베이스
|   |-- career/               # 커리어 전용 프롬프트 및 지식 베이스
|   |-- ip/                   # 특허 전용 프롬프트 및 지식 베이스
|   `-- nnfc/                 # 반도체 공정 프롬프트, 장비 DB, 구조화 엔진
|-- static/                   # 프론트엔드 정적 파일
|   |-- index.html            # 메인 웹 UI
|   |-- app.js                # SSE 트래커 및 동적 모델 셀렉터 제어 로직
|   `-- style.css             # 모던 UI/UX 스타일
|-- requirements.txt          # Python 패키지 의존성
`-- LICENSE                   # Technical Report Sharing License
```

---

## Tech Stack

| Area | Stack | Role |
|---|---|---|
| Frontend | Vanilla JS, HTML5, CSS3 | 로컬 웹 인터페이스, SSE 진행 상태 시각화 |
| Backend | FastAPI, Uvicorn, SSE-Starlette | 비동기 API 서버 및 실시간 상태 스트리밍 |
| Pipeline Engine | Python Dataclasses, Graph Logic | 노드 기반 상태 머신 (Harness Engine) |
| LLM Client | `google-genai`, `openai`, `requests` | Gemini, Solar, Sonar 모델 호출 (Router 패턴) |
| Document Gen | `python-docx`, `docx2pdf`, `python-pptx` | 문서 생성 로직 |

---

## Quality Controls

| Control | Description |
|---|---|
| Domain Selection & Routing | LLM 도메인 분류 실패 시 사전에 정의된 키워드 기반 fallback 매칭으로 자동 복구 |
| Harness Telemetry | 파이프라인 엔진에서 노드별 소요 시간(Duration) 및 에러를 실시간으로 추적 및 진단 |
| Adequacy Scoring | 수집된 기초 자료의 길이, 수치, 참고 근거, 키워드 빈도를 종합 평가하여 보충 조사 분기 |
| Checklist-First QA Gate | 단순 점수를 넘어, 각 도메인별 필수 요건이 담긴 JSON 체크리스트의 CRITICAL 항목 전원 통과 여부를 최우선 승인 조건으로 삼음 |
| Domain Guardrail Node | 특허 내 코드블럭 금지, 이력서 합격 보장 문구 금지 등 도메인 특화 규칙을 사후 검증하며, NNFC 제약사항은 QA 루프 내로 통합됨 |
| Offline Benchmark Eval | EvalHarness를 통해 QA 점수, 분류 정확도, 키워드 적중률, 노드 Latency를 프로덕션과 독립적으로 오프라인 벤치마킹 |

---

## Public Technical Report Boundary

이 저장소의 공개 문서는 시스템 구조와 파이프라인을 설명하기 위한 기술 보고서입니다.

Public repository에 포함되는 항목:

| Included | Purpose |
|---|---|
| `README.md` | 한국어 메인 기술 문서 |
| `README.en.md` | 영문 기술 문서 |
| `LICENSE` | 문서 공유 범위와 제한 사항 |

Public repository에서 제외되는 항목:

| Excluded | Reason |
|---|---|
| Source code | 구현 세부사항과 지식재산 보호 |
| Full prompt text | 프롬프트 자산과 운영 노하우 보호 |
| Provider credentials | 보안 정보 보호 |
| Private datasets | NNFC JSON 등 비공개 자료 보호 |
| Generated reports | 고객/주제별 산출물 보호 |

---

## License and Rights

- Documentation license: [Minseok Song & Company Technical Report Sharing License v1.0](LICENSE)
- Patent reference: KR 10-2026-0009508
- Copyright: 2025-2026 Minseok Song
- Author: Minseok Song & Company

<p align="center">
  <em>Built by Minseok Song &amp; Company</em>
</p>
