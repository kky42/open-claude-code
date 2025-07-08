# Claude Code Proxy - Extended Multi-Provider Support 🔄

> **🚀 This is an enhanced fork** that extends the original Claude Code proxy with support for **OpenRouter**, **Deepseek**, and **OpenAI-compatible APIs**, in addition to the original OpenAI and Gemini support.

## 🎯 Why This Fork?

1. **Expanded Provider Support**: The original proxy only supported OpenAI and Gemini. This fork adds support for:
   - **OpenRouter** - Access to Claude, GPT, Llama, and 15+ other models through a single API
   - **Deepseek** - High-performance Chinese LLM with excellent coding capabilities
   - **OpenAI-Compatible APIs** - Support for any API that follows OpenAI's format (Ollama, Together.ai, Groq, etc.)

2. **Simplified Claude Code Integration**: Discovered and documented the simple method to use Claude Code with custom proxies without complex authentication workarounds.

## 🌟 Key Features Added

- ✅ **OpenRouter Integration** - Access 15+ models including Claude, GPT-4, Llama, Mistral, and more
- ✅ **Deepseek Support** - Use Deepseek's powerful chat and coding models
- ✅ **Generic OpenAI-Compatible API Support** - Works with Ollama, Together.ai, Groq, and other providers
- ✅ **Automatic Model Mapping** - Claude haiku/sonnet automatically map to your preferred provider
- ✅ **Provider-Specific Configuration Examples** - Ready-to-use .env files for each provider
- ✅ **Simplified Authentication** - Easy Claude Code integration without complex setup

---

**Use Anthropic clients (like Claude Code) with OpenAI, Gemini, OpenRouter, Deepseek, and OpenAI-compatible backends.** 🤝

A FastAPI proxy server that translates Anthropic API requests to LiteLLM format, enabling Claude Code to work with multiple LLM providers via a unified interface. 🌉


![Anthropic API Proxy](pic.png)

## Quick Start ⚡

### Prerequisites

Choose one or more providers and get the corresponding API keys:

