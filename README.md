<img width="1260" height="267" src="project_documentation_files\logo_1.png" alt="logo" />

# Finchat - AI Fintech Chatbot
**A Turkish Fintech Conversational AI blending Rule-Based systems with GenAI**

# Overview
*   [Explanation of The Project](#explanation-of-the-project)
*   [Details](#details)
    *   [Features](#1-features)
    *   [Tech Stack](#2-tech-stack)
    *   [Workflow](#3-workflow)
    *   [Data Flow](#4-data-flow)
*   [Installation](#installation)
*   [Rasa Conversation Flow Diagram](#Rasa-Conversation-Flow-Diagram)
*   [Explanation Video](#Explanation-Video)
*   [Notes](#notes)

# Explanation of The Project

Finchat is a fintech banking assistant designed to handle transactions accurately while providing fluent, helpful advice on general financial topics. It demonstrates both rule-based and generative AI architecture:

*   **Rule-Based**: Handles critical banking operations.
*   **Generative**: Answers open-ended financial questions using RAG and LLMs.

# Details

## 1. Features

*   **Hybrid Intelligence**: Seamless switching between strict command execution and generative conversation.
*   **Secure Transactions**: Rule-based handling of money transfers and account inquiries.
*   **Advisory Capability**: Context-aware answers to questions like *"How can I improve my credit score?"*.
*   **Modern UI**: Modern chat interface built with Chainlit.
*   **Vector Search**: Efficient retrieval of bank policies using ChromaDB.

## 2. Tech Stack

*   **Frontend**: Chainlit
*   **Dialogue**: Rasa
*   **LLM**: Google Gemini 2.0 Flash
*   **Embeddings**: Sentence-Transformers
*   **Orchestration**: LangChain
*   **Database**: PostgreSQL
*   **API**: FastAPI

## 3. Workflow

1.  **User Input**: User sends a message.
2.  **Intent Classification**: Rasa NLU determines if the user wants to perform an action or ask a general question.
3.  **Routing**:
    *   **Action**: Rasa Core triggers the Action Server -> Calls FastAPI Backend -> Returns structured data.
    *   **Generation**: Action Server routes to RAG Client -> Retrieves docs from ChromaDB -> Generates answer via Gemini.
4.  **Response**: The system constructs the final response and displays it in the frontend.

## 4. Data Flow

*   **User -> UI**: Natural Language.
*   **UI -> Rasa**: JSON payload via REST API.
*   **Rasa -> Action Server**: Executors.
*   **Action Server -> Backend DB**: SQL Queries via FastAPI.
*   **Action Server -> LLM**: Prompt Engineering & Context.

# Installation

### Prerequisites
*   Python 3.10
*   PostgreSQL (Local or Cloud)
*   Google Gemini API Key

### 1. Clone the Repository
```bash
git clone https://github.com/enistuna/Finchat.git
cd Finchat
```

### 2. Install Dependencies
This project uses two separate virtual environments to avoid conflicts between incompatible version dependencies.

#### Environment A: Core
```bash
py -3.10 -m venv venv_core
.\venv_core\Scripts\activate
pip install -r requirements_core.txt
```

#### Environment B: UI

```bash
py -3.10 -m venv venv_ui
.\venv_ui\Scripts\activate
pip install -r requirements_ui.txt
```

### 3. Environment Setup
Create a `.env` file in the root directory:
```ini
GEMINI_API_KEY=YOUR_API_KEY
DATABASE_URL=postgresql://user:password@localhost:5432/findb
RASA_URL=http://localhost:5005
```

### 4. Running the Application
You will need 4 separate terminals. Run it in order and wait for first 3 servers to fully initiate.

**Terminal 1 (Backend) - `venv_core`**
```bash
.\venv_core\Scripts\activate
uvicorn src.backend.main:app --port 8001 &
python -m rasa_sdk --actions actions
```

**Terminal 2 (Actions) -  `venv_core`**
```bash
.\venv_core\Scripts\activate
uvicorn src.backend.main:app --port 8001 &
python -m rasa_sdk --actions actions
```

**Terminal 3 (Rasa) -  `venv_core`**
```bash
.\venv_core\Scripts\activate
rasa run --enable-api --cors "*" --port 5005
```

**Terminal 4 (Frontend) -  `venv_ui`**
```bash
.\venv_ui\Scripts\activate
python scripts/init_chainlit_db.py
cd src/frontend
chainlit run app.py
```

# Rasa Conversation Flow Diagram

<img src="project_documentation_files\rasa_visualize_diagram.png" alt="rasa visualize" />

# Explanation Video
[<img src="project_documentation_files\thumbnail_1.jpg" />](https://www.youtube.com/@enistuna)
*Video is going to be available soon.*

# Notes

This project is the predecessor of [Gənuine](https://github.com/enistuna/Genuine) graduation project. Keep an eye out for more information.
