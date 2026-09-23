<a id="readme-top"></a>

<div align="center">

<!-- Optional: drop a logo at docs/assets/logo.png and uncomment this line -->
<!-- <img src="docs/assets/logo.png" alt="SpectraDerm logo" width="120" /> -->

# 🔬 SpectraDerm

**An AI-powered skin monitoring prototype that turns an ordinary phone photo into a longitudinal signal, no multispectral hardware required.**

![Status](https://img.shields.io/badge/Status-Prototype-FFFFFF?style=for-the-badge&labelColor=5C4A3B)
![Python](https://img.shields.io/badge/Python-3.10%2B-FFFFFF?style=for-the-badge&labelColor=5C4A3B)
![License](https://img.shields.io/badge/License-Educational%20Use-FFFFFF?style=for-the-badge&labelColor=5C4A3B)

<!-- Once this is pushed to GitHub, swap YOUR_USERNAME below for live, auto-updating badges -->
<!--
![Last Commit](https://img.shields.io/github/last-commit/YOUR_USERNAME/SpectraDerm?style=for-the-badge)
![Issues](https://img.shields.io/github/issues/YOUR_USERNAME/SpectraDerm?style=for-the-badge)
![Stars](https://img.shields.io/github/stars/YOUR_USERNAME/SpectraDerm?style=for-the-badge)
-->

</div>

<br/>

> [!IMPORTANT]
> SpectraDerm is a **monitoring and awareness** prototype. It does not diagnose skin disease, output a disease probability, or replace a dermatologist. Its spectral output is AI‑*estimated*, not a physical multispectral/hyperspectral measurement.

<br/>

## 🎥 Demo

<div align="center">

<video src="https://github.com/user-attachments/assets/361f7c7d-35b5-4c97-9c19-60450a9bd97a" controls width="100%"></video>

</div>

<p align="right"><a href="#readme-top">back to top ↑</a></p>


<!--
  HOW TO ADD YOUR DEMO VIDEO — pick one:

  OPTION 1 (recommended): Native, playable GitHub video
    1. Open any Issue or PR on this repo in your browser (you can close/delete it after).
    2. Drag-and-drop your demo .mp4 into the comment box.
    3. GitHub uploads it and gives you a link like:
       https://github.com/user-attachments/assets/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    4. Paste that link as the src below — it will play inline with controls on GitHub.

    <video src="https://github.com/user-attachments/assets/361f7c7d-35b5-4c97-9c19-60450a9bd97a" controls width="100%"></video>

  OPTION 2: YouTube (click-through thumbnail, works everywhere incl. npm/PyPI mirrors)

    <a href="https://youtube.com/watch?v=YOUR_VIDEO_ID">
      <img src="https://img.youtube.com/vi/YOUR_VIDEO_ID/maxresdefault.jpg" width="80%" alt="SpectraDerm demo video" />
    </a>

  OPTION 3: Looping GIF preview (silent, autoplays, largest file size)

    <img src="docs/assets/demo.gif" width="80%" alt="SpectraDerm demo" />
-->

<img src="https://github.com/user-attachments/assets/361f7c7d-35b5-4c97-9c19-60450a9bd97a" />

</div>

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 📖 Table of Contents

- [System Architecture](#-system-architecture)
- [Pipeline Overview](#-pipeline-overview)
- [Spectral Reconstruction](#-spectral-reconstruction)
- [Feature Engineering](#-feature-engineering)
- [Machine Learning and Change Detection](#-machine-learning-and-change-detection)
- [Personal Baseline and Longitudinal Monitoring](#-personal-baseline-and-longitudinal-monitoring)
- [RAG Pipeline](#-rag-pipeline)
- [Multi-Agent Architecture](#-multi-agent-architecture)
- [MCP Integration](#-mcp-integration)
- [Final Report](#-final-report)
- [Application Flow](#-application-flow)
- [Technology Stack](#-technology-stack)
- [Project Structure](#-project-structure)
- [Environment Variables](#-environment-variables)
- [Installation](#-installation)
- [Testing](#-testing)
- [Privacy and Safety](#-privacy-and-safety)
- [Limitations](#-limitations)
- [Project Goals](#-project-goals)
- [Team](#-team)
- [Disclaimer](#-disclaimer)
- [License](#-license)


**User journey:** `Home → Scan → Analysis → Result → History → Explanation → Next Action`

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🏗️ System Architecture

```mermaid
flowchart TD
    A["📷 RGB Image"] --> B["Image Quality Check"]
    B --> C["Skin / Region Detection"]
    C --> D["RGB-to-Spectral Reconstruction"]
    D --> E["RGB Features"]
    D --> F["Spectral Features"]
    E --> G["Feature Engine"]
    F --> G
    G --> H["Machine Learning"]
    H --> I["Change / Anomaly Score"]
    I --> J["Personal Baseline"]
    J --> K["Temporal Comparison"]
    K --> L["Agent System"]
    L --> M["Vision Agent"]
    L --> N["Monitoring Agent"]
    L --> O["Evidence Agent"]
    M --> P["Safety Agent"]
    N --> P
    O --> P
    P --> Q["Product Agent"]
    P --> R["Referral Agent"]
    Q --> S["MCP Server"]
    R --> S
    S --> T["ML Tools"]
    S --> U["RAG Tools"]
    S --> V["External Tools"]
    T --> W["Final Report"]
    U --> W
    V --> W

    style P fill:#e8590c,color:#fff
    style W fill:#2f9e44,color:#fff
```

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🔄 Pipeline Overview

A higher-level view of the same journey, stage by stage:

```mermaid
flowchart LR
    A["RGB Image"] --> B["Quality + Region Detection"]
    B --> C["Spectral Reconstruction"]
    C --> D["Feature Extraction"]
    D --> E["ML Analysis"]
    E --> F["Baseline + History"]
    F --> G["Agent Reasoning"]
    G --> H["RAG Evidence"]
    H --> I["Safety Assessment"]
    I --> J["Referral / Product Guidance"]
    J --> K["Final Report"]

    style I fill:#e8590c,color:#fff
    style K fill:#2f9e44,color:#fff
```

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## Spectral Reconstruction

SpectraDerm converts an ordinary RGB image into an **AI-estimated spectral representation**, used to derive additional features alongside conventional RGB features.

```mermaid
flowchart LR
    A["RGB Image<br/>H × W × 3"] --> B["Spectral Reconstruction Model<br/>(MST++)"] --> C["Estimated Spectral Image<br/>H × W × N"]
```

> [!NOTE]
> This is explicitly *AI-estimated* spectral information, not an actual multispectral/hyperspectral measurement.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🧬 Feature Engineering

| Category | Examples |
|---|---|
| **RGB Features** | Color statistics, texture, shape, local contrast, pigmentation-related features |
| **Spectral Features** | Band-wise intensity, spectral ratios, spectral differences, regional spectral statistics, spectral signatures |
| **Temporal Features** | Historical scan information used for longitudinal monitoring and comparison |

The combined feature representation feeds the downstream machine-learning pipeline.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🤖 Machine Learning and Change Detection

SpectraDerm compares an RGB-only baseline against an **RGB + estimated-spectral** representation to test whether the extra spectral signal helps.

The output is a **change/anomaly score**, not a disease probability:

```
Change / Anomaly Score: 72 / 100
```

This means the observed pattern differs from the relevant reference pattern — **it is not a 72% probability of disease.**

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 👤 Personal Baseline and Longitudinal Monitoring

Because everyone's skin differs, SpectraDerm builds a **personal** reference rather than comparing against a population average.

```mermaid
flowchart TD
    subgraph S1["First Scan"]
        A["First Scan"] --> B["RGB + Estimated Spectrum"] --> C["Personal Baseline"]
    end
    subgraph S2["Every Scan After"]
        D["Current Scan"] --> F["Difference Analysis"]
        C --> F
        F --> G{"Stable / Changed /<br/>Increasing Change"}
    end
```

Trend visualization becomes more meaningful as more scans accumulate.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 📚 RAG Pipeline

Used whenever the user asks **"Why was this flagged?"** — retrieves relevant dermatology evidence and grounds the explanation in it, rather than letting a model free-generate the answer.

```mermaid
flowchart TD
    A["Knowledge Documents"] --> B["Cleaning"]
    B --> C["Chunking"]
    C --> D["Embedding<br/>(BAAI/bge-small-en-v1.5)"]
    D --> E["Local Vector Store"]
    E --> F["Semantic Retrieval"]
    F --> G["Relevant Evidence"]
    G --> H["Evidence-Grounded Explanation"]
```

The knowledge base covers general dermatology, skin pigmentation, melanin, common skin conditions, warning signs, and situations where professional assessment may help.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🧠 Multi-Agent Architecture

| Agent | Responsibility |
|---|---|
| **Vision Agent** | Interprets image, spectral, feature, and ML outputs |
| **Monitoring Agent** | Handles scan history, comparisons, and trends |
| **Evidence Agent** | Performs RAG retrieval and evidence-grounded explanations |
| **Safety Agent** | Performs safety checks and referral logic |
| **Product Agent** | Handles skincare / product-category guidance |
| **Referral Agent** | Surfaces dermatologist options |
| **Orchestrator Agent** | Coordinates the overall workflow |

Splitting responsibilities this way lets each agent be developed, tested, and improved independently.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🔌 MCP Integration

SpectraDerm uses **Model Context Protocol (MCP)** as the standardized tool layer between agents and core capabilities:

```python
reconstruct_spectrum()
analyze_skin()
extract_features()
calculate_warning_score()
compare_scans()
retrieve_evidence()
get_skin_history()
find_dermatologists()
get_partner_products()
generate_report()
```

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 📄 Final Report

```mermaid
flowchart LR
    A["Image Analysis"] --> F["Final SpectraDerm<br/>Report"]
    B["Spectral Analysis"] --> F
    C["ML Score"] --> F
    D["Historical Change"] --> F
    E["RAG Evidence"] --> F
    G["Safety Assessment"] --> F
    H["Recommended Action"] --> F

    style F fill:#2f9e44,color:#fff
```

The goal is a structured monitoring result — not just a number on a screen.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🖥️ Application Flow

```mermaid
flowchart TD
    A["🏠 Home"] --> B["Start Scan"]
    B --> C["Upload / Capture Image"]
    C --> D["Analysis"]
    D --> E["Spectral Reconstruction"]
    E --> F["Skin Analysis"]
    F --> G["Result"]
    G --> H["Highlighted Region"]
    H --> I["Change / Monitoring Status"]
    I --> J["Why was this flagged?"]
    J --> K["Evidence-Grounded Explanation"]
    K --> L{"Next Action"}
    L --> M["Continue Monitoring"]
    L --> N["Consider Professional Assessment"]
    L --> O["Find Dermatologist"]

    style N fill:#e8590c,color:#fff
    style O fill:#e8590c,color:#fff
```

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🛠️ Technology Stack

**Frontend**

![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)

**Backend**

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![Pydantic](https://img.shields.io/badge/Pydantic-E92063?style=for-the-badge&logo=pydantic&logoColor=white)

**Computer Vision & Deep Learning**

![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![Pillow](https://img.shields.io/badge/Pillow-blue?style=for-the-badge)

**Data & Scientific Computing**

![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)

**Machine Learning**

![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)

**RAG**

![FastEmbed](https://img.shields.io/badge/FastEmbed-BAAI%2Fbge--small--en--v1.5-6f42c1?style=for-the-badge)

**Testing**

![Pytest](https://img.shields.io/badge/Pytest-0A9EDC?style=for-the-badge&logo=pytest&logoColor=white)
![Vitest](https://img.shields.io/badge/Vitest-6E9F18?style=for-the-badge&logo=vitest&logoColor=white)

<details>
<summary><strong>Full dependency list</strong></summary>

- **Agents & tools:** custom Python agent architecture, official Python MCP SDK
- **Storage:** local filesystem-backed JSON metadata, managed image artifacts, local NumPy embedding storage
- **External integration:** HTTPX, Google Places–oriented dermatologist referral provider

</details>

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 📂 Project Structure

<details>
<summary><strong>Click to expand the directory tree</strong></summary>

```text
SpectraDerm/
│
├── frontend/
│   ├── src/
│   ├── public/
│   └── package.json
│
├── backend/
│   ├── agents/
│   ├── api/
│   ├── ml/
│   ├── rag/
│   ├── mcp/
│   ├── storage/
│   ├── reporting/
│   └── tests/
│
├── models/
│   └── spectral/
│
├── data/
│   └── knowledge/
│
├── notebooks/
├── tests/
│
├── .env.example
├── requirements.txt
└── README.md
```

*The exact structure may vary depending on the current implementation.*

</details>

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## ⚙️ Environment Variables

Create a `.env` file based on the project's environment configuration, for example:

```env
GOOGLE_MAPS_API_KEY=your_google_maps_api_key
```

Additional variables may be required depending on which services and deployment environment you enable.

> [!WARNING]
> Never commit API keys or other secrets to Git.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🚀 Installation

<details open>
<summary><strong>1. Clone the repository</strong></summary>

```bash
git clone <repository-url>
cd SpectraDerm
```

</details>

<details open>
<summary><strong>2. Create a Python virtual environment</strong></summary>

```bash
python -m venv .venv
```

Activate it:

| OS | Command |
|---|---|
| Windows | `.venv\Scripts\activate` |
| Linux / macOS | `source .venv/bin/activate` |

</details>

<details open>
<summary><strong>3. Install backend dependencies</strong></summary>

```bash
pip install -r requirements.txt
```

</details>

<details open>
<summary><strong>4. Install frontend dependencies</strong></summary>

```bash
cd frontend
npm install
cd ..
```

</details>

<details open>
<summary><strong>5. Configure environment variables</strong></summary>

Create the `.env` file described above.

</details>

<details open>
<summary><strong>6. Start the backend</strong></summary>

```bash
uvicorn backend.main:app --reload
```

</details>

<details open>
<summary><strong>7. Start the frontend</strong></summary>

```bash
cd frontend
npm run dev
```

The frontend will then be available through the Vite development server.

</details>

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🧪 Testing

```bash
# Backend
pytest

# Frontend
npm test
```

Coverage spans image processing, ML functionality, RAG retrieval, agent behavior, safety logic, MCP tools, referral workflow, frontend components, and end-to-end integration.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🔐 Privacy and Safety

Because SpectraDerm processes skin images, privacy and responsible-use design are core, not an afterthought:

- Pseudonymous user IDs
- Minimum-necessary information
- Access controls
- Secure storage
- Consent handling
- Data deletion
- Safety checks before recommendations or referrals

> [!NOTE]
> SpectraDerm is **HIPAA-aware / HIPAA-inspired**, but does **not** claim legal HIPAA compliance. Actual compliance depends on the organization, deployment environment, data flows, and applicable jurisdiction.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## ⚠️ Limitations

1. **Estimated spectral information** — the reconstructed spectrum is a model prediction, not a physical measurement.
2. **Not a diagnostic system** — SpectraDerm does not diagnose skin diseases.
3. **Change score ≠ disease probability** — a high score means deviation from a reference pattern, nothing more.
4. **Medical significance isn't guaranteed** — a detected change may or may not be medically significant.
5. **Prototype status** — this is a capstone prototype, not a clinically validated medical device.
6. **Professional assessment matters** — concerning or persistent changes may warrant a dermatologist, not just this tool.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 🎯 Project Goals

SpectraDerm combines computer vision, spectral AI, feature engineering, machine learning, longitudinal monitoring, RAG, multi-agent AI, MCP, FastAPI, and React into one end-to-end workflow — exploring how far accessible smartphone imagery and modern AI can go for **skin monitoring and awareness**, without specialized imaging hardware, while clearly separating model-derived observations from medical diagnosis.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 👥 Team

**SpectraDerm — AI/ML Capstone Project**

| Name | Focus |
|---|---|
| **Zainab Fatima** | Data, Computer Vision & Spectral AI |
| **Ayesha Noor** | Machine Learning, RAG & AI Agents |
| **Javaria Akbar** | Backend, Frontend, Privacy & Testing |

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 📌 Disclaimer

> [!WARNING]
> SpectraDerm is an educational AI/ML capstone prototype for skin monitoring and awareness. It is **not medical advice, a medical diagnosis system, or a replacement for professional dermatological assessment.**
>
> If a skin change is persistent, concerning, rapidly changing, painful, bleeding, or otherwise worrying, seek appropriate professional medical advice.

<p align="right"><a href="#readme-top">back to top ↑</a></p>

## 📜 License

This project is currently intended for educational and research purposes. A project-specific license can be added here once finalized.

<div align="center">

---

Built as a collaborative end-to-end AI/ML capstone.

<a href="#readme-top">⬆ back to top</a>

</div>
