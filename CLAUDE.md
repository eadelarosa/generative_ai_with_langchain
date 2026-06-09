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
- **Python:** 3.12.12 (recommended) / 3.11+ (for type checking via `pyproject.toml` target-version)
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
- `Makefile`: Code validation commands (legacy, prefer ruff)

### Files Excluded from Claude Context
The `.claudeignore` file excludes large/temporary files to keep context efficient:
- `.ipynb_checkpoints/`, model files (`.bin`, `.pt`, `.safetensors`), large data files
- Vector store caches (`.faiss`, `chroma_db/`), environment/config files, the book PDF
- The `docs/` and `notebooks/` directories (to avoid duplicating chapter content)
- CLAUDE.md itself (to save tokens)

## Key Dependencies & Versions

**Note:** Versions listed are from the active `second_edition` branch. The `v1` branch tracks ongoing LangChain 1.x compatibility updates. Always check `requirements.txt` or `pyproject.toml` for the current versions in your branch.

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

**Primary Tool: Ruff** — Fast, all-in-one Python linter and formatter
```bash
# Lint code (checks E, F, I rules)
ruff check .

# Format code (auto-fix style issues)
ruff format .

# Lint and auto-fix in one pass
ruff check --fix .
```

**Type Checking**
```bash
# Validate type hints (requires mypy installed)
make typecheck
```

**Legacy Commands** — Still functional but prefer ruff above
```bash
# Full lint with flake8 + black (legacy, slower)
make lint
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

## Common Tasks & Patterns

### Upgrading Chapter Code for LangChain v1

When the LangChain ecosystem releases major updates, chapters need systematic upgrades:

1. **Identify Import Changes:**
   - Old: `from langchain import X` → New: `from langchain_x import X`
   - Example: `from langchain import OpenAI` → `from langchain_openai import ChatOpenAI`

2. **Fix Callback Patterns:**
   - LangChain 1.x changed how callbacks integrate with LangGraph
   - Traced operations now flow through graph state, not separate callback handlers
   - See `chapter9/` examples for modern callback patterns with FastAPI streaming

3. **Update LCEL Chains:**
   - Syntax unchanged (`prompt | llm | parser`), but component imports shift
   - Ensure all components are from the correct namespace package

4. **Test Comprehensively:**
   - Run corresponding notebook in Colab/Kaggle alongside Python scripts
   - Verify LangSmith traces show correct structure
   - Check that streaming still works correctly (common pain point in upgrades)

### Working with Notebooks

- Notebooks are the "source of truth" for chapter examples; Python scripts are implementations
- When updating code, keep notebooks and scripts aligned (mention in commit message if both change)
- Test notebook in Colab/Kaggle before pushing (cloud platforms have latest dependencies)

### Testing Your Changes

- Type checking: `make typecheck` validates main application code
- Style: `ruff check --fix .` auto-fixes most issues
- Before PR: run the specific chapter's examples end-to-end locally

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

### Branch Strategy

**`second_edition`** (primary release branch)
- Latest stable code matching book's 2nd edition
- LangChain 0.3+ focus
- Target for pull requests and releases

**`v1`** (upgrade-in-progress branch)
- Active work on LangChain 1.x compatibility
- Not yet merged to `second_edition`
- Chapters being systematically upgraded (see recent commits)
- PRs merged here before final review to `second_edition`

**`main`** (legacy)
- Original version (December 2023)
- No longer actively maintained

### Commit Message Conventions
When upgrading chapters to v1, follow this pattern:
```
refactor(chapX): upgrade to latest v1 changes

- Updated imports from langchain.x to langchain-x packages
- Fixed callback patterns for LangGraph 1.x
- Updated LCEL usage if applicable

Closes #<issue-number>
```

Examples:
- `refactor(chap9): upgrade chapter 9 to latest v1 changes and resolve callback issues`
- `refactor(chap8): upgrade langsmith_evaluation.ipynb to v1 patterns`

### Key Considerations
- Repository is actively maintained to match LangChain ecosystem changes
- Code stability prioritized over chasing every minor LangChain update
- Pull requests welcome for bug fixes and improvements (see CONTRIBUTING.md)

## Important Notes

### Working with Chapter Code

**Import Paths:** Examples assume `PYTHONPATH` is set to repo root when running scripts:
```bash
PYTHONPATH=. python chapter7/software_development/agent.py
```
Without this, relative imports from shared modules will fail.

**Shared Modules:** Some code is shared across chapters (e.g., `chat_with_retrieval/`, `monitoring_and_evaluation/`). When updating chapter code, check if shared modules are affected by LangChain API changes.

**Notebooks vs Scripts:** 
- Notebooks (`.ipynb`) are primary for interactive exploration and book alignment
- Standalone scripts in `chapter{4-9}/` are deployable applications, not just notebook conversions
- Updating a notebook may not fully update corresponding scripts

### Environment & Configuration
- Never commit credentials or API keys
- Use `config.py` pattern for local development (already in `.gitignore`)
- All examples check for environment variables at runtime

### Notebook Execution
- Notebooks require Jupyter setup; see cloud platform links in chapter READMEs for Colab/Kaggle
- Each notebook is self-contained; can run independently
- Notebooks pull API keys from `config.py`

### Stability vs. Latest Features
- Repository intentionally lags minor LangChain patch updates for stability
- Focus is on reliable, production-grade examples rather than chasing bleeding-edge
- Before major refactors, test against corresponding book chapter examples

### Documentation Updates
- **Chapter READMEs:** Document specific examples and provide cloud platform links
- **SETUP.md:** Installation steps and API key configuration
- **CONTRIBUTING.md:** Dependency syncing and testing guidelines
- **CLAUDE.md:** This file (guidance for Claude Code)

## Development Productivity Tips

**Running Chapter Code:**
- Always set `PYTHONPATH=.` when running scripts: `PYTHONPATH=. python chapter7/agent.py`
- Notebooks can be run via Jupyter locally, or via Colab/Kaggle (links in chapter READMEs)

**Code Quality Workflow:**
- Format before committing: `ruff check --fix . && ruff format .`
- Type check specific modules: `make typecheck` (covers main application directories)
- Run this before opening a PR to catch style and type issues early

**Debugging LangChain Upgrades:**
- When upgrading code to v1, check LangSmith traces for callback/state issues
- Search for deprecated imports: `grep -r "from langchain import" chapter*` (should use `langchain_x` packages)
- Run the notebook in book context alongside the code to ensure parity

**Finding Examples:**
- Chapter READMEs link to Colab/Kaggle notebooks with full setup
- Refer to book for conceptual context; code examples implement those concepts directly
- Each chapter directory is self-contained but may reference shared modules
