# Corporate HR Policy Retrieval Prototype (RAG)

A functional, notebook-driven Retrieval-Augmented Generation (RAG) prototype built to experiment with semantic text processing, vector-based similarity search, and dynamic prompt injection using the LangChain framework. 

This repository demonstrates the step-by-step assembly of a document-grounded answering system using an unstructured corporate policy handbook (Nestlé HR guidelines) as the target domain knowledge base.

## 🛠️ System Architecture & Workflow

The core workspace is structured inside a single, reproducible pipeline covering the entire exploratory lifecycle:

1. **Document Loading & Parsing:** Ingests source multi-page PDF documents and extracts raw text metadata fields using LangChain's `PyPDFLoader`.
2. **Granular Chunking Strategy:** Implements a `RecursiveCharacterTextSplitter` configured with a strict 150-character threshold and multi-stage fallback separators (including lookbehind regular expressions for sentence boundaries) to guarantee semantic preservation within tight token limits.
3. **Vector Ingestion & Persistence:** Converts character chunks into high-dimensional semantic vectors via `OpenAIEmbeddings` and manages indexing within a disk-backed local `Chroma` vector database.
4. **Retrieval Orchestration:** Constructs a standard LangChain `RetrievalQA` chain utilizing a foundational "stuff" document layout to seamlessly bundle retrieved vector contexts directly into model prompts.
5. **Dynamic Context Enrichment (Jinja2):** Employs external Jinja2 template rendering (`prompt_template.jinja2`) to support programmatic text interpolation. Includes conditional intent filtering to intercept specific keyword topics (e.g., parental/maternal leave inquiries) and automatically inject relevant external web references before final model synthesis.
6. **Inference Workspace (Gradio):** Wraps the full retrieval-generation routine into a lightweight, deployable UI utilizing `Gradio` for prompt validation, latency observations, and interactive manual debugging.

## 📂 Repository Layout

* `data/`
  * `1728286846_the_nestle_hr_policy_pdf_2012.pdf` — Unstructured source corporate documentation.
  * `prompt_template.jinja2` — Jinja2 template source mapping target variables.
  * `chroma_vector_x/` — Persisted database index holding localized vector records.
* `scripts/`
  * `00_hr_chatbot.ipynb` — Primary workspace detailing environment setups, extraction routines, and loop execution.

## 💻 Technical Stack

* **Framework:** LangChain
* **Vector Store:** ChromaDB
* **Embeddings & LLM Engine:** OpenAI API
* **Templating Engine:** Jinja2
* **Interface Layer:** Gradio
