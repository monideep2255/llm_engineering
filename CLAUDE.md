# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is an 8-week LLM Engineering course repository teaching practical AI development from basic API calls to building autonomous agentic AI systems. The course uses Jupyter notebooks for hands-on learning, progressing from simple LLM interactions to advanced topics like RAG, fine-tuning, and multi-agent systems.

## Environment Setup

### Python Environment
- Python 3.11+ required (specified in `pyproject.toml`)
- Use virtual environment: `.venv`
- Package management: `uv` (modern, fast Python package manager) or pip
- Install dependencies: `uv sync` or `pip install -r requirements.txt`

### Environment Variables
- Store API keys in `.env` file at repository root
- Required keys:
  - `OPENAI_API_KEY` - for OpenAI API access (starts with `sk-proj-`)
  - `HF_TOKEN` - for HuggingFace datasets and model access
  - Optional: `ANTHROPIC_API_KEY`, `GOOGLE_API_KEY` for other providers
- Load with: `from dotenv import load_dotenv; load_dotenv(override=True)`

### Jupyter Notebooks
- All week folders contain `.ipynb` files as primary learning materials
- Select kernel: `.venv (Python 3.12.x) .venv/bin/python`
- Required extensions in Cursor/VS Code: Python (ms-python), Jupyter (ms-toolsai)

## Course Structure

### Weekly Progression
- **Week 1**: Basic LLM API calls, web scraping, prompt engineering
- **Week 2**: Model comparison, structured outputs, Gradio interfaces
- **Week 3**: Embeddings, semantic similarity (runs on Google Colab with GPUs)
- **Week 4**: Structured outputs, validation, advanced prompting
- **Week 5**: RAG (Retrieval Augmented Generation) with LangChain, vector databases (ChromaDB)
- **Week 6**: Dataset curation, evaluation frameworks
- **Week 7**: Fine-tuning LLMs (runs on Google Colab with GPUs)
- **Week 8**: Multi-agent systems - capstone "Price is Right" project

### Key Directories
- `week[1-8]/` - Weekly lab notebooks and supporting code
- `guides/` - Reference materials (command line, git, Python foundations, debugging, AI APIs)
- `setup/` - Platform-specific setup instructions (Mac, PC, Linux)
- `community-contributions/` - Student-contributed solutions and variations
- `week5/knowledge-base/` - Sample data for RAG exercises (insurance contracts, products, employees)
- `week8/agents/` - Multi-agent framework implementation

## Common Development Tasks

### Running Notebooks
```bash
# Start Jupyter in Cursor/VS Code by opening .ipynb files
# Or use Jupyter Lab:
jupyter lab
```

### Testing Web Applications
Week 2+ includes Gradio applications:
```python
import gradio as gr
gr.ChatInterface(chat_function).launch(inbrowser=True)
```

### Working with Ollama (Free Local Alternative)
Ollama provides free local LLM inference:
```bash
ollama run llama3.2        # Standard model
ollama run llama3.2:1b     # Smaller for limited hardware
```
See `guides/09_ai_apis_and_ollama.ipynb` for OpenAI-compatible usage patterns.

### Google Colab Integration
Weeks 3 and 7 require GPU access:
- Colab links provided in respective week folders
- Free tier sufficient for most exercises
- Notebooks include Colab-specific setup cells

## Architecture Patterns

### Week 5: RAG Implementation
Two implementations provided:
- **Basic** (`implementation/`): Simple ChromaDB + LangChain setup
- **Pro** (`pro_implementation/`): Advanced chunking strategies, metadata filtering

RAG workflow:
1. **Ingestion** (`ingest.py`): Load documents → Split into chunks → Generate embeddings → Store in ChromaDB
2. **Retrieval** (`answer.py`): Query → Find similar chunks → Augment LLM context → Generate response

Key classes:
- `ChatOpenAI` - LangChain wrapper for OpenAI models
- `Chroma` - Vector database for embeddings
- `OpenAIEmbeddings` - Generate vector representations
- `RecursiveCharacterTextSplitter` - Chunk documents intelligently

### Week 8: Multi-Agent System
The "Price is Right" capstone project predicts product prices from descriptions.

