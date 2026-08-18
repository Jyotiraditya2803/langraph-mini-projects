# LangGraph + LangChain Learning Project

This repository contains a step-by-step collection of **LangGraph, LangChain, Groq, tool-calling, memory, and LangSmith tracing examples**.

The notebooks progress from a simple state graph to a chatbot, then add tools, tool-result routing, persistent conversation memory, and LangSmith tracing.

> **Important:** This project is primarily a collection of Jupyter notebooks. The supplied `main(3).py` is only the default PyCharm sample script and is **not** the implementation of the chatbot or LangGraph project.

---

## What This Project Demonstrates

The examples build concepts progressively:

1. **Simple LangGraph**
   - State definition with `TypedDict`
   - Nodes
   - Edges
   - `START` / `END`
   - Graph compilation and invocation

2. **Conditional LangGraph**
   - Multiple graph nodes
   - Conditional routing
   - Currency conversion example

3. **Basic Chatbot**
   - Groq LLM
   - LangGraph state
   - Message history
   - Interactive terminal chat

4. **Tool Calling**
   - `@tool`
   - Binding tools to the LLM
   - `ToolNode`
   - `tools_condition`

5. **Tool-Calling Agent Loop**
   - Tool execution
   - Returning tool results to the chatbot
   - Repeating the chatbot/tool cycle

6. **Memory**
   - `MemorySaver`
   - Checkpointing
   - `thread_id`
   - Conversation state across turns

7. **LangSmith**
   - `@traceable`
   - LLM/graph tracing
   - Environment configuration for tracing

The supplied notebooks use the Groq model:

```text
groq:llama-3.3-70b-versatile
```

and the tool-calling examples use a simple stock-price tool with sample values for `MSFT`, `AAPL`, `AMZN`, and `RIL`.

---

# Project Files

```text
.
├── simple_graph(2).ipynb
├── 2_graph_langraph(2).ipynb
├── 3_chatbot(2).ipynb
├── 4_with_call_tool(2).ipynb
├── 5_tool_return_agent(2).ipynb
├── 6_memory(2).ipynb
├── langsmith(2).ipynb
├── sdfgh(2).ipynb
└── main(3).py
```

### File-by-file purpose

| File | Purpose |
|---|---|
| `simple_graph(2).ipynb` | Basic sequential LangGraph example |
| `2_graph_langraph(2).ipynb` | Conditional graph with INR/EUR routing |
| `3_chatbot(2).ipynb` | Basic Groq chatbot using LangGraph |
| `4_with_call_tool(2).ipynb` | Chatbot with a callable stock-price tool |
| `5_tool_return_agent(2).ipynb` | Tool result returned to the chatbot for another reasoning step |
| `6_memory(2).ipynb` | Tool-calling chatbot with `MemorySaver` |
| `langsmith(2).ipynb` | LangSmith tracing using `@traceable` |
| `sdfgh(2).ipynb` | Checks Groq/LangSmith environment variables |
| `main(3).py` | Default PyCharm sample script; not the chatbot |

---

# Requirements

Recommended:

- Python 3.10+
- Jupyter Notebook or JupyterLab
- Internet connection
- Groq API key
- Optional LangSmith API key for tracing

The code imports packages from:

```text
langchain
langchain-groq
langgraph
langchain-core
langsmith
python-dotenv
```

Because the supplied project does not include a `requirements.txt`, install the packages directly.

---

# Complete Installation Process

## Step 1 — Open the Project

Open a terminal in the project directory:

```bash
cd path/to/project
```

---

## Step 2 — Create a Virtual Environment

### Windows

```bash
python -m venv .venv
```

Activate it:

```bash
.venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv .venv
```

Activate it:

```bash
source .venv/bin/activate
```

---

## Step 3 — Upgrade pip

```bash
python -m pip install --upgrade pip
```

---

## Step 4 — Install the Required Packages

Install the packages used by the notebooks:

```bash
pip install langchain langchain-groq langgraph langchain-core langsmith python-dotenv jupyter ipython
```

If you want JupyterLab instead:

```bash
pip install jupyterlab
```

---

# Step 5 — Create the `.env` File

Create a file named:

```text
.env
```

in the project root.

For the Groq examples:

```env
GROQ_API_KEY=your_groq_api_key_here
```

For the LangSmith example, add:

```env
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=your_langsmith_api_key_here
LANGCHAIN_PROJECT=langgraph-learning-project
```

A typical `.env` file is therefore:

```env
GROQ_API_KEY=your_groq_api_key_here

LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=your_langsmith_api_key_here
LANGCHAIN_PROJECT=langgraph-learning-project
```

**Never commit your real API keys to GitHub.**

The supplied `sdfgh(2).ipynb` checks `GROQ_API_KEY`, `LANGCHAIN_API_KEY`, and `LANGCHAIN_TRACING_V2`, so this is the configuration expected by the project.

---

# Step 6 — Verify Environment Variables

Before running the LLM notebooks, open:

```text
sdfgh(2).ipynb
```

