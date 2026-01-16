# PROJECT: Finchat

> **A Hybrid Conversational AI blending Rule-Based Control (Rasa) with Generative Intelligence (Gemini + RAG).**

# Overview

*   [Explanation of The Project](#explanation-of-the-project)
*   [Details](#details)
    *   [Features](#1-features)
    *   [Tech Stack](#2-tech-stack)
    *   [Workflow](#3-workflow)
    *   [Data Flow](#4-data-flow)
*   [Use Case Scenarios](#use-case-scenarios)
*   [Installation](#installation)
*   [Resources](#resources)
*   [Citation](#citation)
*   [Notes](#notes)

# Explanation of The Project

FinChat is a next-generation banking assistant designed to handle secure transactions accurately while providing fluent, helpful advice on general financial topics. It demonstrates the **"Left Brain / Right Brain" architecture**:

*   **Left Brain (Deterministic)**: Handles critical banking operations like transfers and balance checks with zero hallucination.
*   **Right Brain (Generative)**: Answers open-ended financial questions using RAG (Retrieval Augmented Generation) and LLMs.

# Details

## 1. Features

*   **Hybrid Intelligence**: Seamless switching between strict command execution and generative conversation.
*   **Secure Transactions**: Rule-based handling of money transfers and account inquiries.
*   **Advisory Capability**: Context-aware answers to questions like *"How can I improve my credit score?"*.
*   **Modern UI**: Premium, responsive chat interface built with Chainlit.
*   **Vector Search**: Efficient retrieval of bank policies using ChromaDB.

## 2. Tech Stack

*   **Frontend**: Chainlit
*   **NLU & Dialogue**: Rasa 3.x
*   **LLM**: Google Gemini 2.0 Flash
*   **Embeddings**: Sentence-Transformers (`all-MiniLM-L6-v2`)
*   **Orchestration**: LangChain
*   **Database**: PostgreSQL
*   **API**: FastAPI (Transaction Simulation)

## 3. Workflow

1.  **User Input**: User sends a message via Chainlit.
2.  **Intent Classification**: Rasa NLU determines if the user wants to perform an action (e.g., `check_balance`) or ask a question (e.g., `ask_advice`).
3.  **Routing**:
    *   **Action**: Rasa Core triggers the Action Server -> Calls FastAPI Backend -> Returns structured data.
    *   **Generation**: Action Server routes to RAG Client -> Retrieves docs from ChromaDB -> Generates answer via Gemini.
4.  **Response**: The system constructs the final response and displays it in the UI.

## 4. Data Flow

*   **User -> UI**: Natural Language.
*   **UI -> Rasa**: JSON payload via REST API.
*   **Rasa -> Action Server**: Executor calls.
*   **Action Server -> Backend DB**: SQL Queries (via FastAPI).
*   **Action Server -> LLM**: Prompt Engineering + Context.

# Use Case Scenarios

**Scenario 1: Transactional (Left Brain)**
> **User**: "Enis'e 500 TL gönder."
> **Bot**: (Confirms recipient and amount, checks balance, executes transfer via API, returns success receipt).

**Scenario 2: Advisory (Right Brain)**
> **User**: "Enflasyon paramı nasıl etkiler?"
> **Bot**: (Retrieves educational content from vector store, uses Gemini to explain inflation impact on savings).

**Scenario 3: Hybrid**
> **User**: "Bakiyem ne kadar?" (Transactional)
> **User**: "Bu parayı vadeli hesaba yatırsam ne kadar kazanırım?" (Advisory based on retrieved balance).

# Installation

### Prerequisites
*   Python 3.10+
*   PostgreSQL (Local or Cloud)
*   Google Gemini API Key

### 1. Clone the Repository
```bash
git clone https://github.com/Start-Ops/Finchat.git
cd Finchat
```

### 2. Install Dependencies
This project uses **Two Separate Environments** to avoid version conflicts between Rasa (Core) and Chainlit (UI).

#### Environment A: Core (Rasa + Backend)
```bash
python -m venv venv_core
.\venv_core\Scripts\activate
pip install -r requirements_core.txt
```

#### Environment B: UI (Chainlit)
(Open a new terminal)
```bash
python -m venv venv_ui
.\venv_ui\Scripts\activate
pip install -r requirements_ui.txt
```

### 3. Environment Setup
Create a `.env` file in the root directory:
```ini
GEMINI_API_KEY=your_api_key_here
DATABASE_URL=postgresql://user:password@localhost:5432/findb
RASA_URL=http://localhost:5005
```

### 4. Run the Application
You will need 3 separate terminals.

**Terminal 1 (Backend & Rasa Actions) - uses `venv_core`**
```bash
.\venv_core\Scripts\activate
uvicorn src.backend.main:app --port 8001 &
python -m rasa_sdk --actions actions
```

**Terminal 2 (Rasa Core) - uses `venv_core`**
```bash
.\venv_core\Scripts\activate
rasa run --enable-api --cors "*" --port 5005
```

**Terminal 3 (Frontend) - uses `venv_ui`**
```bash
.\venv_ui\Scripts\activate
cd src/frontend
chainlit run app.py
```

# Resources

*   [Rasa Documentation](https://rasa.com/docs/)
*   [Chainlit Documentation](https://docs.chainlit.io)
*   [Google Gemini API](https://ai.google.dev/)
*   [FastAPI](https://fastapi.tiangolo.com/)

# Citation

If you use this project, please cite:
*   **Author**: Enis Tuna
*   **Repository**: [Finchat](https://github.com/Start-Ops/Finchat)

# Notes

**This project is the Predecessor of [Gənuine](https://github.com/enistuna/Genuine) graduation project.**
