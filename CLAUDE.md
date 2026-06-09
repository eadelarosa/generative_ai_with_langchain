# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the code repository for "Generative AI with LangChain, Second Edition" by Ben Auffarth and Leonid Kuligin. The repository contains practical implementations and examples for building production-ready LLM applications using LangChain and LangGraph, covering topics from fundamentals to advanced multi-agent systems, RAG pipelines, and deployment.

**Key Focus:** Building production-ready LLM applications with Python, LangChain 0.3+, LangGraph, and modern enterprise patterns.

**Repository Branches:**
- `second_edition` (main): Latest 2nd edition code (LangChain 0.3+)
- `softupdate`: Soft update version (LangChain 0.1.13)
- `main`: Original version (December 2023)

## Development Environment Setup

### Prerequisites
- **Python:** 3.12.12
- **System Tools:** pandoc, C++ compiler (gcc/g++), git
- **Package Manager:** conda (recommended), pip, poetry, or Docker

### Installation Options

**Conda (Recommended):**
```bash
conda env create --file=langchain_ai.yaml
conda activate langchain_ai
```

**Pip:**
```bash
pip install -r requirements.txt
```

**Poetry:**
```bash
poetry install --no-root
```

**Docker:**
```bash
docker build -t langchain_ai .
docker run -it -p 8888:8888 langchain_ai
```

### API Key Configuration
Create a `config.py` file in the root directory with environment variable setup (not committed to version control):
```python
import os

def set_environment():
    os.environ['OPENAI_API_KEY'] = 'your-key'
    os.environ['ANTHROPIC_API_KEY'] = 'your-key'
    # ... other provider keys as needed
```

For Google Vertex models, install and authenticate gcloud CLI: `gcloud auth application-default login`

## Repository Structure

### Chapter Organization
Each chapter (`chapter1/` through `chapter9/`) is self-contained with:
- **Notebooks (.ipynb):** Jupyter notebooks with interactive examples and explanations
- **Python Scripts (.py):** Standalone Python modules and applications
- **README.md:** Chapter-specific documentation and links to cloud platforms (Colab, Kaggle)

**Chapter Overview:**
1. **Chapter 1:** Rise of Generative AI - Concepts and fundamentals
2. **Chapter 2:** First Steps with LangChain - Chat models, prompts, LCEL, local models
3. **Chapter 3:** Building Workflows with LangGraph - Graph-based workflow orchestration
4. **Chapter 4:** Building Intelligent RAG Systems - Retrieval-augmented generation
5. **Chapter 5:** Building Intelligent Agents - Agent frameworks and tools
6. **Chapter 6:** Advanced Applications and Multi-Agent Systems - Complex agent architectures
7. **Chapter 7:** Software Development and Data Analysis Agents - Specialized agents
8. **Chapter 8:** Evaluation and Testing of LLM Applications - Testing frameworks and metrics
9. **Chapter 9:** Production Deployment and Observability - FastAPI, Ray, monitoring

### Key Directories
- `chapter{1-9}/`: Book chapter code organized by topic
- `writing_assistant/`: Example Streamlit application
- `pyproject.toml`: Ruff and project configuration
- `Makefile`: Code validation commands

## Key Dependencies & Versions

### Core Libraries
- **langchain:** 1.2.10 - Core LLM orchestration
- **langchain-core:** 1.2.13 - Core abstractions
- **langchain-community:** 0.4.1 - Community integrations
- **langgraph:** 1.0.8 - Graph-based workflows (central for Chapter 3+)
- **langsmith:** 0.7.3 - Tracing and evaluation

### Provider Integrations
- **langchain-openai:** 1.1.9 - OpenAI models
- **langchain-anthropic:** 1.3.3 - Claude models
- **langchain-google-genai:** 4.2.0 - Google Gemini
- **langchain-mistralai:** 1.1.1 - Mistral models
- **langchain-groq:** 1.1.2 - Groq models
- **langchain-ollama:** 1.0.1 - Local Ollama models
- **langchain-huggingface:** 1.2.0 - HuggingFace models

### Data & Utilities
- **langchain-chroma:** 1.1.0 - Chroma vector store
- **langchain-text-splitters:** 1.1.0 - Text chunking
- **rank-bm25:** 0.2.2 - BM25 ranking for hybrid search
- **docarray:** 0.41.0 - Document handling
- **datasets:** 4.5.0 - HuggingFace datasets
- **huggingface-hub:** 0.36.2 - HuggingFace hub access

### Deployment & Monitoring
- **fastapi:** (via dependencies) - Web frameworks
- **streamlit:** 1.54.0 - Web UI framework
- **ray:** 2.53.0 - Distributed computing
- **jupyter:** 1.1.1 - Notebook support

### Development & Code Quality
- **ruff:** 0.15.1 - Fast Python linter and formatter
- **mypy:** - Type checking (via Makefile)
- **flake8:** - Style checking (via Makefile)

## Common Development Commands

