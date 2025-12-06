# RAG Enterprise Core

Enterprise-grade Retrieval-Augmented Generation system with microservices architecture.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.12+](https://img.shields.io/badge/python-3.12+-blue.svg)](https://www.python.org/downloads/)
[![Docker](https://img.shields.io/badge/docker-ready-brightgreen.svg)](https://www.docker.com/)

## 🎯 Features

- 🤖 **LangGraph Orchestration** - Intelligent query routing and agentic workflows
- 🗄️ **ChromaDB Vector Store** - Persistent semantic search
- 📄 **Docling Integration** - High-quality PDF processing
- 💬 **Conversation Memory** - Redis-based session management
- 🔄 **Resilience Patterns** - Retry, circuit breaker, rate limiting
- 📊 **Full Observability** - Prometheus + Grafana monitoring
- 🐳 **Docker Ready** - Complete containerized deployment

## 📋 Architecture

```
┌─────────────┐
│   Agent     │ ← LangGraph Orchestration + Redis Memory
└──────┬──────┘
       │
       ├─────────────────┐
       │                 │
┌──────▼──────┐   ┌─────▼─────┐
│  Knowledge  │   │  Ingest   │
│  (ChromaDB) │   │ (Docling) │
└─────────────┘   └───────────┘
```

**[Full Architecture Documentation →](docs/architecture.md)**

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Ollama

### 1. Setup

```bash
cd /mnt/dev/eingang/rag_enterprise_core
cp .env.example .env
# No API keys needed - uses Ollama by default
# (Only edit .env if you want to use OpenAI instead)
```

### 2. Start All Services

```bash
docker-compose up -d
```

### 3. Verify Health

```bash
curl http://localhost:8003/health  # Agent
curl http://localhost:8002/health  # Knowledge
curl http://localhost:8001/health  # Ingest
```

### 4. Test Chat

```bash
curl -X POST http://localhost:8003/chat \
  -H "Content-Type: application/json" \
  -d '{"query": "Hello, how are you?"}'
```

## 📚 Services

| Service 		| Port | Description 				| Docs 										|
|---------------|------|----------------------------|-------------------------------------------|
| **Agent** 	| 8003 | LangGraph orchestration	| [README](services/agent/README.md) 		|
| **Knowledge** | 8002 | ChromaDB vector search 	| [README](services/knowledge/README.md) 	|
| **Ingest** 	| 8001 | Docling document processing| [README](services/ingest/README.md) 		|
| **Redis** 	| 6379 | Conversation memory 		| - 										|
| **Prometheus**| 9090 | Metrics collection 		| - 										|
| **Grafana** 	| 3000 | Dashboards 				| - 										|


## 📖 API Documentation

Once services are running:

- **Agent API:** http://localhost:8003/docs
- **Knowledge API:** http://localhost:8002/docs
- **Ingest API:** http://localhost:8001/docs


## 🔧 Configuration

### Environment Variables

```bash
# All settings are optional - Ollama is used by default
# No API keys required for default setup

# Optional: Use OpenAI instead of Ollama
# OPENAI_API_KEY=your_openai_api_key
# LLM_PROVIDER=openai

# Service URLs (defaults shown)
KNOWLEDGE_SERVICE_URL=http://knowledge:8000
REDIS_URL=redis://redis:6379
```

## 💻 Development

### Install Service

```bash
cd services/agent  # or knowledge, ingest
pip install -e .
```

### Run Service Locally

```bash
uvicorn rag_enterprise_agent.service:app --reload --port 8003
```

### Run Tests

```bash
pytest tests/ -v --cov
```

## 📊 Monitoring

- **Grafana**: http://localhost:3000 (admin/admin)
- **Prometheus**: http://localhost:9090
- **Metrics**: http://localhost:8003/metrics

## 🛡️ Resilience Features

- ✅ **Retry Logic** 		- Exponential backoff (3 attempts)
- ✅ **Circuit Breaker** 	- Fail-fast after 5 failures
- ✅ **Rate Limiting** 		- 10 req/min per IP (Agent)
- ✅ **Graceful Shutdown** 	- Clean connection closure
- ✅ **Health Checks** 		- Docker health probes

## 🧪 Testing

```bash
# Unit tests
pytest services/agent/tests/unit -v

# Integration tests
pytest tests/integration -v

# Coverage report
pytest --cov --cov-report=html
```

## 📦 Project Structure

```
rag_enterprise_core/
├── services/
│   ├── agent/          # LangGraph orchestration
│   ├── knowledge/      # ChromaDB vector store
│   └── ingest/         # Docling processing
├── shared/             # Shared models & utilities
├── infrastructure/     # Prometheus & Grafana configs
├── docs/               # Documentation
├── tests/              # Integration tests
└── docker-compose.yml
```

## 🔄 Workflow

### 1. Ingest Documents

```bash
curl -X POST http://localhost:8001/ingest \
  -F "file=@document.pdf" \
  -F "collection_name=my_docs"
```

### 2. Chat with RAG

```bash
curl -X POST http://localhost:8003/chat \
  -H "Content-Type: application/json" \
  -d '{
    "query": "What is in the document?",
    "session_id": "optional-uuid"
  }'
```

### 3. View Session History

```bash
curl http://localhost:8003/sessions/{session_id}
```

## 🎓 Documentation

- [Architecture](docs/architecture.md) 				- System design and diagrams
- [Agent Service](services/agent/README.md) 		- LangGraph orchestration
- [Knowledge Service](services/knowledge/README.md) - Vector search
- [Ingest Service](services/ingest/README.md) 		- Document processing

## 🚧 Roadmap
-