**Agent Hierarchy:**
- `Agent` (base class in `agents/agent.py`) - Logging infrastructure with color-coded output
- **Specialized Agents:**
  - `PlanningAgent` - Orchestrates workflow, coordinates other agents
  - `ScannerAgent` - Processes incoming deal opportunities
  - `FrontierAgent` - Calls GPT/Claude for complex reasoning
  - `NeuralNetworkAgent` - Uses deep learning for price prediction
  - `EnsembleAgent` - Combines multiple model predictions
  - `EvaluatorAgent` - Assesses prediction quality
  - `MessagingAgent` - Handles notifications

**Framework** (`deal_agent_framework.py`):
- Coordinates 7+ agents working asynchronously
- Maintains shared state and message passing
- Implements evaluation and continuous improvement loop

**UI** (`price_is_right.py`):
- Gradio interface with real-time logging
- Plotly visualizations for price distributions
- Queue-based log streaming from agents

### Data Processing Patterns (Week 6)

**Item Processing:**
- `pricer/items.py` - Item dataclass with validation
- `pricer/parser.py` - Parse raw Amazon data into Items
- `pricer/loaders.py` - Load datasets from HuggingFace Hub

Typical flow:
1. Load raw dataset from HuggingFace
2. Parse into `Item` objects with price validation ($1-$1000 range)
3. Deduplicate by title and full description
4. Curate via weighted sampling (bias toward higher prices, certain categories)
5. Split train/val/test and push to HuggingFace Hub

## Key Dependencies

### Core AI Libraries
- `openai` - OpenAI API client
- `anthropic` - Anthropic Claude API
- `google-generativeai` - Gemini API
- `ollama` - Local model inference
- `langchain` suite - RAG framework (v1.0+)
- `sentence-transformers` - Embedding models
- `transformers` - HuggingFace models
- `torch` - PyTorch for deep learning

### Data & ML
- `datasets` - HuggingFace datasets (pinned to 3.6.0)
- `chromadb` - Vector database for RAG
- `pandas`, `numpy` - Data manipulation
- `scikit-learn`, `xgboost` - Traditional ML
- `wandb` - Experiment tracking

### UI & Visualization
- `gradio` - Web interfaces (v5.x, <6.0)
- `matplotlib`, `plotly` - Visualizations

### Utilities
- `python-dotenv` - Environment variable management
- `beautifulsoup4` - Web scraping
- `requests` - HTTP requests
- `tiktoken` - Token counting for OpenAI models

## Important Notes

### API Cost Management
Course designed for minimal API costs (~$2 total):
- Use `gpt-4.1-nano` or `gpt-4.1-mini` for cheapest OpenAI models
- Use `claude-3-haiku-20240307` for cheapest Anthropic model
- Week 7 fine-tuning optional (can cost ~$10 with full dataset)
- Always use `ed-donner/items_raw_lite` instead of `items_raw_full` for cost savings
- Monitor usage: [OpenAI dashboard](https://platform.openai.com/usage), [Anthropic dashboard](https://console.anthropic.com/settings/cost)

### Code Execution Philosophy
- **Execute, don't just read**: Run every notebook cell, inspect objects, experiment
- **Modify and extend**: Change prompts, try different models, add features
- **Share improvements**: Submit PRs to `community-contributions/` folder
- Clear notebook outputs before committing: Edit → Clean outputs of all cells

### Troubleshooting
- Common issues documented in `setup/troubleshooting.ipynb`
- Week 5 LangChain upgrade: Code updated for LangChain 1.0 (November 2025)
- If encountering `trust_remote_code` errors with datasets: `uv add --upgrade datasets==3.6.0`
- For NameError issues, see `guides/06_python_foundations.ipynb`

### Model Selection
Default models throughout course:
- OpenAI: `gpt-4.1-mini` (or `gpt-4.1-nano` for lower cost)
- Can substitute with Ollama for free local inference
- Week 3+ may use DeepSeek, Gemini for comparison

### Git Workflow
- Main branch: Updated course material (post-November 2025 refresh)
- Original branch: Code matching original video series
- Switch: `git checkout original` / `git checkout main`
