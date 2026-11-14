# Google ADK Coding Agent

A Google ADK agent that works with files and runs approved commands inside a
local workspace. Optional GitHub MCP tools provide repository, issue, and pull
request access.

## Requirements

- Python 3.10+
- Node.js and npm (`npx` runs the filesystem MCP server)
- A Google AI API key

## Setup

From the project root (the directory containing `agent.py`), create and
activate a virtual environment, then install the dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Set your Google API key before starting the agent:

```powershell
$env:GOOGLE_API_KEY = "your-google-api-key"
```

To enable GitHub tools, also set a GitHub personal access token:

```powershell
$env:GITHUB_PAT = "your-github-token"
```

Never commit API keys or tokens.

## Run

From the same project root:

```powershell
adk run .
```

The agent uses `workspace/` by default and creates it if needed. Set
`WORKSPACE_DIR` to use a different workspace directory.

## Tools and safety

- **Filesystem MCP:** Reads and edits files only within the configured workspace.
- **Safe shell MCP:** Runs allowlisted Python, pytest, pip, and Git commands there.
  Some commands require approval; Git push and other sensitive Git operations
  are blocked.
- **GitHub MCP (optional):** Read tools are enabled with `GITHUB_PAT`; GitHub
  write operations require approval.
- **Local app server:** Starts FastAPI apps on `http://127.0.0.1:8001`.

The agent is configured in [`agent.py`](agent.py), and the shell MCP server is
in [`servers/shell_server.py`](servers/shell_server.py).