- **OpenAI**: OpenAI API key 🔑
- **Google Gemini**: Google AI Studio API key 🔑
- **OpenRouter**: OpenRouter API key 🔑
- **Deepseek**: Deepseek API key 🔑
- **OpenAI-Compatible**: API key for your chosen provider (Ollama, Together.ai, Groq, etc.) 🔑
- [uv](https://github.com/astral-sh/uv) installed

### Setup 🛠️

1. **Clone this repository**:
   ```bash
   git clone https://github.com/your-username/open-claude-code.git
   cd open-claude-code
   ```

2. **Install uv** (if you haven't already):
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```
   *(`uv` will handle dependencies based on `pyproject.toml` when you run the server)*

3. **Configure Environment Variables**:

   Choose a provider-specific configuration template:

   ```bash
   # For OpenAI (default)
   cp .env.openai .env

   # For OpenRouter (recommended for variety)
   cp .env.openrouter .env

   # For Deepseek (great for coding)
   cp .env.deepseek .env

   # For Google Gemini
   cp .env.gemini .env

   # For OpenAI-compatible APIs (Ollama, Together.ai, etc.)
   cp .env.openai-compatible .env
   ```

   Then edit `.env` and fill in your actual API keys. The configuration options are:

   *   `ANTHROPIC_API_KEY`: (Optional) Needed only if proxying *to* Anthropic models.
   *   `OPENAI_API_KEY`: Your OpenAI API key (Required if using the default OpenAI preference or as fallback).
   *   `GEMINI_API_KEY`: Your Google AI Studio (Gemini) API key (Required if PREFERRED_PROVIDER=google).
   *   `OPENROUTER_API_KEY`: Your OpenRouter API key (Required if PREFERRED_PROVIDER=openrouter).
   *   `DEEPSEEK_API_KEY`: Your Deepseek API key (Required if PREFERRED_PROVIDER=deepseek).
   *   `OPENAI_COMPATIBLE_API_KEY`: Your OpenAI-compatible API key (Required if PREFERRED_PROVIDER=openai-compatible).
   *   `OPENAI_COMPATIBLE_BASE_URL`: Base URL for OpenAI-compatible API (Required if PREFERRED_PROVIDER=openai-compatible).
   *   `PREFERRED_PROVIDER` (Optional): Set to `openai` (default), `google`, `openrouter`, `deepseek`, or `openai-compatible`. This determines the primary backend for mapping `haiku`/`sonnet`.
   *   `BIG_MODEL` (Optional): The model to map `sonnet` requests to. Defaults vary by provider.
   *   `SMALL_MODEL` (Optional): The model to map `haiku` requests to. Defaults vary by provider.

   **Mapping Logic:**
   - If `PREFERRED_PROVIDER=openai` (default), `haiku`/`sonnet` map to `SMALL_MODEL`/`BIG_MODEL` prefixed with `openai/`.
   - If `PREFERRED_PROVIDER=google`, `haiku`/`sonnet` map to `SMALL_MODEL`/`BIG_MODEL` prefixed with `gemini/` *if* those models are in the server's known `GEMINI_MODELS` list (otherwise falls back to OpenAI mapping).
   - If `PREFERRED_PROVIDER=openrouter`, `haiku`/`sonnet` map to `SMALL_MODEL`/`BIG_MODEL` prefixed with `openrouter/` *if* those models are in the server's known `OPENROUTER_MODELS` list (otherwise falls back to OpenAI mapping).
   - If `PREFERRED_PROVIDER=deepseek`, `haiku`/`sonnet` map to `SMALL_MODEL`/`BIG_MODEL` prefixed with `deepseek/` *if* those models are in the server's known `DEEPSEEK_MODELS` list (otherwise falls back to OpenAI mapping).
   - If `PREFERRED_PROVIDER=openai-compatible`, `haiku`/`sonnet` map to `SMALL_MODEL`/`BIG_MODEL` prefixed with `openai-compatible/`.

4. **Run the server**:
   ```bash
   uv run uvicorn server:app --host 0.0.0.0 --port 8083 --reload
   ```
   *(`--reload` is optional, for development)*

## Using with Claude Code 🎮

### Simple Method (Recommended)

1. **Install Claude Code** (if you haven't already):
   ```bash
   npm install -g @anthropic-ai/claude-code
   ```

2. **Connect to your proxy** (no complex authentication needed!):
   ```bash
   ANTHROPIC_BASE_URL="http://localhost:8083" ANTHROPIC_API_KEY="DUMMY" claude
   ```

   That's it! Claude Code will now use your configured provider (OpenRouter, Deepseek, etc.) instead of Anthropic's API.

### Convenient Alias (Optional)

Add this to your shell profile (`.bashrc`, `.zshrc`, etc.) for easy access:

```bash
alias claude-proxy='ANTHROPIC_BASE_URL="http://localhost:8083" ANTHROPIC_API_KEY="DUMMY" claude'
```

Then simply run:
```bash
claude-proxy
   ```

3. **That's it!** Your Claude Code client will now use the configured backend models through the proxy. 🎯

## Provider Configuration Examples 📋

This repository includes ready-to-use configuration files for each supported provider:

- **`.env.openai`** - OpenAI configuration (GPT-4o, GPT-4o-mini)
- **`.env.openrouter`** - OpenRouter configuration (access to Claude, GPT, Llama, and 15+ models)
- **`.env.deepseek`** - Deepseek configuration (deepseek-chat, deepseek-coder)
- **`.env.gemini`** - Google Gemini configuration (Gemini 2.5 Pro, Gemini 2.0 Flash)
- **`.env.openai-compatible`** - Generic OpenAI-compatible API configuration (Ollama, Together.ai, Groq, etc.)

Each file contains:
- Required API keys for that provider
- Recommended model mappings
- Usage examples and comments
- Alternative model options

Simply copy the desired configuration file to `.env` and add your API keys!

## Model Mapping 🗺️

The proxy automatically maps Claude models to the configured provider based on the `PREFERRED_PROVIDER` setting:

| Claude Model | Default Mapping (OpenAI) | Other Provider Examples |
|--------------|---------------------------|-------------------------|
| haiku | openai/gpt-4.1-mini | openrouter/anthropic/claude-3-haiku, deepseek/deepseek-chat |
| sonnet | openai/gpt-4.1 | openrouter/anthropic/claude-3.5-sonnet, deepseek/deepseek-chat |

### Supported Models

#### OpenAI Models
The following OpenAI models are supported with automatic `openai/` prefix handling:
- o3-mini
- o1
- o1-mini
- o1-pro
- gpt-4.5-preview
- gpt-4o
- gpt-4o-audio-preview
- chatgpt-4o-latest
- gpt-4o-mini
- gpt-4o-mini-audio-preview
- gpt-4.1
- gpt-4.1-mini

#### Gemini Models
The following Gemini models are supported with automatic `gemini/` prefix handling:
- gemini-2.5-pro-preview-03-25
- gemini-2.0-flash

#### OpenRouter Models
The following OpenRouter models are supported with automatic `openrouter/` prefix handling:
- anthropic/claude-3.5-sonnet
- anthropic/claude-3-sonnet
- anthropic/claude-3-haiku
- openai/gpt-4o
- openai/gpt-4o-mini
- openai/gpt-4-turbo
- openai/o1-preview
- openai/o1-mini
- google/gemini-pro-1.5
- meta-llama/llama-3.1-405b-instruct
- meta-llama/llama-3.1-70b-instruct
- meta-llama/llama-3.1-8b-instruct
- mistralai/mistral-large
- mistralai/mistral-medium
- cohere/command-r-plus

#### Deepseek Models
The following Deepseek models are supported with automatic `deepseek/` prefix handling:
- deepseek-chat
- deepseek-coder

#### OpenAI-Compatible Models
OpenAI-compatible APIs can use any model names supported by the target API. Configure the base URL using `OPENAI_COMPATIBLE_BASE_URL`.

### Model Prefix Handling
The proxy automatically adds the appropriate prefix to model names:
- OpenAI models get the `openai/` prefix
- Gemini models get the `gemini/` prefix
- OpenRouter models get the `openrouter/` prefix
- Deepseek models get the `deepseek/` prefix
- OpenAI-compatible models get the `openai-compatible/` prefix
- The BIG_MODEL and SMALL_MODEL will get the appropriate prefix based on which provider's model list they're found in

For example:
- `gpt-4o` becomes `openai/gpt-4o`
- `gemini-2.5-pro-preview-03-25` becomes `gemini/gemini-2.5-pro-preview-03-25`
- `anthropic/claude-3.5-sonnet` becomes `openrouter/anthropic/claude-3.5-sonnet`
- `deepseek-chat` becomes `deepseek/deepseek-chat`
- When BIG_MODEL is set to an OpenRouter model, Claude Sonnet will map to `openrouter/[model-name]`

### Customizing Model Mapping

Control the mapping using environment variables in your `.env` file or directly:

**Example 1: Default (Use OpenAI)**
No changes needed in `.env` beyond API keys, or ensure:
```dotenv
OPENAI_API_KEY="your-openai-key"
GEMINI_API_KEY="your-google-key" # Needed if PREFERRED_PROVIDER=google
# PREFERRED_PROVIDER="openai" # Optional, it's the default
# BIG_MODEL="gpt-4.1" # Optional, it's the default
# SMALL_MODEL="gpt-4.1-mini" # Optional, it's the default
```

**Example 2: Prefer Google**
```dotenv
GEMINI_API_KEY="your-google-key"
OPENAI_API_KEY="your-openai-key" # Needed for fallback
PREFERRED_PROVIDER="google"
# BIG_MODEL="gemini-2.5-pro-preview-03-25" # Optional, it's the default for Google pref
# SMALL_MODEL="gemini-2.0-flash" # Optional, it's the default for Google pref
```

**Example 3: Use Specific OpenAI Models**
```dotenv
OPENAI_API_KEY="your-openai-key"
GEMINI_API_KEY="your-google-key"
PREFERRED_PROVIDER="openai"
BIG_MODEL="gpt-4o" # Example specific model
SMALL_MODEL="gpt-4o-mini" # Example specific model
```

**Example 4: Use OpenRouter**
```dotenv
OPENROUTER_API_KEY="sk-or-v1-..."
OPENAI_API_KEY="your-openai-key" # Needed for fallback
PREFERRED_PROVIDER="openrouter"
BIG_MODEL="anthropic/claude-3.5-sonnet"
SMALL_MODEL="anthropic/claude-3-haiku"
```

**Example 5: Use Deepseek**
```dotenv
DEEPSEEK_API_KEY="sk-..."
OPENAI_API_KEY="your-openai-key" # Needed for fallback
PREFERRED_PROVIDER="deepseek"
BIG_MODEL="deepseek-chat"
SMALL_MODEL="deepseek-chat"
```

**Example 6: Use OpenAI-Compatible API**
```dotenv
OPENAI_COMPATIBLE_API_KEY="your-api-key"
OPENAI_COMPATIBLE_BASE_URL="https://api.example.com/v1"
OPENAI_API_KEY="your-openai-key" # Needed for fallback
PREFERRED_PROVIDER="openai-compatible"
BIG_MODEL="your-big-model-name"
SMALL_MODEL="your-small-model-name"
```

## How It Works 🧩

This proxy works by:

1. **Receiving requests** in Anthropic's API format 📥
2. **Translating** the requests to OpenAI format via LiteLLM 🔄
3. **Sending** the translated request to OpenAI 📤
4. **Converting** the response back to Anthropic format 🔄
5. **Returning** the formatted response to the client ✅

The proxy handles both streaming and non-streaming responses, maintaining compatibility with all Claude clients. 🌊

## Why Use This Fork? 🚀

### Cost Savings 💰
- **OpenRouter**: Often cheaper than direct API access, with competitive pricing
- **Deepseek**: Significantly cheaper than OpenAI/Anthropic for many use cases
- **Local APIs**: Use Ollama or other local models for free inference

### Model Variety 🎯
- **OpenRouter**: Access to 15+ different model families in one place
- **Deepseek**: Excellent performance on coding tasks
- **Latest Models**: Access to newest models as they become available

### Simplified Setup ⚡
- **No Complex Auth**: Simple `ANTHROPIC_API_KEY="DUMMY"` bypass discovered
- **Ready-to-Use Configs**: Provider-specific .env files included
- **One Command**: Start using any provider with a single command

### Enterprise Features 🏢
- **OpenAI-Compatible**: Works with internal APIs, Ollama, Together.ai, Groq
- **Flexible Routing**: Mix and match providers for different use cases
- **Fallback Support**: Automatic fallback to OpenAI if preferred provider fails

## Contributing 🤝

Contributions are welcome! Please feel free to submit a Pull Request. 🎁
