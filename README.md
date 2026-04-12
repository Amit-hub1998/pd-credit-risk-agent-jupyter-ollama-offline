# PD Credit Risk Agent
### Intelligent Debugging Chatbot — Offline Build (Ollama + Llama 3.1 8B)

> Built to eliminate 1–3 hours of manual DA/BA investigation per Finance query.  
> Runs entirely inside Jupyter — no external infrastructure, no internet required at runtime.  
> Designed for eventual deployment on HSBC's Citrix/Hadoop environment.

---

## What Is This?

An intelligent debugging chatbot for the PDHBC Credit Risk Model at HSBC. Finance teams raise queries about attribute values in the model output — null CRRs, missing local customer IDs, rating source discrepancies. This agent answers those queries automatically by combining:

- **RAG search** over BRD and ERD pipeline documentation (FAISS + HuggingFace embeddings)
- **Live data lookups** across four source tables (Pandas)
- **A locally running LLM** — Llama 3.1 8B via Ollama — fully offline, no internet required

| Without Agent | With Agent |
|---|---|
| 1–3 hours manual tracing through PySpark notebooks | Under 1 minute, structured answer with root cause |

---

## Repository Structure

```
pdhbc-credit-risk-agent-jupyter-internal/
├── notebook/
│   └── PDHBC_Ollama_Agent.ipynb        ← main agent notebook
├── data/
│   ├── credit_applications.xls
│   ├── basic_details.xls
│   ├── multi_ratings.xls
│   └── downstream.xls
├── docs/
│   ├── BRD_CreditRiskPipeline.docx
│   └── ERD_CreditRiskPipeline.docx
├── faiss_index/                         ← auto-generated on first run
├── README.md                            ← this file
├── SETUP.docx                           ← step-by-step installation guide
├── ARCHITECTURE.docx                    ← system design document
└── PDHBC_Agent_Documentation.docx      ← full agent reference
```

---

## Quick Start

1. Install Ollama from https://ollama.com/download
2. Pull the model (one-time, ~5 GB):
```bash
ollama pull llama3.1:8b
```
3. Start the Ollama server (keep this terminal open):
```bash
ollama serve
```
4. Create Python 3.9 virtual environment and install dependencies:
```bash
python -m venv venv
.\venv\Scripts\activate
pip install langchain==0.3.12 langchain-community faiss-cpu sentence-transformers jupyter python-docx ollama pandas xlrd openpyxl
```
5. Launch Jupyter and open `notebook/PDHBC_Ollama_Agent.ipynb`
```bash
jupyter notebook
```
6. Run all cells top to bottom, then fire a query:
```python
debug_agent("Why is final_crr null for relationship 1000101?");
```

---

## Tech Stack

| Component | Tool |
|---|---|
| LLM | Ollama + llama3.1:8b (fully local, offline) |
| LLM Framework | LangChain 0.3.12 + langchain-community |
| Vector Store | FAISS (faiss-cpu) |
| Embeddings | HuggingFace all-MiniLM-L6-v2 (CPU) |
| Data | Pandas + xlrd (Excel files) |
| Document Parsing | python-docx |
| Notebook | Jupyter Notebook |
| Python | 3.9 |

---

## Example Queries

```python
# Null CRR investigation
debug_agent("Why is final_crr null for relationship 1000101?");

# Reverse lookup from local customer ID
debug_agent("For LCL-IN-5530 what is the latest credit proposal that satisfies the base filters?");

# Missing local customer ID
debug_agent("Why is local_customer_id blank for relation 1000305?");

# Pipeline logic question
debug_agent("Is there a chance we drop a record because final_crr is NULL?");

# General documentation query
debug_agent("How is final_crr calculated in the pipeline?");
```

---

## Source Tables

| Table | Join Type | Description |
|---|---|---|
| `credit_applications` | Base / Filtered | Credit proposals — anchor table |
| `basic_details` | INNER JOIN | Baseline risk ratings — fallback source |
| `multi_ratings` | LEFT JOIN | Primary risk ratings — preferred source |
| `downstream` | LEFT JOIN | Local customer ID mapping |

---

## Governance

This build was designed for HSBC's Citrix environment where no external internet connections are permitted.

- ✅ No data leaves the local machine
- ✅ No external API calls at runtime
- ✅ Ollama runs entirely on `localhost:11434`
- ✅ FAISS runs in-process — no network calls
- ✅ HuggingFace embeddings run on CPU locally
- ✅ Internet only needed once — for the initial `ollama pull`

---

## Build Versions

| Version | LLM | Internet Required | Status |
|---|---|---|---|
| v1 — Groq Prototype | Groq API (llama-3.3-70b-versatile) | YES — data sent to Groq cloud | Complete. Blocked from production by governance. |
| v2 — Ollama Offline | Ollama local (llama3.1:8b) | NO — fully air-gapped | ✅ Active. Production-path build. |

---

## Documentation

| File | Purpose |
|---|---|
| `README.md` | This file — overview and quick start |
| `SETUP.docx` | Full installation guide including offline Citrix deployment |
| `ARCHITECTURE.docx` | System design, data flow, component interactions |
| `PDHBC_Agent_Documentation.docx` | Complete agent reference — functions, pipeline logic, example queries |

---

## Topics

`llm` `rag` `langchain` `ollama` `faiss` `credit-risk` `jupyter` `offline-ai` `python`

---

*PDHBC Credit Risk Agent | v2.0 | April 2026 | HSBC Data Engineering | Internal Use Only*
