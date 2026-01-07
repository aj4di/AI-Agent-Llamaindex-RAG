
<p align="left">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-green">
  <img alt="Python" src="https://img.shields.io/badge/python-3.11+-blue">
  <a href="https://github.com/USER/REPO/actions/workflows/docker-image.yml">
    <img alt="Docker CI" src="https://github.com/USER/REPO/actions/workflows/docker-image.yml/badge.svg">
  </a>
</p>


<p align="left">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-green">
  <img alt="Python" src="https://img.shields.io/badge/python-3.11+-blue">
  <img alt="CI" src="https://img.shields.io/badge/CI-GitHub%20Actions-blueviolet">
</p>

# CareGraph — Multi-Agent Healthcare RAG (LlamaIndex)

A portfolio-ready demo showing multi-agent orchestration with **LlamaIndex**, grounded retrieval over synthetic healthcare data (ICD-10/CPT, plan rules), and explainable outputs via a **Why Card** JSON.

## Features
- LlamaIndex agents (Eligibility, PriorAuth, Provider, Summarizer)
- Hybrid RAG over policy docs + synthetic notes
- Tool use for ICD-10/CPT, mock FHIR coverage, provider directory
- **Local LLM** (llama.cpp via GGUF) or OpenAI fallback
- Prometheus metrics + **Grafana dashboard**
- Streamlit demo UI (pretty, tabbed)
- Eval harness to check grounding + JSON schema validity

## Quickstart (Local Python)
```bash
python -m venv .venv && source .venv/bin/activate
pip install -r docker/requirements.txt

# (Optional) Use a local GGUF model:
export LLAMA_CPP_MODEL_PATH=./models/<your-model>.gguf

# Build index
python -c "from app.rag.index_builder import build_policy_index; build_policy_index()"

# Run UI
streamlit run app/ui.py
```

## Quickstart (Docker Compose)
```bash
# Put a GGUF in models/ and set the path in docker/docker-compose.yml
docker compose -f docker/docker-compose.yml up --build
```
Then open:
- UI: http://localhost:8501
- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000  (dashboard auto-provisioned; default creds admin/admin)

## Project Layout
```
caregraph/
  app/
    orchestrator.py
    ui.py
    llm.py
    metrics.py
    agents/
    tools/
    rag/
  data/ (synthetic)
  eval/
  docker/ (compose + prometheus + grafana)
  models/ (your GGUF goes here)
```

## Notes
- Data is synthetic and safe to publish.
- If no local model is set, the app falls back to OpenAI (requires `OPENAI_API_KEY`). Do not commit keys.
- This is a demo; do not use for clinical decisions.


##Project Description 

**CareGraph** is a demo of prototype how to combine multi-agent orchestration, Retrieval-Augmented Generation (RAG)

- **Multi-Agent System** — Specialized agents (Eligibility-, PriorAuth-, Provider-, Summarizer - agent) coordinate flow using role-specific tools.
- **RAG over Structured & Unstructured Data** — Combines ICD-10/CPT tables, synthetic FHIR coverage JSON, and Markdown/PDF plan rules.
- **Explainability via Why Card** — Every answer is backed by a JSON card listing decisions, criteria, and source snippets.
- **Local & Cloud LLMs** — Works with llama.cpp GGUF models (local) or OpenAI APIs (fallback).
- **MLOps & Observability** — Exports metrics to Prometheus, ships with a Grafana dashboard.
- **UI for Engagement** — Streamlit web UI with modern layout, latency badges, tabs for Why Card / sources / raw output.

This project is safe for public demonstration because it uses **synthetic data only**.


Additional feature: 
## Fine‑tune a tiny model (QLoRA)


**Notes**
- Change `BASE_MODEL` env var to switch model (e.g., `meta-llama/Llama-3.2-3B-Instruct`).
