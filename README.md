# Local AI

A lightweight local AI setup using **Docker Compose** and **Open WebUI**, backed by **Ollama** running natively on Windows for Intel Arc GPU acceleration (Docker Model Runner remains available as an alternative).

---

## 🚀 Features

- **Open WebUI**: Rich, modern web interface for chatting with local LLMs (served on port `3000`).
- **Docker Compose AI Models**: Native `models` definitions for pulling and managing models locally.
- **Persistent Data**: Chat history, settings, and documents persisted in named Docker volume `open-webui`.
- **GPU Acceleration on Intel Arc**: Native Ollama with the Vulkan backend (`11434`).
- **Flexible Backend**: Can switch back to Docker Model Runner (`12434`) or any OpenAI-compatible API.

---

## 📋 Current Project State

### Active Model
- **Gemma 4** (`gemma4:e2b`)

### Alternative / Configured Models (available in `docker-compose.yml`)
- **Llama 3.2** (`llama3.2:3B-F16` / `llama3.2:3B-Q4_K_M`)
- **Qwen 2.5 Coder** (`hf.co/Qwen/Qwen2.5-Coder-7B`)

---

## 🛠️ Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (with Docker Compose)
- [Ollama for Windows](https://ollama.com/download) (recent version with Vulkan support)
- Latest Intel Arc graphics driver

### Why not Docker Model Runner?
On Windows, Docker Desktop's *GPU-backed inference* only supports NVIDIA (CUDA) GPUs. On an Intel Arc GPU, `docker model status` shows a `llama.cpp ...-cpu` build, meaning inference runs on the CPU. Native Ollama with Vulkan uses the Arc GPU instead.

### Ollama setup (PowerShell)
```powershell
# Enable the Vulkan backend for Ollama (persists for your user)
setx OLLAMA_VULKAN 1
# Let containers reach Ollama via host.docker.internal
setx OLLAMA_HOST 0.0.0.0
# Quit Ollama from the system tray and start it again, then pull a model:
ollama pull gemma4:e2b
```

Verify:
- `curl http://localhost:11434/api/tags` lists your models.
- While generating, **Task Manager → Performance → GPU (Intel Arc)** shows dedicated GPU memory and Compute usage rising.
- `ollama ps` shows `100% GPU` in the PROCESSOR column.

---

## ⚡ Quick Start

1. **Start Ollama** (see Prerequisites) and pull a model.

2. **Clone the repository**:
   ```bash
   git clone https://github.com/Z377A1/local-ai.git
   cd local-ai
   ```

3. **Start the services**:
   ```bash
   docker compose up -d
   ```

4. **Access Open WebUI**:
   Open your browser and navigate to:
   ```
   http://localhost:3000
   ```

5. **Stop the stack**:
   ```bash
   docker compose down
   ```

---

## ⚙️ Configuration

You can customize `docker-compose.yml` based on your needs:

### Switching Models
With Ollama, pull models on the host (`ollama pull <model>`); they appear in Open WebUI automatically.

To use Docker Model Runner instead, set `OLLAMA_BASE_URL` to port `12434`, then uncomment the `models:` sections (top-level and under `services.open-webui`):

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
| `OLLAMA_BASE_URL` | `http://host.docker.internal:11434` | Native Ollama endpoint (`12434` for Docker Model Runner) |
| `OPENAI_API_BASE_URL` | `http://host.docker.internal:11434/v1` | Optional OpenAI-compatible API base URL |
| `OPENAI_API_KEY` | `not-needed` | API key if using OpenAI-compatible backend |
| `WEBUI_AUTH` | `true` (uncomment `false` to disable) | Enable or disable Open WebUI user login authentication |

---

## 📂 Project Structure

```
local-ai/
├── docker-compose.yml   # Compose specification for Open WebUI and local models
└── README.md            # Project documentation
```
