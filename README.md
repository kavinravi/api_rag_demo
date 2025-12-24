# RAG Demo

A simple Retrieval Augmented Generation (RAG) demo that lets you ask questions about a collection of dummy (AI-generated) documents. The system retrieves relevant chunks from your documents and uses an LLM to generate answers with source citations. Optionally, it includes functionality for asking questions with additional documents as inputs with a sample input txt file also provided (see bottom of README for instructions)

Built with:
- **LangChain** for orchestration
- **OpenAI** (gpt-4o-mini) for the LLM
- **ChromaDB** for vector storage
- **HuggingFace** (all-MiniLM-L6-v2) for embeddings (runs locally, no API limits)

## Setup

### 1. (Optional) Create virtual environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Create `.env` file

Create a `.env` file in the project root with your OpenAI API key:

```
OPENAI_API_KEY=your-api-key-here (should start with "sk-")
MODEL_NAME=gpt-4o-mini (or any other model)
```

### 4. Add your documents

Replace the dummy files in `rag_data/` with your own `.txt` files. The system will load all `.txt` files from this directory.

### 5. Run the notebook

Open `rag_demo.ipynb` and run all cells. The notebook will:
1. Load your documents
2. Split them into chunks
3. Create embeddings and store in ChromaDB
4. Set up the LLM
5. Let you ask questions with `ask("your question here")`

## Usage

```python
print(ask("what is the company's annual revenue?"))
```

The model will answer based on the retrieved context and cite which source file the information came from.

Example output:
Question:
```python
print(ask("what security certs do they have?"))
```
Output:
```python
TechVentures holds the following security certifications:

- SOC 2 Type II certified
- GDPR compliant
- HIPAA compliant (Enterprise healthcare module)
- ISO 27001 certified
- PCI DSS Level 1 (for payment processing features)

(Source: [rag_data/example5.txt])
```

### Ask with an input document

You can also ask questions about a specific document while still using the RAG database for context. Put a `.txt` file in `./input/` and use:

```python
print(ask_with_doc("summarize this and compare to company policies", "my_document.txt"))
```

## Notes

- First run downloads the embedding model (~90MB), after that it's cached
- The `chroma_db/` folder is gitignored and regenerates when you run the notebook
- Embeddings run locally so there's no rate limits on that part
- gpt-4o-mini is cheap (~$0.15/1M tokens) and fast, gpt-4.1 is another option, slightly more intelligent but 2-3x more expensive, would not recommend reasoning models unless document corpus is extensive and questions are tricky/thorough

