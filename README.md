# pico

`pico` is a lightweight local coding agent designed for working directly inside code repositories.

Unlike a pure chat-based assistant, `pico` runs in the terminal, understands the current workspace, uses a controlled set of tools to inspect files, modify code, execute commands, and persist session states locally under the `.pico/` directory.

It works as a continuous coding assistant inside a repository, supporting tasks such as debugging, test fixing, codebase analysis, and small engineering iterations.

---

# What Can Pico Do

Typical use cases:

- Debug failing tests in local repositories
- Analyze existing project structures and provide modification suggestions
- Perform incremental code changes based on the current workspace
- Maintain context across sessions and continue previous tasks

---

# Key Features

- Package name: `pico`
- CLI command: `pico`
- Python module entry:

```bash
python -m pico
```

- Session storage:

```
.pico/sessions/
```

- Runtime artifacts:

```
.pico/runs/<run_id>/
```

- Supported model providers:

  - Ollama
  - OpenAI-compatible Responses API
  - Anthropic-compatible Messages API
  - DeepSeek Anthropic-compatible API

---

# Screenshots

## CLI Help

![pico help](assets/screenshots/pico-help.png)

## Startup Interface

![pico start](assets/screenshots/pico-start.png)

## REPL Commands and Session Information

![pico repl](assets/screenshots/pico-repl.png)

---

# Installation

Requirements:

- Python 3.10+

## Install with uv

```bash
uv sync
```

## Install as Editable Package

If you already have a Python environment:

```bash
pip install -e .
```

---

# Quick Start

Start interactive mode inside the current repository.

Example using DeepSeek:

```bash
uv run pico --provider deepseek
```

Specify another working directory:

```bash
uv run pico --cwd /path/to/repo
```

Run a one-shot task:

```bash
uv run pico --provider deepseek "inspect the test failures and propose a fix"
```

If the package is already installed:

```bash
python -m pico --provider deepseek
```

---

# Model Providers

Pico loads configuration from `.env` in the project root.

Real API keys should only exist in local `.env` files. The repository should only contain `.env.example`.

Configuration priority:

```
CLI arguments
    >
PICO_* environment variables
    >
legacy environment variables
    >
default values
```

Initialize local configuration:

```bash
cp .env.example .env
```

Then configure the required provider keys.

The `.env` file is ignored by `.gitignore` and should never contain committed secrets.

---

# Ollama

Start Ollama:

```bash
ollama serve
```

Pull a model:

```bash
ollama pull qwen3.5:4b
```

Run Pico:

```bash
uv run pico --provider ollama --model qwen3.5:4b
```

---

# OpenAI-Compatible API

Default OpenAI-compatible endpoint:

```bash
PICO_OPENAI_API_BASE="https://www.right.codes/codex/v1"
PICO_OPENAI_API_KEY="your-api-key"
PICO_OPENAI_MODEL="gpt-5.4"
```

Other OpenAI-compatible services are supported:

```bash
PICO_OPENAI_API_BASE="https://your-api.example/v1"
PICO_OPENAI_API_KEY="your-api-key"
PICO_OPENAI_MODEL="gpt-5.4"
```

Run:

```bash
uv run pico --provider openai
```

---

# Anthropic-Compatible API

Default Anthropic-compatible endpoint:

```bash
PICO_ANTHROPIC_API_BASE="https://www.right.codes/claude/v1"
PICO_ANTHROPIC_API_KEY="your-api-key"
PICO_ANTHROPIC_MODEL="claude-sonnet-4-6"
```

Run:

```bash
uv run pico --provider anthropic
```

For compatible services sharing the same credentials, Pico supports fallback lookup from:

```
PICO_ANTHROPIC_API_KEY

ANTHROPIC_API_KEY

PICO_RIGHT_CODES_API_KEY

RIGHT_CODES_API_KEY

PICO_OPENAI_API_KEY

OPENAI_API_KEY
```

---

# DeepSeek

Configuration:

```bash
PICO_DEEPSEEK_API_KEY="your-api-key"

PICO_DEEPSEEK_MODEL="deepseek-v4-pro"
```

Run:

```bash
uv run pico --provider deepseek
```

The default DeepSeek endpoint is:

```
https://api.deepseek.com/anthropic
```

Pico uses DeepSeek's Anthropic-compatible interface.

A custom endpoint can be configured with:

```bash
PICO_DEEPSEEK_API_BASE
```

or:

```bash
--base-url
```

---

# Interactive Commands

Inside Pico REPL:

| Command | Description |
|---|---|
| `/help` | Show available commands |
| `/memory` | Display extracted working memory |
| `/session` | Show current session file path |
| `/reset` | Clear current session state |
| `/exit` | Exit REPL |
| `/quit` | Exit REPL |

---

# Security and Persistence

Pico does not automatically allow unrestricted actions.

High-risk operations such as:

- Shell execution
- File modification

are controlled through approval modes:

```bash
--approval ask
```

```bash
--approval auto
```

```bash
--approval never
```

---

# Runtime Artifacts

After each run, Pico stores execution artifacts locally:

```
.pico/runs/<run_id>/
```

Generated files:

```
task_state.json

trace.jsonl

report.json
```

These files are stored locally by default and do not need to be committed with the repository.

---

# Development

Run code checks with Ruff:

```bash
uv run ruff check .
```

---

# Project Summary

Pico is a lightweight terminal-based coding agent that combines:

- Local repository understanding
- Controlled tool execution
- Persistent session management
- Model provider abstraction
- Safe engineering workflows

It aims to provide a practical coding assistant that can continuously work inside real software projects.
