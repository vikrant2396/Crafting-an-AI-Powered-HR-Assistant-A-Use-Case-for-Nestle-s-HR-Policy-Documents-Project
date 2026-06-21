# Crafting an AI-Powered HR Assistant A Use Case for Nestle’s HR Policy Documents Project

A Retrieval-Augmented Generation (RAG) chatbot that answers employee questions about Nestlé's HR policies in natural language, built with **LangChain**, **OpenAI**, **ChromaDB**, and a **Gradio** web interface.

---

##  Overview

Large HR policy documents are hard to search and time-consuming to read. This project solves that problem by turning Nestlé's official HR Policy PDF into a conversational chatbot that:

- Understands natural-language HR questions
- Retrieves the most relevant sections of the policy document
- Generates clear, accurate, context-grounded answers
- Politely declines to answer when the document doesn't contain the relevant information, instead of guessing

The result is a fast, self-service HR assistant accessible through a clean, branded chat UI.

---

##  Features

-  **PDF-based knowledge base** — loads and indexes Nestlé's HR Policy document directly
-  **Smart text chunking** — splits long policy text into overlapping, semantically coherent chunks
-  **Semantic search** — OpenAI embeddings + ChromaDB vector store for accurate retrieval
-  **Conversational memory** — maintains per-session chat history for context-aware follow-up questions
-  **Hallucination control** — a custom prompt restricts answers strictly to the provided document context
-  **Polished Gradio UI** — dark, professional theme with Nestlé branding and one-click sample questions
-  **Secure API key handling** — key is entered at runtime via `getpass`, never hardcoded in the notebook

---

##  Demo

| | |
|---|---|
| **Chat UI** | Dark-themed Gradio interface with a conversation panel, input box, Send/Clear buttons, and 8 quick-access sample questions |
| **Sample Q&A** | *"How does Nestlé handle promotions and career development?"* → returns a structured, bullet-point answer sourced directly from the policy document |

> Add a screenshot of the running app here, e.g. `![Demo](assets/demo-screenshot.png)`

---

##  How It Works

```
HR Policy PDF
      │
      ▼
PyPDFLoader  ──►  RecursiveCharacterTextSplitter  ──►  Text Chunks
      │                                                     │
      │                                                     ▼
      │                                      OpenAI Embeddings (text-embedding-ada-002)
      │                                                     │
      │                                                     ▼
      │                                              ChromaDB Vector Store
      │                                                     │
      ▼                                                     ▼
 User Question  ──►  Retriever (top-k similarity search)  ──►  Relevant Chunks
                                                              │
                                                              ▼
                                          Custom Prompt + Chat History + GPT-3.5 Turbo
                                                              │
                                                              ▼
                                                      Answer (via Gradio UI)
```

1. **Document Loading** — `PyPDFLoader` reads the HR Policy PDF page by page.
2. **Chunking** — `RecursiveCharacterTextSplitter` breaks the document into ~1000-character overlapping chunks for better retrieval granularity.
3. **Embedding & Storage** — Each chunk is embedded with OpenAI's `text-embedding-ada-002` model and stored in a local **ChromaDB** vector database.
4. **Retrieval** — On each query, the top-k most semantically similar chunks are fetched from ChromaDB.
5. **Answer Generation** — A custom LangChain prompt feeds the retrieved context, chat history, and the user's question into **GPT-3.5 Turbo** to produce a grounded answer.
6. **Conversational Memory** — `RunnableWithMessageHistory` tracks per-session history so follow-up questions retain context.
7. **UI** — Gradio renders the chat experience, including branded styling and pre-built sample questions.

---

##  Tech Stack

| Category | Technology |
|---|---|
| Language | Python 3.10+ |
| LLM Orchestration | LangChain |
| Language Model | OpenAI GPT-3.5 Turbo |
| Embeddings | OpenAI `text-embedding-ada-002` |
| Vector Database | ChromaDB |
| PDF Processing | PyPDFLoader (`pypdf`) |
| Web UI | Gradio |
| Environment | Jupyter Notebook / Anaconda |