Run its cell.

It checks:

```text
GROQ KEY
LANGSMITH KEY
TRACING
```

You should see that the values are found.

Do not print or share the complete secret values.

---

# How to Run the Project Properly

There is **no single `python main.py` command that starts this project**.

The project is organized as a learning sequence of notebooks.

The recommended order is:

```text
1. simple_graph(2).ipynb
          ↓
2. 2_graph_langraph(2).ipynb
          ↓
3. 3_chatbot(2).ipynb
          ↓
4. 4_with_call_tool(2).ipynb
          ↓
5. 5_tool_return_agent(2).ipynb
          ↓
6. 6_memory(2).ipynb
          ↓
7. langsmith(2).ipynb
```

This order makes the progression easier to understand.

---

# Step 7 — Start Jupyter

From the project directory:

```bash
jupyter notebook
```

or:

```bash
jupyter lab
```

Your browser will open the Jupyter interface.

Open the notebooks from the project directory.

---

# Notebook 1 — `simple_graph(2).ipynb`

This is the simplest LangGraph example.

It defines a state:

```python
class portfoliostate(TypedDict):
    amount_usd: float
    total_usd: float
    total_inr: float
```

The graph contains two processing nodes:

```text
START
  ↓
cal total
  ↓
convert_to_inr
  ↓
END
```

The graph is invoked with:

```python
graph.invoke({"amount_usd": 100})
```

The first node calculates:

```text
total_usd = amount_usd × 1.08
```

The second node calculates:

```text
total_inr = amount_usd × 92
```

### How to run it

Open:

```text
simple_graph(2).ipynb
```

Run the cells from top to bottom.

---

# Notebook 2 — `2_graph_langraph(2).ipynb`

This notebook introduces **conditional routing**.

The state contains:

```python
class portfoliostate(TypedDict):
    amount_usd: float
    total_usd: float
    target_currency: Literal["INR", "EUR"]
    total: float
```

The graph is:

```text
             START
               ↓
           cal_total
               ↓
       choose_conversion
          ↙          ↘
       INR            EUR
        ↓              ↓
       END            END
```

The routing function returns:

```python
state["target_currency"]
```

The graph therefore chooses either:

```text
convert_to_inr
```

or:

```text
convert_to_eur
```

Example:

```python
graph.invoke({
    "amount_usd": 100,
    "target_currency": "EUR"
})
```

The example uses fixed demonstration conversion values; they are not live exchange rates.

---

# Notebook 3 — `3_chatbot(2).ipynb`

This notebook introduces the LLM chatbot.

It loads environment variables:

```python
load_dotenv()
```

and initializes:

```python
llm = init_chat_model(
    "groq:llama-3.3-70b-versatile"
)
```

The graph contains one node:

```text
START
  ↓
Chatbot
  ↓
END
```

The state stores messages:

```python
class state(TypedDict):
    messages: Annotated[list, add_messages]
```

### Run it

Open:

```text
3_chatbot(2).ipynb
```

Run all cells from top to bottom.

The notebook also contains an interactive loop:

```text
enter the question:
```

You can type:

```text
hello
```

or:

```text
What is LangGraph?
```

To stop:

```text
quit
```

or:

```text
exit
```

---

# Notebook 4 — `4_with_call_tool(2).ipynb`

This notebook adds **tool calling**.

The project defines:

```python
@tool
def get_stock_price(symbol: str) -> float:
```

The sample data is:

```text
MSFT → 200.3
AAPL → 100.4
AMZN → 150.0
RIL  → 87.6
```

The LLM is connected to the tool with:

```python
llm_with_tools = llm.bind_tools(tools)
```

The graph contains:

```text
START
  ↓
Chatbot
  ↓
tools_condition
  ↓
ToolNode
```

### Example question

After running the notebook, try:

```text
What is the stock price of AAPL?
```

or:

```text
What is the stock price of RIL?
```

The model can decide to call the stock-price tool.

---

# Notebook 5 — `5_tool_return_agent(2).ipynb`

This notebook improves the tool-calling flow.

It adds an edge:

```text
tools → Chatbot
```

The resulting flow is:

```text
              START
                ↓
             Chatbot
                ↓
         tools_condition
            ↙       ↘
        tools       END
          ↓
       Chatbot
```

This allows the tool result to be returned to the chatbot so the LLM can produce a final natural-language answer.

### Example

Ask:

```text
What is the stock price of MSFT?
```

The flow is approximately:

```text
User question
     ↓
Chatbot
     ↓
Tool call
     ↓
get_stock_price("MSFT")
     ↓
Tool result: 200.3
     ↓
Chatbot
     ↓
Final answer
```

Run the notebook cells from top to bottom.

---

# Notebook 6 — `6_memory(2).ipynb`

This notebook adds conversation memory using:

```python
MemorySaver()
```

The graph is compiled with:

```python
graph = builder.compile(checkpointer=memory)
```

A thread is identified using:

```python
config = {
    "configurable": {
        "thread_id": "1"
    }
}
```

