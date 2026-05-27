<img width="1260" height="267" src="project_documentation_files\logo_1.png" alt="logo" />

# Finchat - AI Fintech Chatbot
**A Turkish Fintech Conversational AI that blends Rule-Based systems with GenAI**

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

Finchat is a fintech conversational AI designed to handle transactions accurately while providing fluent, helpful advice on general financial topics. It demonstrates both rule-based and generative AI architecture:

*   **Rule-Based**: Handles critical banking operations.
*   **Generative**: Answers open-ended financial questions using RAG and LLMs.

# Details

## 1. Features

*   Hybrid architecture. Switching between strict command execution and generative conversation.
*   Rule-based handling of money transfers and account inquiries.
*   Context aware answers to questions like *"How can I improve my credit score?"*.
*   Modern chat interface built with Chainlit.
*   Efficient retrieval of bank policies using ChromaDB.

## 2. Tech Stack

*   **Python**
*   **Rasa**
*   **PostgreSQL**
*   **FastAPI**
*   **Chainlit**
*   **Google Gemini API**
*   **Transformers**
*   **LangChain**

## 3. Workflow

1.  **User Input**: User sends a message.
2.  **Intent Classification**: Rasa NLU determines if the user wants to perform an action or ask a general question.
3.  **Routing**:
    *   **Action**: Rasa Core triggers the Action Server >> Calls FastAPI Backend >> Returns structured data.
    *   **Generation**: Action Server routes to RAG Client >> Retrieves docs from ChromaDB >> Generates answer via Gemini.
4.  **Response**: The system constructs the final response and displays it in the frontend.

## 4. Data Flow

*   **User** >> **UI** >> **Rasa** (JSON payload via REST API) >> **Action Server** (Executors)
    *   **Action Server** >> **Backend DB** (SQL Queries via FastAPI)
    *   **Action Server** >> **LLM** (Context)

# Installation

### Prerequisites
*   **Python 3.10**
*   **PostgreSQL (Local or Cloud)**
*   **[Google Gemini API Key](aistudio.google.com/api-keys)**

### 1. Clone the Repository
```bash
git clone https://github.com/enistuna/Finchat.git
cd Finchat/finchat_code
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
```

**Terminal 2 (Actions) -  `venv_core`**
```bash
.\venv_core\Scripts\activate
python -m rasa run actions --actions src.rasa.actions
```

**Terminal 3 (Rasa) -  `venv_core`**
```bash
.\venv_core\Scripts\activate
python -m rasa run --enable-api --cors "*" --model src/rasa/models --endpoints src/rasa/endpoints.yml --credentials src/rasa/credentials.yml
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
[<img src="project_documentation_files\thumbnail_1.jpg" />](https://youtu.be/kLQ4ITD405A?si=LtObTiRTFb3Zzk2f)

# Notes

* This project is the predecessor of ***[Gənuine](https://github.com/enistuna/Genuine)*** graduation project. Please make sure to check *Gənuine v1* out to see an improved version of this project.
* For any question, contribution or inquiry, [send me an email](mailto:enissstuna@gmail.com).
