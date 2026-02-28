# Document Portal - AI-Powered Document Intelligence Platform

> An intelligent document processing and chat system built with **LangChain**, **FAISS**, and **LLMs** for conversational document analysis, comparison, and question-answering.

## 🎯 Overview

**Document Portal** is a FastAPI-based backend service that enables intelligent document processing through three core capabilities:

1. **Document Analysis** – Extract structured metadata, summaries, and insights from documents
2. **Document Comparison** – Semantically compare two documents to identify differences and similarities
3. **Conversational Chat** – Ask questions about documents with context-aware responses using RAG

The system is production-ready with proper error handling, logging, session management, and Docker deployment support.

---

## ✨ Key Features

- **📤 Multi-format Document Support** – PDF, DOCX, TXT files
- **🔍 RAG-Based Retrieval** – FAISS vector store with semantic search
- **💬 Conversational Memory** – Maintains chat history and context across turns
- **📊 Document Analysis** – Automatic extraction of metadata and summaries
- **🔀 Document Comparison** – LLM-powered semantic document diffing
- **📝 Session Management** – Persistent storage of chat sessions and indices
- **🛠️ Modular Architecture** – Clean separation of concerns for easy extension
- **📋 Structured Logging** – StructLog integration for production-grade monitoring
- **🐳 Docker Ready** – Dockerfile included for containerized deployment
- **✅ Test Coverage** – Pytest integration for quality assurance

---

## 🏗️ Architecture

### System Design

```
User Interface (HTML/Templates)
         ↓
   FastAPI Backend
  ┌─────────────────────┬──────────────────┬────────────────────┐
  ↓                     ↓                  ↓                    ↓
/analyze         /compare            /chat/index          /chat/query
DocumentAnalyzer  DocumentComparator  ChatIngestor       ConversationalRAG
  ↓                ↓                  ↓                    ↓
LLM + Prompts    LLM + Prompts      RecursiveCharacter   FAISS Retriever
                                    TextSplitter + LLM    + LLM

                    Model Loader (Groq/Google)
                     Embedding Model (Google)
```

### Core Components

| Component | Location | Purpose |
|-----------|----------|---------|
| **FastAPI Server** | `api/main.py` | REST API endpoints and UI serving |
| **Document Ingestion** | `src/document_ingestion/data_ingestion.py` | File parsing, chunking, FAISS indexing |
| **Document Analyzer** | `src/document_analyzer/data_analysis.py` | Metadata extraction and summarization |
| **Document Comparator** | `src/document_compare/document_comparator.py` | Semantic document comparison |
| **Conversational RAG** | `src/document_chat/retrieval.py` | Chat with documents using LCEL chains |
| **Model Loader** | `utils/model_loader.py` | LLM and embedding model initialization |
| **Logger** | `logger/custom_logger.py` | StructLog-based logging service |
| **Exception Handler** | `exception/custom_exception.py` | Custom error definitions |

---

## 🔧 Tech Stack

### Core Dependencies
- **FastAPI** `0.116.1` – Async web framework
- **LangChain** `0.3.27` – LLM orchestration and RAG framework
- **FAISS** `1.11.0` – Vector similarity search
- **LLM Providers:**
  - **Groq** – `langchain-groq` (DeepSeek-R1 Distill LLaMA 70B)
  - **Google** – `langchain-google-genai` (Gemini 2.5 Flash)
- **Embeddings** – Google Text Embedding 004
- **Document Processing:**
  - **PyMuPDF** `1.26.3` – PDF handling
  - **python-docx** `0.9` – DOCX support
- **Server** – Uvicorn `0.35.0`
- **Logging** – StructLog `25.4.0`
- **UI** – Jinja2 templates + static assets

### Development Tools
- **Testing** – Pytest `8.4.1`
- **Environment** – Python-dotenv `1.1.1`

---

## 📁 Project Structure