The graph is then invoked with that configuration.

This allows the graph to maintain checkpointed conversation state for the thread.

### Run it

Open:

```text
6_memory(2).ipynb
```

Run all cells.

Then try multiple questions in the interactive loop.

Use the same `thread_id` when you want the same conversation thread.

---

# Notebook 7 — `langsmith(2).ipynb`

This notebook adds LangSmith tracing.

It imports:

```python
from langsmith import traceable
```

and decorates the chatbot node:

```python
@traceable(name="Chatbot Node")
def chatbot(state: state) -> state:
```

To use this notebook, configure:

```env
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=your_langsmith_api_key_here
LANGCHAIN_PROJECT=langgraph-learning-project
```

Then run the notebook normally.

The chatbot node can be traced through LangSmith.

---

# Running the Interactive Chatbot

The chatbot notebooks use a terminal-style input loop.

The basic pattern is:

```text
enter the question:
```

For example:

```text
enter the question: hello
bot: ...
```

Then:

```text
enter the question: what is the stock price of AAPL?
bot: ...
```

Exit with:

```text
quit
```

or:

```text
exit
```

---

# `main(3).py`

The supplied `main(3).py` is only the default PyCharm-generated sample:

```python
def print_hi(name):
    print(f'Hi, {name}')
```

Running:

```bash
python main(3).py
```

will only print:

```text
Hi, PyCharm
```

It does **not** start the LangGraph chatbot.

Therefore, use the Jupyter notebooks for this project rather than treating `main(3).py` as the application entry point.

---

# Recommended Execution Order

For learning the project from beginning to end:

### 1. Understand basic state graphs

```text
simple_graph(2).ipynb
```

### 2. Learn conditional routing

```text
2_graph_langraph(2).ipynb
```

### 3. Build a basic chatbot

```text
3_chatbot(2).ipynb
```

### 4. Add a tool

```text
4_with_call_tool(2).ipynb
```

### 5. Create the tool-return loop

```text
5_tool_return_agent(2).ipynb
```

### 6. Add memory

```text
6_memory(2).ipynb
```

### 7. Add LangSmith tracing

```text
langsmith(2).ipynb
```

This sequence follows the actual progression represented by the supplied files.

---

# Troubleshooting

## `GROQ_API_KEY` not found

Check that `.env` exists in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

Restart the Jupyter kernel after changing environment variables.

Then run:

```text
sdfgh(2).ipynb
```

to verify the environment.

---

## `LANGCHAIN_API_KEY` not found

Only required for the LangSmith example.

Add:

```env
LANGCHAIN_API_KEY=your_langsmith_api_key_here
```

and:

```env
LANGCHAIN_TRACING_V2=true
```

---

## `langchain_groq` import error

Activate the virtual environment and install:

```bash
pip install langchain-groq
```

The supplied tool notebook also contains a diagnostic check for whether `langchain_groq` is installed.

---

## Jupyter command not found

Install Jupyter:

```bash
pip install jupyter
```

Then:

```bash
jupyter notebook
```

---

## Notebook does not see `.env`

Make sure Jupyter is started from the project root:

```bash
cd path/to/project
jupyter notebook
```

This keeps the `.env` file in the expected working directory.

---

## Graph visualization does not display

The notebooks use:

```python
from IPython.display import Image, display
```

and:

```python
graph.get_graph().draw_mermaid_png()
```

This visualization is intended to run inside Jupyter/IPython.

If the graph image is not displayed, continue with the graph execution cells; the visualization is not required for the graph logic itself.

---

# Security

Do not commit:

```text
.env
```

to Git.

A recommended `.gitignore` entry is:

```gitignore
.env
.venv/
__pycache__/
.ipynb_checkpoints/
```

Never publish:

```text
GROQ_API_KEY
LANGCHAIN_API_KEY
```

---

# Quick Start

For a completely fresh setup:

```bash
# 1. Go to the project
cd path/to/project

# 2. Create environment
python -m venv .venv

# 3. Windows activation
.venv\Scripts\activate

# 4. Install packages
pip install langchain langchain-groq langgraph langchain-core langsmith python-dotenv jupyter ipython

# 5. Create .env
# GROQ_API_KEY=your_groq_api_key_here

# 6. Start Jupyter
jupyter notebook
```

Then run the notebooks in this order:

```text
simple_graph(2).ipynb
2_graph_langraph(2).ipynb
3_chatbot(2).ipynb
4_with_call_tool(2).ipynb
5_tool_return_agent(2).ipynb
6_memory(2).ipynb
langsmith(2).ipynb
```

---

# Summary

This project demonstrates the progression:

```text
Simple Graph
     ↓
Conditional Graph
     ↓
LLM Chatbot
     ↓
Tool Calling
     ↓
Tool Result Loop
     ↓
Conversation Memory
     ↓
LangSmith Tracing
```

It is best understood as a **LangGraph learning project**, not as a single standalone application. The notebooks are the primary implementation, while `main(3).py` is only a default sample script.
