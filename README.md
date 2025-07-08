# Claude Code Proxy 🔄

**Use Claude Code with OpenAI, Gemini, OpenRouter, Deepseek, and other LLM providers.**

A FastAPI proxy server that translates Anthropic API requests to work with multiple LLM providers, enabling Claude Code to access different models through a unified interface.

![Anthropic API Proxy](pic.png)

## 🌟 Key Features

- ✅ **Multiple Providers** - OpenAI, Gemini, OpenRouter, Deepseek, and OpenAI-compatible APIs
- ✅ **Automatic Model Mapping** - Claude haiku/sonnet automatically map to your preferred provider
- ✅ **Ready-to-Use Configs** - Provider-specific .env files included
- ✅ **Simple Setup** - No complex authentication needed

## Quick Start ⚡

### 1. Setup

```bash
# Clone and install
git clone https://github.com/your-username/open-claude-code.git
cd open-claude-code

# Install uv (if needed)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 2. Configure Provider

Choose a provider and copy its config:

```bash
# OpenAI (default)
cp .env.openai .env

# OpenRouter (access to multiple models)
cp .env.openrouter .env

# Deepseek (great for coding)
cp .env.deepseek .env

# Google Gemini
cp .env.gemini .env

# OpenAI-compatible (Ollama, Together.ai, etc.)
cp .env.openai-compatible .env
```

Edit `.env` and add your API key for the chosen provider.

### 3. Start Server

```bash
uv run uvicorn server:app --host 0.0.0.0 --port 8083
```

### 4. Use with Claude Code

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Connect to proxy
ANTHROPIC_BASE_URL="http://localhost:8083" ANTHROPIC_API_KEY="DUMMY" claude
```

**Optional:** Add an alias to your shell profile:
```bash
alias claude-proxy='ANTHROPIC_BASE_URL="http://localhost:8083" ANTHROPIC_API_KEY="DUMMY" claude'
```

## Supported Providers

| Provider | Models | Use Case |
|----------|--------|----------|
| **OpenAI** | GPT-4o, GPT-4o-mini, o1, o3-mini | General purpose |
| **OpenRouter** | Claude, GPT, Llama, Mistral, 15+ models | Model variety |
| **Deepseek** | deepseek-chat, deepseek-coder | Coding tasks |
| **Gemini** | Gemini 2.5 Pro, Gemini 2.0 Flash | Google's models |
| **OpenAI-Compatible** | Any model | Ollama, Together.ai, Groq, etc. |

## Model Mapping

Claude models automatically map to your configured provider:

| Claude Model | Maps To |
|--------------|---------|
| `haiku` | Your provider's small/fast model |
| `sonnet` | Your provider's large/capable model |

Configure mapping in your `.env` file:
```bash
PREFERRED_PROVIDER="openrouter"  # or openai, gemini, deepseek, openai-compatible
BIG_MODEL="anthropic/claude-3.5-sonnet"    # for sonnet requests
SMALL_MODEL="anthropic/claude-3-haiku"     # for haiku requests
```

## How It Works

The proxy translates between Anthropic and OpenAI API formats:

1. Receives Claude Code requests in Anthropic format
2. Converts to OpenAI format via LiteLLM
3. Sends to your configured provider
4. Converts response back to Anthropic format
5. Returns to Claude Code

## Why Use This?

- **Cost Savings**: Use cheaper providers like Deepseek or local models
- **Model Variety**: Access multiple model families through OpenRouter
- **Simple Setup**: No complex authentication required
- **Flexibility**: Switch providers without changing your workflow

## Contributing

Contributions welcome! Please submit a Pull Request.
