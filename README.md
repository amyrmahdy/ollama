Here’s a concise `README.md` for your project:

---

# Open WebUI + Ollama Docker Setup

A Docker Compose setup for running **Ollama** and **Open WebUI** with persistent storage.

## 🐋 Quick Start
1. Create required directories:
   ```bash
   mkdir -p ./ollama_data ./models ./webui_data
   ```
2. Start services:
   ```bash
   docker-compose up -d
   ```
3. Access Open WebUI:  
   → [http://localhost:3000](http://localhost:3000)

---

## 🔧 Key Configuration

### 1. Fixing `host.docker.internal` on Linux
Open WebUI hardcodes `host.docker.internal` for Ollama connections. To override this:
1. Go to **Settings** → **Connections** in Open WebUI ([http://localhost:3000/admin/settings](http://localhost:3000/admin/settings)).
2. Set **Ollama Base URL** to:
   - `http://ollama:11434` (if using Docker networking, default)
   - `http://localhost:11434` (if using `network_mode: host`)

### 2. Persistent Directories
| Directory       | Purpose                          |
|-----------------|----------------------------------|
| `./ollama_data` | Ollama's configuration & cache   |
| `./models`      | Custom GGUF model files          |
| `./webui_data`  | Open WebUI conversations/settings|

---

## 🧩 Custom Model Example
To create a model from a GGUF file in `./models`:

1. Place your model file (e.g., `gemma-3-4b-it.Q2_K.gguf`) in `./models`
2. Create a Modelfile:
   ```bash
   echo 'FROM /models/gemma-3-4b-it.Q2_K.gguf
   TEMPLATE """{{ .System }}
   {{ .Prompt }}"""
   PARAMETER num_ctx 4096' > ./models/GemmaQ2.Modelfile
   ```
3. Create the model:
   ```bash
   docker exec ollama ollama create gemmaq2 -f /models/GemmaQ2.Modelfile
   ```

---

## 🚀 Usage
- **Access Ollama API**: `http://localhost:11434`
- **Open WebUI Docs**: [https://docs.openwebui.com/](https://docs.openwebui.com/)

![Architecture](https://docs.openwebui.com/img/architecture.png)  
*(Default Docker networking setup)*

--- 

💡 **Tip**: Use `docker-compose logs -f` to monitor logs.  
🐛 **Issues?** Check [Open WebUI GitHub](https://github.com/open-webui/open-webui).
