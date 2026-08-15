# Local AI

A lightweight, containerized local AI setup using **Docker Compose**, **Open WebUI**, and Docker Model Runner.

---

## 🚀 Features

- **Open WebUI**: Rich, modern web interface for chatting with local LLMs (served on port `3000`).
- **Docker Compose AI Models**: Native `models` definitions for pulling and managing models locally.
- **Persistent Data**: Chat history, settings, and documents persisted in named Docker volume `open-webui`.
- **Flexible Backend**: Compatible with both Ollama API endpoints (`12434`) and OpenAI-compatible API specifications.

---

## 📋 Current Project State

### Active Model
- **Gemma 4** (`gemma4:e2b`)

### Alternative / Configured Models (available in `docker-compose.yml`)
- **Llama 3.2** (`llama3.2:3B-F16` / `llama3.2:3B-Q4_K_M`)
- **Qwen 2.5 Coder** (`hf.co/Qwen/Qwen2.5-Coder-7B`)

---

## 🛠️ Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (with Docker Compose and Docker Model Runner support)

---

## ⚡ Quick Start

1. **Clone the repository**:
   ```bash
   git clone https://github.com/Z377A1/local-ai.git
   cd local-ai
   ```

2. **Start the services**:
   ```bash
   docker compose up -d
   ```

3. **Access Open WebUI**:
   Open your browser and navigate to:
   ```
   http://localhost:3000
   ```

4. **Stop the stack**:
   ```bash
   docker compose down
   ```

---

## ⚙️ Configuration

You can customize `docker-compose.yml` based on your needs:

### Switching Models
Uncomment or add models under the `models:` section and link them under `services.open-webui.models`:

```yaml
models:
  gemma4:
    model: gemma4:e2b
  # llama3-2:
  #   model: llama3.2:3B-F16
  # qwen2-5-coder:
  #   model: hf.co/Qwen/Qwen2.5-Coder-7B
```

### Environment Variables

| Variable | Default / Example | Description |
| :--- | :--- | :--- |
| `OLLAMA_BASE_URL` | `http://host.docker.internal:12434` | Endpoint for Ollama / Docker Model Runner backend |
| `OPENAI_API_BASE_URL` | `http://host.docker.internal:12434/v1` | Optional OpenAI-compatible API base URL |
| `OPENAI_API_KEY` | `not-needed` | API key if using OpenAI-compatible backend |
| `WEBUI_AUTH` | `true` (uncomment `false` to disable) | Enable or disable Open WebUI user login authentication |

---

## 📂 Project Structure

```
local-ai/
├── docker-compose.yml   # Compose specification for Open WebUI and local models
└── README.md            # Project documentation
```
