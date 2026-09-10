# Adaptive RAG - Agentic AI Chatbot

[![Python 3.9+](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-Latest-green.svg)](https://fastapi.tiangolo.com/)
[![LangGraph](https://img.shields.io/badge/LangGraph-0.5.4-orange.svg)](https://python.langchain.com/langgraph/)
[![Qdrant](https://img.shields.io/badge/Qdrant-VectorDB-purple.svg)](https://qdrant.tech/)

## 📋 Overview

**Adaptive RAG** is an intelligent, end-to-end Retrieval-Augmented Generation (RAG) system powered by agentic AI architecture. It combines dynamic query routing, intelligent document retrieval, and advanced LLM capabilities to provide accurate, context-aware answers to user queries.

The system intelligently adapts its retrieval strategy based on query type, utilizing indexed documents, general knowledge, or real-time web search to generate comprehensive responses. Built with a modular architecture using LangGraph for workflow orchestration and multiple storage backends for scalability.

---

## 🎯 Key Features

### 🧠 Intelligent Query Routing
- **Adaptive Classification**: Automatically routes queries to the most appropriate processing pipeline
- **Three Query Types**:
  - **Index**: Queries answerable from uploaded documents
  - **General**: Queries answerable with general knowledge
  - **Search**: Queries requiring real-time web search

### 📚 Advanced RAG Pipeline
- **Document Processing**: Intelligent chunking and embedding of documents
- **Vector Search**: Fast similarity-based retrieval using Qdrant
- **Relevance Grading**: Automatic evaluation of retrieved documents
- **Query Rewriting**: Optimizes queries for better retrieval results

### 🤖 Agentic AI Architecture
- **Multi-Agent System**: Specialized agents for different tasks
- **ReAct Framework**: Reasoning and Acting pattern for intelligent decision-making
- **Tool Integration**: Seamless integration with retrieval tools and web search

### 💾 State Management
- **MongoDB Backend**: Persistent chat history and session management
- **Session Tracking**: Individual conversation contexts per user
- **Memory Management**: Full conversation context retention

### 🎨 User Interface
- **Streamlit Web App**: Interactive chat interface with document upload
- **File Support**: PDF and TXT document uploads
- **Real-time Feedback**: Live chat with instant responses

### ⚡ API-First Architecture
- **FastAPI Backend**: High-performance REST API
- **Async Operations**: Non-blocking database and API calls
- **RESTful Endpoints**: Well-defined API contracts

---

## 🏗️ Architecture

### System Components

```
┌─────────────────────────────────────────────────────────────────┐
│                         User Interface                          │
│  ┌──────────────��───────────────────────────────────────��───┐  │
│  │  Streamlit Web Application                               │  │
│  │  • Chat Interface                                        │  │
│  │  • Document Upload (PDF, TXT)                            │  │
│  │  • Session Management                                    │  │
│  └──────────────────────────────────────────────────────────��  │
└───────────────────────────────────────────��─────────────────────┘
                            ↓
┌────────────────────────────────────────────────��────────────────┐
│                       FastAPI Backend                           │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │  REST API Endpoints                                      │  │
│  │  • POST /rag/query                                       │  │
│  │  • POST /rag/documents/upload                            │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────────┐
│                    LangGraph Orchestration                      │
│  ┌─────────┐  ┌──────────┐  ┌─────────┐  ┌──────────┐         │
│  │ Query   │→ │ Classify │→ │ Router  │→ │ Pipeline │         │
│  │ Analyze │  │ Query    │  │ Output  │  │ Exec     │         │
│  └─────────┘  └──────────┘  └───��─────┘  └──────────┘         │
└─────────────────────────────────────────────────────────────────┘
                            ↓
        ┌───────��──────────┬────────────────��─┬────────────────┐
        ↓                  ↓                  ↓                ↓
   ┌─────────┐       ┌──────────┐      ┌────────────┐   ┌──────────┐
   │ Retriever│      │ General  │      │ Web Search │   │ Response │
   │ (Index)  │      │ LLM      │      │ (Tavily)   │   │ Generator│
   └─────────┘       └──────────┘      └────────────┘   └──────────┘
        ↓                  ↓                  ↓                ↓
        └──────────────────┬──────────────────┬────────────────┘
                           ↓
            ┌─────────────────────────────────┐
            │   Response to User               │
            └─────────────────────────────────┘
```

### Graph Nodes

1. **query_analysis**: Analyzes and classifies incoming queries
2. **retriever**: Retrieves relevant documents from vector store
3. **grade**: Evaluates relevance of retrieved documents
4. **rewrite**: Optimizes query for better retrieval results
5. **generate**: Generates final response from context
6. **web_search**: Performs real-time web search when needed
7. **general_llm**: Provides general knowledge answers

---

## 📦 Project Structure

```
AdaptiveRag/
├── src/                              # Main source code
│   ��── main.py                       # FastAPI application entry point
│   ├── api/                          # API routes and endpoints
│   │   └── routes.py                 # RAG query and document upload endpoints
│   ├── config/                       # Configuration management
│   │   ├── settings.py               # Application settings
│   │   └── prompts.yaml              # LLM prompts and system messages
│   ├── core/                         # Core utilities
│   │   ├── config.py                 # Core configuration
│   │   └── logger.py                 # Logging setup
│   ├── db/                           # Database layer
│   │   └── mongo_client.py           # MongoDB client initialization
│   ├── llms/                         # Language model integrations
│   │   └── openai.py                 # OpenAI ChatGPT-4o initialization
│   ├── memory/                       # Chat memory management
│   │   ├── chat_history_mongo.py     # MongoDB-backed chat history
│   │   └── chathistory_in_memory.py  # In-memory chat history (fallback)
│   ├── models/                       # Data models and schemas
│   │   ├── state.py                  # Graph state definition
│   │   ├── query_request.py          # Query request schema
│   │   ├── grade.py                  # Relevance grade model
│   │   ├── route_identifier.py       # Route classification model
│   │   └── verification_result.py    # Answer verification model
│   ├── rag/                          # RAG pipeline implementation
│   │   ├── graph_builder.py          # LangGraph workflow construction
│   │   ├── nodes.py                  # Graph node implementations
│   │   ├── retriever_setup.py        # Vector store and retriever setup
│   │   ├── document_upload.py        # Document processing and upload
│   │   └── reAct_agent.py            # ReAct agent setup
│   └── tools/                        # Utility tools and functions
│       ├── common_tools.py           # Shared utility functions
│       └── graph_tools.py            # Graph routing and decision tools
│
├── streamlit_app/                    # Streamlit web application
│   ├── home.py                       # Authentication and login page
│   ├── pages/                        # Multi-page application
│   │   └── chat.py                   # Chat interface and document upload
│   └── utils/                        # Streamlit utilities
│       └── api_client.py             # Backend API client
│
├── README.md                         # This file
├── requirements.txt                  # Python dependencies
├── CODE_STYLE_GUIDE.md               # Code formatting standards
├── QUICK_REFERENCE.md                # Quick reference guide
├── README_FORMATTING.md              # Formatting documentation
├── VERIFICATION_CHECKLIST.md         # QA verification checklist
├── FORMATTING_SUMMARY.md             # Summary of code formatting
└── DOCUMENTATION_INDEX.md            # Documentation navigation index
```

---