### Code Quality & Validation
```bash
# Type checking with mypy
make typecheck

# Linting with ruff
ruff check .

# Linting with flake8 (legacy)
make lint

# Auto-format code with ruff
ruff format .
ruff check --fix .
```

### Running Examples

**Notebook Examples:**
- Open and run `.ipynb` files in Jupyter or cloud platforms (Colab, Kaggle links in chapter READMEs)
- All notebooks reference API keys from `config.py`

**Chapter 9 Deployment Examples:**
```bash
# FastAPI web service with WebSocket streaming
cd chapter9
PYTHONPATH=. python fastapi/main.py
# Visit http://localhost:8000

# Ray Serve for distributed indexing
PYTHONPATH=. python ray/build_index.py  # Build index
PYTHONPATH=. python ray/serve_index.py  # Serve it

# Observability examples
PYTHONPATH=. python tracing.py
PYTHONPATH=. python prompt_tracking.py
```

**Chapter 7 Agent Examples:**
```bash
# Software development agent
python chapter7/software_development/agent.py

# Data science agent
python chapter7/data_science/agent.py
```

## Code Architecture & Patterns

### LangChain LCEL (LangChain Expression Language)
- Chains are composed using `|` operator: `prompt | llm | output_parser`
- All chains implement `Runnable` protocol with `.invoke()`, `.batch()`, `.stream()`
- See Chapter 2 (LCEL.ipynb) and throughout for patterns

### LangGraph Workflows (Primary Pattern for Chapter 3+)
- Graph-based state machines for complex workflows
- Nodes are functions, edges define transitions
- State is passed through graph execution
- Supports branching, cycles, and conditional routing
- Central to Chapters 3, 5, 6, 7 examples

### RAG Architecture (Chapter 4)
- Document loading → Text splitting → Embedding → Vector store retrieval
- Hybrid search: BM25 (lexical) + vector (semantic) ranking
- Re-ranking and fact-checking pipelines for accuracy

### Agent Patterns (Chapter 5-7)
- Tool-using agents with LangChain agent framework
- Multi-agent systems with agent handoffs and communication
- Specialized agents for software development and data analysis
- LangSmith integration for debugging and trajectory tracking

### Deployment Patterns (Chapter 9)
- **FastAPI:** REST + WebSocket endpoints for streaming
- **Ray Serve:** Distributed serving with scaling
- **Observability:** LangSmith tracing, custom monitoring integrations
- **Vector Indexing:** FAISS for similarity search at scale

## Testing & Evaluation

### Type Checking
```bash
make typecheck
```
Validates type hints in `chat_with_retrieval/`, `data_science/`, `information_extraction/`, `monitoring_and_evaluation/`, `prompting/`, `question_answering/`, `search_engine/`, `software_development/`, `summarize/`, `webserver/`, `writing_assistant/`

### Style Validation
```bash
ruff check .
```
Enforces E (pycodestyle), F (pyflakes), I (isort) rules with max complexity 10 (McCabe).

### Notebook Testing
- Notebooks in `chapter{1-9}/` are manually run and verified against latest LangChain versions
- Each notebook is self-contained and can be executed independently
- See chapter READMEs for cloud platform links (Colab, Kaggle)

## Git Workflow & Branch Strategy

### Current Branch Context
- Working on `v1` branch (for specific LangChain v1 upgrades)
- Main branch for releases: `second_edition`
- Regular updates to harmonize with LangChain releases

### Commit Message Conventions
- Use format: `refactor(chapX): <description>` for chapter code updates
- Example: `refactor(chap9): upgrade chapter 9 to latest v1 changes`
- Example: `refactor(chap8): upgrade chapter 8 langsmith_evaluation.ipynb to latest v1 changes`

### Key Considerations
- Repository is actively maintained to match LangChain ecosystem changes
- Code stability prioritized over chasing every minor LangChain update
- Pull requests welcome for bug fixes and improvements (see CONTRIBUTING.md)

## Important Notes

### Stability vs. Latest Features
- Repository may lag minor LangChain patch updates intentionally for stability
- Focus is on reliable, production-grade examples rather than bleeding-edge
- Test code against book examples before significant refactors

### Environment Variables
- Never commit credentials or API keys
- Use `config.py` pattern for local development
- All examples check for environment variables before executing

### Notebook Execution
- Most code exists in Jupyter notebooks (`.ipynb` files)
- Pure Python scripts in `chapter{4-9}/` are applications (FastAPI, Ray, etc.)
- Always set up environment and API keys before running notebooks

### Documentation Updates
- Chapter READMEs document the examples and provide platform links
- SETUP.md covers installation and API key setup
- CONTRIBUTING.md covers contribution guidelines (dependency sync, testing)

## Development Productivity Tips

- Use Jupyter for interactive exploration matching chapter structure
- Leverage PYTHONPATH when running standalone scripts: `PYTHONPATH=. python script.py`
- Check chapter README for specific examples and cloud platform options
- Use ruff for fast linting: `ruff check --fix .` to auto-format
- Refer to book chapters for conceptual context behind examples