```
doc-ai-toolkit/
├── api/
│   ├── main.py                 # FastAPI app with 6 endpoints
│   └── __init__.py
├── src/
│   ├── document_ingestion/    # File parsing & FAISS indexing
│   │   ├── data_ingestion.py
│   │   └── __init__.py
│   ├── document_analyzer/     # Document analysis & metadata extraction
│   │   ├── data_analysis.py
│   │   └── __init__.py
│   ├── document_compare/      # Semantic document comparison
│   │   ├── document_comparator.py
│   │   └── __init__.py
│   └── document_chat/         # RAG-based conversational chat
│       ├── retrieval.py
│       └── __init__.py
├── utils/
│   ├── model_loader.py        # LLM & embedding model initialization
│   ├── config_loader.py       # YAML configuration loader
│   ├── document_ops.py        # Document processing utilities
│   ├── file_io.py             # File I/O and session handling
│   └── __init__.py
├── logger/
│   ├── custom_logger.py       # StructLog logger setup
│   └── __init__.py
├── exception/
│   ├── custom_exception.py    # Custom exception definitions
│   └── __init__.py
├── prompt/
│   └── prompt_library.py      # Prompt templates registry
├── model/
│   └── models.py              # Pydantic models for structured output
├── config/
│   └── config.yaml            # LLM, embedding, and retriever config
├── static/                    # CSS, JavaScript assets
├── templates/                 # HTML templates
├── tests/                     # Test files
├── data/                      # Upload directory for documents
├── faiss_index/               # FAISS vector indices
├── logs/                      # Application logs
├── requirements.txt
├── setup.py
├── Dockerfile
├── pytest.ini
└── README.md
```

---

## 🚀 Installation & Setup

### Prerequisites
- Python 3.10+
- pip or conda

### 1. Clone Repository
```bash
git clone <repository-url>
cd doc-ai-toolkit
```

### 2. Create Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment
Create a `.env` file in the project root with your API keys:
```env
# LLM Provider - Groq
GROQ_API_KEY=your_groq_api_key

# Embeddings & LLM Provider - Google
GOOGLE_API_KEY=your_google_api_key

# Directories
FAISS_BASE=faiss_index
UPLOAD_BASE=data
FAISS_INDEX_NAME=index
```

### 5. Configure Models (Optional)
Edit `config/config.yaml` to select LLM providers and models:
```yaml
llm:
  groq:
    provider: "groq"
    model_name: "deepseek-r1-distill-llama-70b"
    temperature: 0
    max_output_tokens: 2048

embedding_model:
  provider: "google"
  model_name: "models/text-embedding-004"

retriever:
  top_k: 10
```

---

## 📖 API Endpoints

### 1. Health Check
```
GET /health
```
Returns service status.

**Response:**
```json
{"status": "ok", "service": "document-portal"}
```

---

### 2. Analyze Document
```
POST /analyze
Content-Type: multipart/form-data

file: <PDF/DOCX/TXT file>
```

Extracts metadata, key topics, and summary from document.

**Response:**
```json
{
  "metadata": {
    "title": "...",
    "author": "...",
    "num_pages": 10,
    ...
  },
  "summary": "...",
  "key_topics": ["topic1", "topic2", ...]
}
```

---

### 3. Compare Documents
```
POST /compare
Content-Type: multipart/form-data

reference: <reference document>
actual: <document to compare>
```

Identifies differences, similarities, and changes between two documents.

**Response:**
```json
{
  "rows": [
    {"section": "Introduction", "difference": "..."},
    ...
  ],
  "session_id": "session_20250228_123456_abc123"
}
```

---

### 4. Build Chat Index
```
POST /chat/index
Content-Type: multipart/form-data

files: <list of documents>
session_id: <optional; auto-generated if not provided>
use_session_dirs: true
chunk_size: 1000
chunk_overlap: 200
k: 5
```

Ingests documents, chunks them, creates embeddings, and builds FAISS index for chat.

**Response:**
```json
{
  "session_id": "session_20250228_123456_abc123",
  "documents_ingested": 3,
  "chunks_created": 45,
  "embedding_model": "models/text-embedding-004",
  "retriever_k": 5
}
```

---

### 5. Chat Query
```
POST /chat/query
Content-Type: application/json

{
  "session_id": "session_20250228_123456_abc123",
  "question": "What is the main topic?",
  "chat_history": [
    {"role": "user", "content": "Previous question"},
    {"role": "assistant", "content": "Previous answer"}
  ]
}
```

Queries documents using RAG and returns context-aware answers.

**Response:**
```json
{
  "answer": "...",
  "source_chunks": [
    {"content": "...", "document": "filename.pdf", "page": 1}
  ],
  "session_id": "session_20250228_123456_abc123"
}
```

