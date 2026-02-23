# KnowledgeGraph-RAG

## 🚀 Project Overview

**KnowledgeGraph-RAG** is an advanced, open-source system for Retrieval-Augmented Generation (RAG) using knowledge graphs and Large Language Models (LLMs). By extracting structured knowledge from PDFs and scientific papers, it enables explainable, multilingual, and context-aware question-answering for research and enterprise use.

- **Value Proposition:**  
  Move beyond vector-only RAG: leverage explicit relationships between concepts, entities, and documents for richer, more explainable answers. Multilingual support and exportable history make it perfect for research, education, and enterprise knowledge management.

---

## 🧠 Conceptual Background

### What is a Knowledge Graph?

A **knowledge graph** is a network of real-world entities (like people, places, or concepts) and their relationships. Imagine a mind map: each node is an entity, each edge a relationship (e.g., "Paper A uses Method B"). Knowledge graphs enable machines to reason about context and connections, not just keywords.

### Retrieval-Augmented Generation (RAG)

RAG systems combine information retrieval (finding relevant facts) with generation (LLMs composing answers). Instead of generating from scratch, the LLM is "grounded" by retrieved data—here, from both vectors *and* the knowledge graph.

### Agents & Tools

- **Agents**: Specialized LLM-powered modules that orchestrate retrieval, entity linking, and answer synthesis.
- **Tools**: Modular components—PDF loaders, entity extractors, graph search, etc.—that agents can call to complete queries.

### PDF-Graph-Based RAG

Instead of treating each PDF as a "bag of words," this system:
1. Splits PDFs into chunks (paragraphs, sections).
2. Extracts entities (methods, datasets, concepts) using NER (Named Entity Recognition).
3. Builds a relational graph: chunks ↔ entities ↔ documents.
4. At query time, retrieves not just similar text, but also related entities & their context.

**This approach provides more explainable, connected answers than plain vector search.**

---

## 🏛️ Architecture

- **Frontend (TypeScript/React/Streamlit)**:  
  - Chat interface for querying the knowledge graph.
- **Backend (Python/FastAPI/Neo4j/LLM)**:  
  - PDF ingestion, chunking, and entity extraction.
  - Knowledge graph creation and Cypher querying.
  - Agent and tool orchestration for advanced QA.
  - Secure user management and history storage.
- **Integrations**:
  - **LLMs**: OpenAI, Together AI, or local models (environment-configurable).
  - **Neo4j**: Graph database for entity storage and query.
  - **Vector Store**: For semantic retrieval.
  - **PDF Parsing**: Robust chunking for scientific docs.

**Environment Variables**:  
- `TOGETHER_API_KEY` (LLM access, for mistralai/Mistral-7B-Instruct-v0.2)  
- `OPENAI_API_KEY` (optional)  
- `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD` (graph DB)  

---

## 🗂️ Repository Structure

```
KnowledgeGraph-RAG/
│
├── frontend/                # React frontend (UI, API calls, visualization)
│   ├── src/
│   │   ├── components/      # React components
│   │   ├── hooks/           # Custom React hooks
│   │   ├── lib/             # API utilities
│   │   ├── App.tsx          # Main app entry
│   │   └── ...              
│   ├── public/              # Static assets
│   ├── index.html           # HTML entry point
│   └── ...                  
│
├── KG_RAG_backend/          # Python backend (FastAPI, Neo4j, LLM)
│   ├── main.py              # FastAPI app entry
│   ├── modules/             # Core modules (KnowledgeGraph, tools, etc.)
│   ├── routers/             # API routers
│   ├── data/                # Data storage (PDFs, entities, graphs)
│   ├── db/                  # Database utilities
│   ├── auth/                # Authentication logic
│   ├── utils/               # Utility functions
│   └── ...                  
│
├── docker-compose.yml       # Docker orchestration
├── requirements.txt         # Python dependencies
├── README.md                # Project documentation
└── ...
```

---

