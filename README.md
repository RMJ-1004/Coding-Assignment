# Workflow Engine

A minimal but functional workflow/graph engine built using FastAPI.  
It supports node-based execution, conditional edges, looping logic, and a registry of reusable "tools" (Python functions).  
This project includes a sample workflow based on **Option A – Code Review Mini-Agent** from the assignment.

---

## 🚀 How to Run

### 1. Create virtual environment (optional but recommended)

python -m venv .venv

### 2. Activate virtual environment

Windows (PowerShell):

.\.venv\Scripts\Activate.ps1


### 3. Install dependencies
pip install -r requirements.txt

### 4. Start server
uvicorn app.main:app --reload

### 5. Test API

Use Postman, curl, or Swagger UI:
📌 Open in browser → http://127.0.0.1:8000/docs

Example test call:

curl -X POST "http://127.0.0.1:8000/sample/run" \
-H "Content-Type: application/json" \
-d "{\"code\": \"def foo(): pass\ndef bar(): pass\"}"

🧠 What the Engine Supports
✔️ Nodes

Each node represents a Python function.

Functions modify a shared state dictionary (wrapped in a Pydantic State model).

✔️ Edges

Define workflow routing.

Can be:

A simple string → next node

A conditional mapping for branching/loops →

{
  "condition": "quality_score >= 5",
  "then": "end",
  "else": "check_complexity"
}

✔️ State Flow

A central dictionary passed from node to node and updated as the workflow runs.

✔️ Tool Registry

A global dictionary of callable tools.

Nodes reference tools by name.

Tools include:

extract_functions

check_complexity

detect_issues

suggest_improvements

✔️ APIs Provided

POST /graph/create → Create workflow graphs dynamically.

POST /graph/run → Execute a graph with initial state.

GET /graph/state/{run_id} → Fetch logs + final state of a run.

POST /sample/run → Run included sample workflow instantly.

✔️ Sample Workflow (Option A – Code Review Mini-Agent)

This workflow:

Extracts number of functions

Computes code complexity

Detects code issues (long lines, TODOs)

Suggests improvements

Loops until quality_score >= 5

📁 Project Structure
workflow-engine/
│
├── app/
│   ├── main.py            # FastAPI app + endpoints
│   ├── engine.py          # Core workflow engine
│   ├── models.py          # Pydantic models for graph/state
│   ├── tools.py           # Tool registry + tool functions
│   ├── workflows.py       # Sample workflow (Option A)
│   └── __init__.py        # Marks package
│
├── requirements.txt
└── README.md

🔮 What I'd Improve with More Time
🗄️ 1. Persistence (SQLite or PostgreSQL)

Store:

graphs

runs

state logs

So workflows survive server restarts and can be queried historically.

⚙️ 2. Async Engine Execution

Turn node execution asynchronous using:

async def tools

asyncio task scheduling

background task execution

Useful for long-running tools.

🔌 3. WebSockets for Live Log Streaming

Allow clients (Postman/Web UI) to receive logs in real time:

/ws/logs/{run_id} endpoint

similar to procedural workflow debuggers

❗ 4. Improved Error Handling & Validation

Validate graph structure before saving

Validate node names and functions

Retry logic for failing nodes

🔐 5. Safer Condition Evaluation

Replace Python eval() with:

a mini-parser

restricted evaluation

rule-based operator handling

This prevents misuse or malicious expressions.

🎨 6. Extra Ideas (Future Enhancements)

Runtime pause/resume of workflows

Visual graph editor UI (React/JS)

Dockerfile for portable deployment

Unit tests (pytest)

📬 Summary

This project fulfills all core requirements of the assignment:

Implements workflow engine

Supports nodes, edges, branching, looping

Includes a tool registry

Exposes all required API endpoints

Provides a full sample workflow (Option A)

Includes clear documentation and test instructions

For production-ready use, the improvements listed above would complete the engine.