---

### 6. Serve UI
```
GET /
```

Serves HTML UI (`templates/index.html`).

---

## 💻 Running the Application

### Local Development
```bash
uvicorn api.main:app --host 0.0.0.0 --port 8000 --reload
```

Access at: `http://localhost:8000`

### Docker Deployment
```bash
docker build -t document-portal:latest .
docker run -p 8080:8080 --env-file .env document-portal:latest
```

Access at: `http://localhost:8080`

---

## 🧪 Testing

Run tests with pytest:
```bash
pytest tests/ -v
```

Run with coverage:
```bash
pytest tests/ --cov=src --cov-report=html
```

---

## 📝 Usage Examples

### Example 1: Analyze a Document

```bash
curl -X POST "http://localhost:8000/analyze" \
  -F "file=@document.pdf"
```

### Example 2: Build Index and Chat

```bash
# Step 1: Build index
curl -X POST "http://localhost:8000/chat/index" \
  -F "files=@document1.pdf" \
  -F "files=@document2.pdf" \
  -F "session_id=my_session" \
  -F "chunk_size=1000" \
  -F "chunk_overlap=200" \
  -F "k=5"

# Step 2: Ask a question
curl -X POST "http://localhost:8000/chat/query" \
  -H "Content-Type: application/json" \
  -d '{
    "session_id": "my_session",
    "question": "What are the key findings?",
    "chat_history": []
  }'
```

---

## 🔐 Configuration

### LLM Models Available

**Groq:**
- `deepseek-r1-distill-llama-70b` (default)

**Google:**
- `gemini-2.5-flash`

### Embedding Model
- `google/text-embedding-004`

### Retriever Settings
- **top_k** (default: 10) – Number of chunks to retrieve
- **chunk_size** (default: 1000) – Characters per chunk
- **chunk_overlap** (default: 200) – Overlap between chunks

---

## 📊 Session Management

Sessions are automatically created with unique IDs:
```
session_YYYYMMDD_HHMMSS_hash
```

Each session stores:
- **FAISS Index** – `faiss_index/session_*/`
- **Upload Data** – `data/session_*/`
- **Metadata** – Ingested file info and chunk metadata

---

## 📋 Logging

Logs are written to `logs/` directory using StructLog. Key log levels:
- **INFO** – API requests, session creation
- **WARNING** – Retrieval fallbacks, edge cases
- **ERROR** – Failed ingestion, LLM errors
- **DEBUG** – Detailed processing steps (dev mode)

Access logs:
```bash
tail -f logs/app.log
```

---

## ⚠️ Error Handling

Custom exceptions in `exception/custom_exception.py`:
- `DocumentPortalException` – Application-level errors
- `DocumentIngestionError` – File parsing/indexing failures
- `RetrievalError` – FAISS or retrieval failures
- `LLMError` – Model or API call failures

All errors are logged with full traceback.

---

## 🚀 Roadmap

- [ ] Multi-language document support
- [ ] Web UI improvements (Streamlit/Gradio)
- [ ] Hybrid search (BM25 + semantic)
- [ ] Source citation highlighting
- [ ] User authentication & role-based access
- [ ] Document versioning and audit logs
- [ ] Feedback loop for model refinement
- [ ] Batch document processing

---

## 📄 License

Developed by Bharath

---

## 🤝 Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 🔍 Troubleshooting

### Issue: "FAISS index not found"
**Solution:** Ensure documents are indexed via `/chat/index` endpoint before querying.

### Issue: "LLM API key not set"
**Solution:** Check `.env` file has valid `GROQ_API_KEY` and `GOOGLE_API_KEY`.

### Issue: "Unsupported file format"
**Solution:** Supported formats are `.pdf`, `.docx`, `.txt`. Convert other formats first.

### Issue: "Out of memory during indexing"
**Solution:** Reduce `chunk_size` or process fewer documents at once.

---

## 📊 Performance Notes

- **Embedding Generation:** ~1-2s per 1000 tokens (Google API)
- **FAISS Indexing:** ~0.1s per 1000 chunks (CPU)
- **Query Retrieval:** ~0.05s for top-k (FAISS)
- **LLM Response:** 2-10s depending on model and query complexity

**Recommendation:** For production use, consider deploying on a GPU instance for faster embedding generation.