---

##  Project Structure

```
nestle-hr-assistant/
├── Nestle_HR_Policy_Documents_Project.ipynb   # Main notebook (data loading → vector DB → QA chain → Gradio UI)
├── data/
│   └── THE_NE_1.PDF                           # Nestlé HR Policy source document
├── docs/
│   ├── Nestle_HR_Project_Writeup.pdf          # Project write-up (STAR format)
│   └── Nestle_HR_Project_Screenshots.pdf      # UI/output screenshots
├── requirements.txt                           # Python dependencies
├── .gitignore                                 # Excludes secrets, vector DB, and notebook checkpoints
└── README.md
```

> Adjust file/folder names above to match your actual repository layout before publishing.

---

##  Installation

### 1. Clone the repository
```bash
git clone https://github.com/<your-username>/nestle-hr-assistant.git
cd nestle-hr-assistant
```

### 2. Create and activate a virtual environment
```bash
conda create -n nestle-hr python=3.10 -y
conda activate nestle-hr
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

**`requirements.txt`**
```
langchain
langchain-community
langchain-openai
langchain-text-splitters
langchain-core
chromadb
pypdf
gradio
openai
nest_asyncio
```

---

##  Configuration

This project requires an OpenAI API key. **The key is never hardcoded** — you'll be prompted to enter it securely at runtime:

```python
import getpass, os
os.environ["OPENAI_API_KEY"] = getpass.getpass("Enter your OpenAI API key: ")
```

You can generate a key from the [OpenAI API dashboard](https://platform.openai.com/api-keys).

>  **Security note:** Never commit API keys to GitHub. If a key is ever accidentally exposed, revoke it immediately from the OpenAI dashboard and generate a new one.

---

##  Usage

1. Open the notebook:
   ```bash
   jupyter notebook Nestle_HR_Policy_Documents_Project.ipynb
   ```
2. Run all cells in order (top to bottom):
   - Imports and environment setup
   - PDF loading and chunking
   - Vector store creation (ChromaDB)
   - QA chain construction
   - Gradio UI launch
3. Once launched, the chatbot will be available at:
   ```
   http://127.0.0.1:7860
   ```
4. Ask questions using the input box, or click any of the 8 pre-built sample questions.

### Example Queries

| Question | Sample Response Summary |
|---|---|
| "How does Nestlé handle promotions and career development?" | Promotions are based on sustained performance and future potential, supported by active succession planning and open career dialogue with line managers. |
| "How does Nestlé handle employee training and development?" | Learning is part of the company culture; responsibility is shared between employees, managers, and HR, with on-the-job training as the primary source of learning. |
| "What are the Total Rewards components at Nestlé?" | Fixed Pay, Variable Pay, Benefits, Personal Growth and Development, and Work Life Environment. |
| "What is Nestlé's policy on hiring employees?" *(out-of-scope example)* | The assistant responds: "I don't have enough information in the Nestlé HR Policy document to answer that question. Please consult the HR department." |

---

##  Limitations

- Answers are limited strictly to the content of the loaded PDF — the assistant will not answer questions outside that scope.
- Currently runs locally (`localhost`) only; not yet deployed to a public cloud environment.
- Single-document knowledge base; does not yet support multiple HR documents simultaneously.

---

##  Future Enhancements

-  Deploy to a cloud platform (AWS / Azure / GCP) for shared team access
-  Add multi-language support
-  Add voice-based interaction
-  Support multiple HR policy documents at once
-  Integrate with live HR databases for real-time data

---

##  License

This project is intended for educational and demonstration purposes. Nestlé's HR Policy document is © Nestec Ltd. and used here solely for non-commercial, academic illustration.

---

## 🙏 Acknowledgments

- [LangChain](https://www.langchain.com/) for the RAG orchestration framework
- [OpenAI](https://platform.openai.com/) for the language model and embeddings
- [Gradio](https://www.gradio.app/) for the interactive chat interface
- Nestlé's publicly available *Human Resources Policy* (September 2012) as the source document