## 🛠️ Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/DhruvJ2k4/KnowledgeGraph-RAG.git
   cd KnowledgeGraph-RAG
   ```

2. **Setup Python environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

3. **Frontend (optional):**
   ```bash
   cd frontend
   npm install
   npm run build
   # or for development
   npm run dev
   ```

4. **Configure environment variables:**
   - Copy `.env.example` to `.env` and set required keys.

---

## Usage

- **Upload PDFs:**  
  Use the web interface or API `/upload` endpoint.

- **Build the Knowledge Graph:**  
  Click "Build KG" or use the backend API.

- **Chat/Query Examples:**
  - "Show me all datasets related to BERT."
  - "What methods are used in Document 3, Page 5?"
  - "Find papers connecting 'contrastive learning' and 'graph neural networks'."

---

## 📖 API Reference

### Backend Endpoints

- **PDF Upload:** `POST /data-loader/upload`
- **List PDFs:** `GET /data-loader/list`
- **Delete PDF:** `DELETE /data-loader/delete/{fileId}`
- **PDF Status:** `GET /data-loader/status/{fileId}`
- **Process PDFs:** `POST /KG-status/pdf-status`
- **Extract Entities:** `POST /KG-status/entity-extractor`
- **Build Knowledge Graph:** `POST /KG-status/build-kg`
- **Update Knowledge Graph:** `POST /KG-status/update-kg`
- **Delete PDF Status:** `DELETE /KG-status/pdf-status`
- **Get KG Status:** `GET /KG-status/status`

See [frontend/src/lib/api.ts](frontend/src/lib/api.ts) for TypeScript API client usage.

---

### Data Models

- **PDFFile**:  
  See [`PDFFile`](frontend/src/types/index.ts) for structure.
- **KnowledgeGraphStatus**:  
  See [`KnowledgeGraphStatus`](frontend/src/types/index.ts) for status fields.
- **GraphNode**:  
  See [`GraphNode`](frontend/src/types/index.ts) for node structure.

---

## Supported Features

- PDF ingestion & chunking
- Custom Named Entity Recognition (NER)
- Knowledge graph creation and Cypher querying
- Hybrid retrieval (vector + graph)
- User authentication & history
- Dockerized deployment
- LLM-augmented agents and tools

---
## 🐳 Docker Compose Setup

This project provides a ready-to-use `docker-compose.yml` for local development and testing with PostgreSQL and pgAdmin.  
**We recommend including this file in your repository for easy onboarding and reproducibility.**

### Services

- **Postgres**  
  - Image: `postgres:latest`
  - User: `kgrag_user`
  - Password: `kgrag_password`
  - Database: `kgrag_db`
  - Port: `5433` (host) → `5432` (container)
  - Persistent volume: `postgres_data`
- **pgAdmin**  
  - Image: `dpage/pgadmin4:latest`
  - Default email: `admin@admin.com`
  - Default password: `admin`
  - Port: `5051` (host) → `80` (container)
  - Depends on Postgres

### Usage

1. **Start the stack:**
   ```bash
   docker-compose up -d
   ```

2. **Access pgAdmin:**  
   Open [http://localhost:5051](http://localhost:5051)  
   Login with the default credentials above.

3. **Connect to Postgres:**  
   - Host: `postgres`
   - Port: `5432`
   - User: `kgrag_user`
   - Password: `kgrag_password`
   - Database: `kgrag_db`

4. **Stop the stack:**
   ```bash
   docker-compose down
   ```

### Networks & Volumes

- **Network:** `kgrag_network` (isolated bridge)
- **Volume:** `postgres_data` (persists database data)

---

## 💡 Environment Variables

| Variable             | Description                  |
|----------------------|-----------------------------|
| `TOGETHER_API_KEY`   | API key for LLM inference   |
| `NEO4J_URI`          | Neo4j database URI          |
| `NEO4J_USERNAME`     | Neo4j username              |
| `NEO4J_PASSWORD`     | Neo4j password              |

## 📜 License

Open-source under the [MIT License](LICENSE).

## 👤 Author

- [Haswanth S](https://github.com/HaswanthVIT)
