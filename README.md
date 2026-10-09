# Building a Multi-Agent AI System with LangGraph

A multi-agent AI system built with **LangGraph, LangChain, and Python**. The application uses conditional routing to direct user requests to specialized agents for SEO blog writing, X (Twitter) post generation, and general conversations.

## 🚀 Project Overview

This project demonstrates how to build a stateful multi-agent workflow with LangGraph. A router classifies each user request and directs it to the appropriate agent. Specialized agents can call tools when needed and continue processing until they produce a final response.

## ✨ Features

- **Conditional Routing:** Directs requests to the appropriate agent.
- **SEO Blog Writer:** Generates long-form blog content using research and internet-search tools.
- **X (Twitter) Writer:** Generates concise social media posts with a target of fewer than 280 characters.
- **General Handler:** Answers general questions using conversation context.
- **Tool-Calling Loops:** Allows agents to call tools, receive results, and decide whether further tool calls are needed.
- **Conversation Checkpointing:** Uses LangGraph's `MemorySaver` to preserve graph state between invocations that share a thread ID.
- **Functional Testing:** Tests all three routes and checks the X post's character count.

## 🏗️ Architecture

The workflow follows this structure:

```text
                         START
                           |
                         Router
                           |
             Conditional Routing
                  /        |        \
                 /         |         \
        SEO Blog Writer  X Writer  General Handler
               |             |            |
          tools_condition tools_condition |
            /       \       /      \      |
        SEO Tools   END  X Tools    END    END
            |               |
            v               v
        SEO Writer       X Writer
```

The router selects one agent for each request. The SEO and X writers can enter their respective tool-calling loops. The General Handler responds directly and then ends the graph.

## 🛠️ Technology Stack

- Python
- LangGraph
- LangChain
- LangChain OpenAI integration
- OpenAI API
- Tavily Search
- SerpAPI Google Search
- Jupyter Notebook

## 📂 Project Structure

```text
building-a-multi-agent-ai-system-with-langgraph/
│
├── multi_agent_system.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

*Adjust the notebook filename if yours is different.*

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/building-a-multi-agent-ai-system-with-langgraph.git
cd building-a-multi-agent-ai-system-with-langgraph
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install -r requirements.txt
```

If you do not have a `requirements.txt` yet, install the packages used by the project:

```bash
python -m pip install langgraph langchain langchain-openai tavily-python google-search-results python-dotenv
```

## 🔑 Environment Variables

Configure your API keys in environment variables. Do not hardcode API keys in source code or commit them to GitHub.

Example `.env` file for local development:

```text
OPENAI_API_KEY=your_openai_api_key
Tavily_API_KEY=your_tavily_api_key
SERP_API_KEY=your_serpapi_api_key
```

If using a `.env` file, load it with `python-dotenv` before initializing the API clients. Alternatively, configure these variables in your operating system.

**Security:** Keep `.env` out of version control.

## ▶️ Running the Project

1. Open `multi_agent_system.ipynb` in Jupyter Notebook or VS Code.
2. Configure your API keys.
3. Execute the notebook cells in order:
   - Initialize the language model.
   - Define the shared state and router.
   - Create the research and search tools.
   - Define the three agents.
   - Build the graph and conditional edges.
   - Compile the graph with a checkpointer.
   - Run the test cases.

## 🧪 Example Test Cases

| Agent | Example request | Expected behavior |
|---|---|---|
| SEO Blog Writer | Write an SEO blog about the benefits of learning LangGraph. | Generates long-form blog content. |
| X Blog Writer | Write an engaging X post about LangChain. | Generates a short social media post and checks its character count. |
| General Handler | What are edges? Explain in simple words. | Provides a simple explanation. |

The X post test checks whether the generated output is fewer than 280 characters.

## 💾 Conversation Memory

The graph uses LangGraph's `MemorySaver` checkpointer:

```python
from langgraph.checkpoint.memory import MemorySaver

memory = MemorySaver()
graph = builder.compile(checkpointer=memory)
```

Use a consistent thread ID across invocations to access the same checkpointed conversation.

**Note:** `MemorySaver` stores state in memory. Checkpoints are lost when the Python process ends,
