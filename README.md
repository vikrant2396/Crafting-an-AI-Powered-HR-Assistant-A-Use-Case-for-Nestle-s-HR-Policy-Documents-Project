#  AI-Powered HR Assistant — Nestlé HR Policy Documents

> A conversational chatbot that answers employee queries using Nestlé's HR Policy PDF, built with LangChain, OpenAI GPT-3.5 Turbo, ChromaDB, and Gradio.

---

##  Table of Contents

- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Project Workflow](#project-workflow)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [How to Run](#how-to-run)
- [Features](#features)
- [Sample Questions](#sample-questions)
- [Screenshots](#screenshots)
- [Challenges & Solutions](#challenges--solutions)
- [Future Enhancements](#future-enhancements)
- [Acknowledgements](#acknowledgements)

---

## Project Overview

Nestlé's HR policies are stored in large PDF documents that contain critical information on topics such as recruitment, employee relations, training, total rewards, and performance management. Employees often face challenges locating specific information quickly, leading to inefficiencies and lost productivity.

This project addresses that problem by developing an **AI-powered conversational HR assistant** that:

- Ingests Nestlé's HR Policy PDF document
- Converts text into semantic vector embeddings stored in ChromaDB
- Uses OpenAI's GPT-3.5 Turbo to understand and answer natural-language questions
- Maintains multi-turn conversation history for context-aware responses
- Presents everything through a polished, user-friendly **Gradio** chatbot interface

---

## Tech Stack

| Category | Technology |
|---|---|
| Language | Python 3.13 |
| LLM | OpenAI GPT-3.5 Turbo |
| Embeddings | OpenAI `text-embedding-ada-002` |
| Vector Database | ChromaDB (persistent local store) |
| Framework | LangChain (Community + Core + OpenAI) |
| PDF Loading | LangChain `PyPDFLoader` |
| Text Splitting | `RecursiveCharacterTextSplitter` |
| Chatbot UI | Gradio (`gr.Blocks`) |
| Conversation Memory | `RunnableWithMessageHistory` + `ChatMessageHistory` |

---

## Project Workflow

```
HR Policy PDF
      │
      ▼
 PyPDFLoader          ← Load PDF pages
      │
      ▼
 Text Chunker         ← Split into 1000-char chunks (200 overlap)
      │
      ▼
 OpenAI Embeddings    ← Convert chunks to vectors
      │
      ▼
 ChromaDB             ← Persist vector store locally
      │
      ▼
 Retriever            ← Similarity search (top-k relevant chunks)
      │
      ▼
 PromptTemplate       ← Format context + question for LLM
      │
      ▼
 GPT-3.5 Turbo        ← Generate accurate, grounded answer
      │
      ▼
 Chat History         ← Maintain session memory for follow-up questions
      │
      ▼
 Gradio UI            ← User-facing conversational interface
```

---

## Project Structure

```
nestle-hr-assistant/
│
├── Nestle_s_HR_Policy_Documents_Project.ipynb   # Main Jupyter Notebook
├── THE_NE_1.PDF                                  # Nestlé HR Policy PDF (source document)
├── nestle_hr_chroma_db/                          # ChromaDB persistent vector store (auto-generated)
│   └── ...
└── README.md                                     # Project documentation
```

> **Note:** The `nestle_hr_chroma_db/` directory is created automatically when the notebook is run for the first time.

---

## Prerequisites

- Python **3.9 or higher**
- An active **OpenAI API key** ([Get one here](https://platform.openai.com/api-keys))
- Jupyter Notebook or JupyterLab
- The Nestlé HR Policy PDF file (`THE_NE_1.PDF`) placed in the same directory as the notebook

---

## Installation

**1. Clone the repository**

```bash
git clone https://github.com/<your-username>/nestle-hr-assistant.git
cd nestle-hr-assistant
```

**2. Create and activate a virtual environment (recommended)**

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

**3. Install required dependencies**

```bash
pip install openai langchain langchain-community langchain-openai langchain-text-splitters chromadb gradio pypdf
```

Or install everything at once using:

```bash
pip install openai langchain langchain-community langchain-openai langchain-text-splitters chromadb gradio pypdf
```

---

## Configuration

Open the notebook and locate **Step 2: Configure OpenAI API Key**.

Replace the placeholder with your actual OpenAI API key:

```python
os_env.environ["OPENAI_API_KEY"] = "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"
```

> **Security Warning:** Never commit your API key to a public repository. Consider using a `.env` file with `python-dotenv` for safer key management:
> ```python
> from dotenv import load_dotenv
> load_dotenv()
> # Then set OPENAI_API_KEY in a .env file
> ```

Also verify the PDF path in **Step 3**:

```python
PDF_PATH = "THE_NE_1.PDF"   # Update if your file has a different name or path
```

---

## How to Run

**1. Launch Jupyter Notebook**

```bash
jupyter notebook
```

**2. Open the notebook**

Open `Nestle_s_HR_Policy_Documents_Project.ipynb`.

**3. Run all cells in order**

Go to `Kernel → Restart & Run All`, or run each cell sequentially from top to bottom.

**4. Access the chatbot**

Once Step 12 executes, the Gradio interface will launch at:

```
http://127.0.0.1:7862
```

The notebook will also embed the interface inline within the Jupyter output.

---

## Features

- **PDF-based knowledge system** — All answers are grounded in the actual Nestlé HR Policy document (September 2012), reducing hallucinations
- **Semantic search** — ChromaDB retrieves the most contextually relevant text chunks for each query using vector similarity
- **Conversational memory** — Session-based chat history allows follow-up questions without losing context
- **Prompt engineering** — A custom `PromptTemplate` ensures focused, factual, policy-aligned responses
- **Responsible AI behavior** — When insufficient context is found, the assistant politely directs the user to consult the HR department
- **Nestlé-branded Gradio UI** — Professional dark-theme interface with the Nestlé logo, sample question buttons, Send/Clear controls, and a branded footer
- **Low-temperature LLM** — `temperature=0.2` keeps responses factual and consistent

---

## Sample Questions

The interface includes 8 pre-built quick-access questions:

1. What is Nestlé's employee relations policy?
2. How does Nestlé support work-life balance?
3. What is the role of HR managers at Nestlé?
4. How does Nestlé handle promotions and career development?
5. How does Nestlé manage performance appraisals?
6. What training and development programs does Nestlé offer?
7. What is Nestlé's stance on employee health and safety?
8. What is Nestlé's code of conduct for employees?

---

## Screenshots

### Gradio Chatbot Interface

The Gradio UI launched at `http://127.0.0.1:7862` with a professional dark theme, Nestlé branding, and an embedded conversation panel.

> *(Screenshots available in the project submission documents.)*

---

## Challenges & Solutions

| Challenge | Solution |
|---|---|
| Handling large PDF documents efficiently | Used `RecursiveCharacterTextSplitter` with 1000-char chunks and 200-char overlap for better context continuity |
| Ensuring retrieval accuracy | ChromaDB similarity search retrieves top-4 most relevant chunks per query |
| Avoiding hallucinated answers | Low temperature (0.2) combined with strict prompt instructions to stay within document context |
| Managing multi-turn conversation context | Implemented `RunnableWithMessageHistory` with session-scoped `ChatMessageHistory` |
| Response relevance when context is insufficient | Prompt instructs the model to acknowledge gaps and redirect to the HR department |

---

## Future Enhancements

-  Deploy on cloud platforms (AWS / Azure / GCP) for organization-wide access
-  Add multi-language support for global Nestlé employees
-  Integrate voice-based interaction using speech-to-text APIs
-  Support multiple HR documents simultaneously (benefits guide, code of conduct, etc.)
-  Secure API key management using environment variables and secrets managers
-  Add query logging and analytics dashboard for HR administrators
-  Fine-tune a domain-specific model on Nestlé HR content for improved accuracy

---

## Acknowledgements

- **Nestlé S.A.** — For the publicly available Human Resources Policy document (September 2012)
- **OpenAI** — GPT-3.5 Turbo LLM and text-embedding-ada-002 embeddings
- **LangChain** — Document loading, text splitting, retrieval chains, and memory management
- **ChromaDB** — Open-source vector database for semantic search
- **Gradio** — Rapid, interactive UI development for ML applications
- **IIT Guwahati** — Professional Certificate Program in Generative AI and Machine Learning (Course-End Project)

---

*Built as part of the Course-End Project — Generative AI and Machine Learning Program, IIT Guwahati.*
