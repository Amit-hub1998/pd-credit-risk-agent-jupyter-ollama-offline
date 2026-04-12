PDHBC Credit Risk Agent
README — Repository Overview
Version 2.0  |  April 2026  |  HSBC Data Engineering
What Is This?
An intelligent debugging chatbot for the PDHBC Credit Risk Model. Finance teams raise queries about attribute values in the model output — null CRRs, missing local customer IDs, rating source discrepancies. This agent answers those queries automatically by combining:

•	RAG search over BRD and ERD pipeline documentation
•	Live data lookups across four source tables
•	A locally running LLM (Llama 3.1 8B via Ollama) — fully offline, no internet required

Without this agent: 1-3 hours per query, manual tracing through PySpark notebooks.
With this agent: under 1 minute per query, structured answer with root cause.


Repository Structure
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
├── README.docx                          ← this file
├── SETUP.docx                           ← step-by-step installation guide
├── ARCHITECTURE.docx                   ← system design document
└── PDHBC_Agent_Documentation.docx      ← full agent reference


Quick Start
1.	Install Ollama from https://ollama.com/download
2.	Run: ollama pull llama3.1:8b
3.	Run: ollama serve  (keep this terminal open)
4.	Create Python 3.9 virtual environment and install dependencies (see SETUP.docx)
5.	Open notebook/PDHBC_Ollama_Agent.ipynb in Jupyter
6.	Run all cells top to bottom
7.	In the last cell, call: debug_agent("your Finance query here");


Tech Stack
Component	Tool
LLM	Ollama + llama3.1:8b (fully local, offline)
LLM Framework	LangChain 0.3.12 + langchain-community
Vector Store	FAISS (faiss-cpu)
Embeddings	HuggingFace all-MiniLM-L6-v2 (CPU)
Data	Pandas + xlrd (Excel files)
Document Parsing	python-docx
Notebook	Jupyter Notebook
Python	3.9


Governance
This build was designed for Citrix environment where no external internet connections are permitted. Once Ollama and the model are downloaded, the agent operates entirely offline. No data is transmitted outside the local machine.


Documents in This Repo
File	Purpose
README.docx	This file — overview and quick start
SETUP.docx	Full step-by-step installation and environment setup guide including offline Ollama installation
ARCHITECTURE.docx	System design, data flow diagrams, component interactions
PDHBC_Agent_Documentation.docx	Complete agent reference — functions, pipeline logic, example queries, governance

PDHBC Credit Risk Agent  |  README v2.0  |  April 2026  |  Internal Use Only
