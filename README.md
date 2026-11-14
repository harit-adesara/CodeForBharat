# ⚖️ FastLegal

### An AI workbench for Indian litigation practice

Summarise judgments, research Indian law, draft pleadings, and track court matters, all in one place.

**🌐 Live app:** https://codeforbharat-3j0c.onrender.com
**📘 API docs:** https://codeforbharat-3j0c.onrender.com/api/docs

---

## **As of now use Gemini 3.1 flash lite model because it may possible that provider removed some of the models**

## 📖 Table of Contents

- [About](#-about)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Quick Start](#-quick-start)
- [Bring Your Own Key](#-bring-your-own-key-byok)
- [Supported AI Providers](#-supported-ai-providers)
- [API Overview](#-api-overview)
- [Deployment](#-deployment)
- [Project Structure](#-project-structure)
- [Roadmap](#-roadmap)
- [Disclaimer](#%EF%B8%8F-disclaimer)

---

## 🎯 About

Indian litigation involves long judgments, scattered statutes, strict timelines, and a lot of repetitive drafting. **FastLegal** brings the everyday parts of that workflow into a single web app powered by large language models, with live Indian Kanoon retrieval and a built-in library of Bare Acts.

It was built for the **Code for Bharat hackathon by Team Helper**.

**Highlights**

- 🧰 **10 purpose-built legal tools** in one interface
- 🔌 **Multi-provider AI:** Gemini, Groq, OpenAI, Hugging Face, Cloudflare Workers AI
- 🔑 **Bring-your-own-key:** no inference bill for the host, no key needed to boot
- 📤 **Export** results to DOCX, PDF, CSV, Markdown, or plain text
- 🌗 **Dark and light themes**, fully responsive
- ⚡ **No JS build step:** FastAPI + vanilla JavaScript

---

## ✨ Features

| Tool                       | What it does                                                                                                                           |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| 📅 **List of Dates**       | Extracts important dates from a case document and builds an editable chronology.                                                       |
| 📝 **Judgment Summariser** | Generates a structured brief: case, facts, issues, arguments, decision, ratio, and significance.                                       |
| 🔍 **Authority Check**     | Checks how a cited judgment has been treated and surfaces later cases that considered it.                                              |
| 🌐 **Translate & OCR**     | Extracts text from documents and scans, translates to English, and lets you chat with the document. Scans need a vision-capable model. |
| 📚 **Legal Research**      | Answers Indian-law questions with citations, using Indian Kanoon (when configured) or an uploaded judgment as the source.              |
| ✨ **Prompt Enhancer**     | Turns a rough question into a structured research prompt you can send straight to Legal Research.                                      |
| 📖 **Bare Acts**           | Searchable library of Indian statutes by Act and section, with amendment info and Q&A on provisions.                                   |
| ✍️ **Document Drafter**    | Drafts plaints, written statements, notices, applications, and arbitration pleadings.                                                  |
| 🛡️ **Counter Arguments**   | Extracts claims from an opposing pleading, drafts rebuttals, and assembles them into a written statement.                              |
| 🗓️ **Court Tracker**       | Tracks matters, hearings, outcomes, and next dates, with an **eCourts CNR lookup**.                                                    |

## 🛠 Tech Stack

| Layer            | Technology                                                                        |
| ---------------- | --------------------------------------------------------------------------------- |
| **Backend**      | Python 3.12, FastAPI, Uvicorn, Pydantic v2                                        |
| **Frontend**     | HTML, CSS, vanilla JavaScript (no build step)                                     |
| **AI providers** | Google Gemini, Groq, OpenAI (and compatible), Hugging Face, Cloudflare Workers AI |
| **Legal data**   | Indian Kanoon API, eCourts CNR service, bundled Bare Acts JSON                    |
| **Documents**    | PyMuPDF (PDF reading), python-docx (DOCX), ReportLab (PDF export)                 |
| **Storage**      | SQLite (default), PostgreSQL via `DATABASE_URL`                                   |
| **Deployment**   | Docker, Render blueprint, Procfile                                                |

---

## 🚀 Quick Start

### Prerequisites

- Python **3.12** (recommended)
- Git
- Internet access for AI providers and optional live legal-data services

> No `.env` file and no API key are required to start the app. You can add a key later from the in-app **API keys** panel.

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/CodeForBharat.git
cd CodeForBharat
```

### 2. Set up and run

**macOS / Linux**

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m uvicorn app.main:app --reload --port 8000
```

**Windows (PowerShell)**

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m uvicorn app.main:app --reload --port 8000
```

If activation is blocked, run this once in the same terminal and activate again:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

**Docker**

```bash
docker build -t fastlegal .
docker run --rm -p 8000:8000 fastlegal
```

To pass a server-side key:

```bash
docker run --rm -p 8000:8000 -e GEMINI_API_KEY=your_key fastlegal
```

### 3. Open the app

|                     | URL                              |
| ------------------- | -------------------------------- |
| 🖥️ App              | http://127.0.0.1:8000            |
| 📘 Swagger API docs | http://127.0.0.1:8000/api/docs   |
| ❤️ Health check     | http://127.0.0.1:8000/api/health |

---

## ⚙️ Configuration

Create a `.env` file in the project root **only if** you want server-side defaults:

```env
GEMINI_API_KEY=your_gemini_api_key
INDIAN_KANOON_API_TOKEN=your_kanoon_token
ALLOW_CLIENT_KEYS=1
```

> 🔒 Keep `.env` private and never commit it. Don't wrap values in quotes.

| Variable                            |    Required     | Description                                                           |
| ----------------------------------- | :-------------: | --------------------------------------------------------------------- |
| `GEMINI_API_KEY` / `GOOGLE_API_KEY` |       No        | Google Gemini key.                                                    |
| `GROQ_API_KEY`                      |       No        | Groq key.                                                             |
| `OPENAI_API_KEY`                    |       No        | OpenAI or compatible-provider key.                                    |
| `OPENAI_BASE_URL`                   |       No        | Custom OpenAI-compatible endpoint (default: OpenAI).                  |
| `HUGGINGFACE_API_KEY` / `HF_TOKEN`  |       No        | Hugging Face key.                                                     |
| `CLOUDFLARE_API_TOKEN`              |       No        | Cloudflare Workers AI token.                                          |
| `CLOUDFLARE_ACCOUNT_ID`             | With Cloudflare | Required alongside the Cloudflare token.                              |
| `INDIAN_KANOON_API_TOKEN`           |       No        | Enables live Indian Kanoon case-law retrieval.                        |
| `ALLOW_CLIENT_KEYS`                 |       No        | `1` (default) lets users enter their own keys; `0` disables it.       |
| `DATABASE_URL`                      |       No        | PostgreSQL URL for persistent tracker data. SQLite is used otherwise. |
| `DEMO_SEED`                         |       No        | `1` seeds sample matters into an empty tracker.                       |
| `MAX_UPLOAD_MB`                     |       No        | Max upload size (default `25`).                                       |
| `MAX_OCR_PAGES`                     |       No        | Max pages processed for OCR (default `8`).                            |
| `REQUEST_TIMEOUT`                   |       No        | Upstream request timeout in seconds (default `180`).                  |
| `CORS_ORIGINS`                      |       No        | Comma-separated allowed origins.                                      |
| `DEBUG`                             |       No        | Enables debug logging.                                                |

Optional per-provider overrides are also supported, e.g. `GEMINI_MODEL`, `GROQ_MODEL`, and `<PROVIDER>_EXTRA_MODELS` to add models to the picker.

---

## 🔑 Bring Your Own Key (BYOK)

FastLegal can be hosted publicly **without paying for anyone's inference**:

1. The server boots with **no provider key**.
2. Each visitor opens **API keys** in the app and pastes their own provider key (and optionally an Indian Kanoon token).
3. The key is sent with that user's requests only and is not intended to be stored in the application database.

Running privately with only your own keys? Set `ALLOW_CLIENT_KEYS=0`.

> ⚠️ Treat API keys as secrets and always use a trusted HTTPS deployment.

---

## 🤖 Supported AI Providers

| Provider                  | Example models                                 | Vision (scans/OCR) |
| ------------------------- | ---------------------------------------------- | :----------------: |
| **Google Gemini**         | Gemini 2.5 Flash / Flash-Lite / Pro, 2.0 Flash |         ✅         |
| **Groq**                  | Llama 3.3 70B Versatile and others             |  Model dependent   |
| **OpenAI**                | GPT-4o, GPT-4o mini, GPT-4.1, GPT-4.1 mini     |         ✅         |
| **Hugging Face**          | Open models via the HF router                  |  Model dependent   |
| **Cloudflare Workers AI** | Hosted open models                             |  Model dependent   |

Switch providers and models any time from the **model picker**. Scanned documents require a vision-capable model.

---

## 🔌 API Overview

Interactive docs live at `/api/docs`. Main endpoint groups:

| Group             | Endpoints                                                                                                                                    |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **System**        | `GET /api/health`, `GET /api/config`, `POST /api/providers/verify`                                                                           |
| **Documents**     | `POST /api/extract`, `POST /api/tools/translate`, `POST /api/tools/document-chat`                                                            |
| **Analysis**      | `POST /api/tools/list-of-dates`, `POST /api/tools/summarise`, `POST /api/tools/authority-check`                                              |
| **Research**      | `POST /api/tools/research`, `GET /api/tools/kanoon/search`, `POST /api/tools/enhance-prompt`                                                 |
| **Drafting**      | `POST /api/tools/draft`, `POST /api/tools/counter/{claims,draft,draft-all,assemble,ask}`                                                     |
| **Bare Acts**     | `GET /api/acts`, `GET /api/acts/search`, `GET /api/acts/detail`, `POST /api/acts/ask`                                                        |
| **Court Tracker** | `GET/POST /api/cases`, `PATCH/DELETE /api/cases/{id}`, `POST /api/cases/{id}/hearings`, `GET /api/cases/upcoming`, `POST /api/cases/ecourts` |
| **Export**        | `POST /api/export`, `POST /api/export/raw`                                                                                                   |

---

## ☁️ Deployment

### Render (recommended)

The repo ships with a Render blueprint (`render.yaml`).

1. Push the repo to GitHub.
2. In Render: **New → Blueprint** and select the repository.
3. Deploy. No provider key is required thanks to BYOK.

### Docker / any container host

Build from the included `Dockerfile`. The container exposes port `8000`, honours `$PORT`, and ships a health check at `/api/health`.

### Procfile / shell hosts

```bash
./start.sh
```

> 💾 **Persistence note:** the default SQLite tracker lives on local disk. On free or ephemeral hosts, data is lost on restart or redeploy. Set `DATABASE_URL` to a PostgreSQL connection string to keep case data permanently.

---

## 📁 Project Structure

```text
CodeForBharat/
├── app/
│   ├── main.py             # FastAPI entry point, serves API + frontend
│   ├── settings.py         # Environment-driven configuration
│   ├── db.py               # SQLite / PostgreSQL storage layer
│   ├── schemas.py          # Pydantic request/response models
│   ├── api/
│   │   ├── routes.py       # All tool and tracker endpoints
│   │   └── byok.py         # Per-request user-supplied keys
│   ├── llm/
│   │   ├── providers.py    # Gemini, Groq, OpenAI, HF, Cloudflare adapters
│   │   ├── registry.py     # Provider/model selection and vision checks
│   │   ├── keyring.py      # Request-scoped key handling
│   │   └── base.py         # Shared LLM helpers
│   └── services/
│       ├── analysis.py     # Dates, summaries, authority checks
│       ├── research.py     # Legal research pipeline
│       ├── kanoon.py       # Indian Kanoon API client
│       ├── drafting.py     # Document drafting + counter arguments
│       ├── bare_acts.py    # Statute search and Q&A
│       ├── tracker.py      # Case tracker + eCourts CNR lookup
│       ├── documents.py    # PDF / image / text extraction
│       ├── exports.py      # DOCX, PDF, CSV, MD, TXT export
│       └── seed.py         # Demo data
├── web/                    # Frontend (index.html, app.js, styles.css)
├── data/                   # Bare Acts JSON + local SQLite database
├── docs/                   # Demo media
├── Dockerfile
├── render.yaml
├── Procfile
├── start.sh
└── requirements.txt
```

---

## 🗺 Roadmap

- [ ] Hindi and other Indian-language output
- [ ] More Bare Acts and state-level statutes
- [ ] Hearing reminders (email / WhatsApp)
- [ ] User accounts and shared workspaces
- [ ] Automated tests and CI

---

## ⚠️ Disclaimer

FastLegal is a legal-work **assistant**, not a substitute for professional legal judgment. AI-generated research, summaries, and drafts can be incomplete or inaccurate. **Always verify facts, citations, and authorities before relying on or filing any output.** Do not upload confidential client material to a deployment unless you are authorised to do so.

---

Built with ❤️ by **Team Helper** for **Code for Bharat**
