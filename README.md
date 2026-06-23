# Corporate HR Policy Retrieval Prototype (RAG)

A notebook-driven Retrieval-Augmented Generation (RAG) prototype for experimenting with text processing, vector similarity search, and dynamic prompt injection in LangChain.

The notebook walks through building a document-grounded question-answering system step by step, using a corporate policy handbook (Nestlé's HR guidelines) as the knowledge base.

## 🛠️ System Architecture & Workflow

The whole pipeline lives in one notebook and runs end to end:

1. **Document loading & parsing:** Reads the multi-page source PDF and extracts its text and metadata with LangChain's `PyPDFLoader`.
2. **Chunking:** Splits the text with a `RecursiveCharacterTextSplitter` set to a 150-character limit, with fallback separators (including lookbehind regular expressions for sentence boundaries) so chunks break on natural boundaries instead of mid-sentence.
3. **Vectorization & storage:** Turns the chunks into embeddings with `OpenAIEmbeddings` and indexes them in a local, disk-backed `Chroma` vector store.
4. **Retrieval:** Builds a LangChain `RetrievalQA` chain with the "stuff" document layout, which packs the retrieved chunks straight into the model prompt.
5. **Prompt templating with Jinja2:** Renders the prompt from an external Jinja2 template (`prompt_template.jinja2`). A conditional check catches specific topics (for example, parental or maternal leave questions) and adds relevant external web links before the prompt is sent to the model.
6. **Interface (Gradio):** Wraps the retrieval and generation steps in a small `Gradio` UI for testing prompts, watching latency, and debugging by hand.

## 📂 Repository Layout

* `data/`
  * `1728286846_the_nestle_hr_policy_pdf_2012.pdf` — Source corporate document.
  * `prompt_template.jinja2` — Jinja2 prompt template.
  * `chroma_vector_x/` — Persisted Chroma index holding the stored vectors.
* `scripts/`
  * `00_hr_chatbot.ipynb` — The notebook: environment setup, extraction, and the full pipeline.

## 💻 Technical Stack

* **Framework:** LangChain
* **Vector Store:** ChromaDB
* **Embeddings & LLM:** OpenAI API
* **Templating:** Jinja2
* **Interface:** Gradio
