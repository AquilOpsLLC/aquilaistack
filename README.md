# 🚀 AquilAI Stack

**All-in-One AI Development Platform** - Production-ready AI infrastructure with Docker Compose

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-Required-blue.svg)](https://www.docker.com/)
[![NVIDIA GPU](https://img.shields.io/badge/NVIDIA-GPU%20Ready-green.svg)](https://developer.nvidia.com/cuda-downloads)

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Service Overview](#-service-overview)
- [Model Setup](#-model-setup)
- [Configuration](#-configuration)
- [Access URLs](#-access-urls)
- [Usage Examples](#-usage-examples)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Overview

AquilAI Stack is a comprehensive, self-hosted AI development platform that provides everything you need to build, test, and deploy AI applications. From LLM inference to image generation, from vector databases to workflow orchestration - all in one Docker Compose setup.

**Perfect for:**
- 🤖 Building RAG (Retrieval-Augmented Generation) systems
- 🎨 AI-powered content generation pipelines
- 🔊 Voice-enabled AI applications
- 🧪 Local LLM experimentation and fine-tuning
- 🚀 Production-ready AI agent development

---

## ✨ Features

### 🔒 Privacy First
- **100% Local** - All data stays on your infrastructure
- **No Cloud Dependencies** - Works completely offline
- **No Vendor Lock-in** - Open source, self-hosted

### ⚡ Production Ready
- **Auto Health Checks** - Built-in service monitoring
- **Volume Persistence** - Data survives container restarts
- **GPU Acceleration** - NVIDIA GPU support for AI workloads
- **Queue Management** - Redis-backed job processing

### 🔧 Developer Friendly
- **Zero Configuration** - Works out of the box
- **Modular Design** - Use only what you need
- **Two Deployment Options** - Minimal and full-stack versions
- **Hot Reload Support** - Development-friendly setup

---

## 📋 Prerequisites

### Required
- **Docker** (v20.10+) and **Docker Compose** (v2.0+)
- **8GB RAM minimum** (16GB+ recommended for full stack)
- **50GB free disk space** (more needed for AI models)

### Optional but Recommended
- **NVIDIA GPU** with CUDA support (for GPU acceleration)
- **NVIDIA Container Toolkit** ([Installation Guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html))

### System Requirements
```bash
# Check Docker version
docker --version

# Check Docker Compose version
docker-compose --version

# Check NVIDIA GPU (if available)
nvidia-smi
```

---

## 🚀 Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/AquilOpsLLC/aquilaistack.git
cd aquilaistack
```

### 2. Create Environment File
```bash
cp .env.example .env
```

### 3. Choose Your Setup

#### Option A: Minimal Setup (Core AI Services Only)
```bash
docker-compose up -d
```

#### Option B: Full Stack (All Services + Monitoring + DevOps)
```bash
docker-compose -f docker-compose-fullstack.yml up -d
```

### 4. Verify Services
```bash
# Check all containers are running
docker-compose ps

# View logs
docker-compose logs -f

# Check specific service
docker-compose logs -f n8n
```

### 5. Access the Platform
Open your browser and navigate to:
- **n8n Workflow**: http://localhost:5678 (admin/admin123)
- **ComfyUI**: http://localhost:8188
- **MinIO Console**: http://localhost:9001 (minioadmin/minioadmin123)

---

## 🧩 Service Overview

### AI & ML Services

| Service | Port | Description | GPU |
|---------|------|-------------|-----|
| **Ollama** | 11434 | Local LLM inference engine | ✅ |
| **ComfyUI** | 8188 | Stable Diffusion & image generation | ✅ |
| **Whisper** | 9000 | Speech-to-text transcription | ✅ |
| **Piper** | 5500 | Text-to-speech synthesis | ❌ |
| **Qdrant** | 6333, 6334 | Vector database for embeddings | ❌ |

### Orchestration & Backend

| Service | Port | Description |
|---------|------|-------------|
| **n8n** | 5678 | Visual workflow automation |
| **PostgreSQL** | 5432 | Primary database |
| **Redis** | 6379 | Cache & queue management |
| **MinIO** | 9001, 9002 | S3-compatible object storage |

### Monitoring & DevOps (Full Stack Only)

| Service | Port | Description |
|---------|------|-------------|
| **Portainer** | 9443 | Docker container management |
| **Dozzle** | 8080 | Real-time log viewer |
| **Grafana** | 3000 | Metrics visualization |
| **Uptime Kuma** | 3001 | Service monitoring |
| **Gitea** | 3002 | Self-hosted Git server |
| **Code Server** | 8443 | Browser-based VS Code |
| **SonarQube** | 9003 | Code quality analysis |

---

## 📦 Model Setup

### Ollama Models

After starting the stack, pull the models you need:

```bash
# Pull a general-purpose LLM
docker exec -it ollama ollama pull llama3.2

# Pull an embedding model (for RAG)
docker exec -it ollama ollama pull nomic-embed-text

# Pull other available models
docker exec -it ollama ollama pull mistral
docker exec -it ollama ollama pull codellama
docker exec -it ollama ollama pull llama3.2-vision

# List installed models
docker exec -it ollama ollama list
```

**Available Models**: Check [Ollama Model Library](https://ollama.com/library)

### ComfyUI Models

ComfyUI requires Stable Diffusion models to function. Download and place them in the appropriate directories:

#### Directory Structure
```
aquilaistack/
├── comfyui_models/
│   ├── checkpoints/          # Main SD models go here
│   ├── vae/                  # VAE models
│   ├── loras/                # LoRA models
│   ├── controlnet/           # ControlNet models
│   ├── embeddings/           # Textual inversions
│   ├── upscale_models/       # Upscaler models
│   └── ... (other model types)
```

#### Required Models

**1. Stable Diffusion Checkpoint** (Required)
- Download from: [Hugging Face](https://huggingface.co/models?pipeline_tag=text-to-image&sort=downloads) or [Civitai](https://civitai.com/)
- Recommended models:
  - **SD 1.5**: `v1-5-pruned-emaonly.safetensors` (~4GB)
  - **SDXL**: `sd_xl_base_1.0.safetensors` (~6.9GB)
- Place in: `./comfyui_models/checkpoints/`

```bash
# Example: Download SD 1.5
cd comfyui_models/checkpoints/
wget https://huggingface.co/runwayml/stable-diffusion-v1-5/resolve/main/v1-5-pruned-emaonly.safetensors
```

**2. VAE Model** (Recommended)
- Download: `vae-ft-mse-840000-ema-pruned.safetensors`
- Place in: `./comfyui_models/vae/`

```bash
cd comfyui_models/vae/
wget https://huggingface.co/stabilityai/sd-vae-ft-mse-original/resolve/main/vae-ft-mse-840000-ema-pruned.safetensors
```

**3. Optional Models** (Enhance capabilities)

- **ControlNet** (for guided generation):
  ```bash
  cd comfyui_models/controlnet/
  # Download from: https://huggingface.co/lllyasviel/ControlNet-v1-1
  ```

- **LoRA** (for style customization):
  ```bash
  cd comfyui_models/loras/
  # Download from: https://civitai.com/ (LoRA section)
  ```

- **Upscaler** (for image enhancement):
  ```bash
  cd comfyui_models/upscale_models/
  # Recommended: RealESRGAN or ESRGAN models
  ```

#### Verify Models

After placing models, restart ComfyUI:
```bash
docker-compose restart comfyui
```

Visit http://localhost:8188 and verify models appear in the checkpoint selector.

#### Model Sources

- **Hugging Face**: https://huggingface.co/models
- **Civitai**: https://civitai.com/
- **ComfyUI Model Database**: https://comfyanonymous.github.io/ComfyUI_examples/

### Whisper Models

Whisper models are downloaded automatically on first use. To pre-download:

```bash
# Models are cached in whisper_models volume
# Available sizes: tiny, base, small, medium, large
# Default: base (configured in docker-compose)
```

---

## ⚙️ Configuration

### Environment Variables

Edit `.env` file to customize:

```bash
# n8n Configuration
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=changeme

# PostgreSQL
POSTGRES_PASSWORD=strongpassword

# MinIO
MINIO_ROOT_USER=admin
MINIO_ROOT_PASSWORD=changeme

# Timezone
TZ=Europe/Istanbul
```

### GPU Configuration

If you **don't have an NVIDIA GPU**, remove GPU-related sections from docker-compose:

```yaml
# Remove these sections from ollama, comfyui, whisper services:
deploy:
  resources:
    reservations:
      devices:
        - driver: nvidia
          count: all
          capabilities: [gpu]
```

### Resource Limits

Adjust container resources in docker-compose.yml:

```yaml
deploy:
  resources:
    limits:
      cpus: '4'
      memory: 8G
```

---

## 🌐 Access URLs

### Default Credentials

| Service | URL | Username | Password |
|---------|-----|----------|----------|
| n8n | http://localhost:5678 | admin | admin123 |
| MinIO Console | http://localhost:9001 | minioadmin | minioadmin123 |
| Portainer | https://localhost:9443 | - | (set on first login) |
| Grafana | http://localhost:3000 | admin | admin123 |
| Code Server | http://localhost:8443 | - | changeme123 |
| SonarQube | http://localhost:9003 | admin | admin (change on first login) |

### Service Endpoints

```
AI Services:
├── Ollama API:        http://localhost:11434
├── ComfyUI Web:       http://localhost:8188
├── Whisper (Wyoming): http://localhost:9000
├── Piper (Wyoming):   http://localhost:5500
└── Qdrant API:        http://localhost:6333

Backend:
├── PostgreSQL:        localhost:5432
├── Redis:             localhost:6379
└── MinIO S3:          http://localhost:9002

Monitoring:
├── Dozzle Logs:       http://localhost:8080
├── Uptime Kuma:       http://localhost:3001
└── Webhook Test:      http://localhost:8084
```

---

## 💡 Usage Examples

### 1. Create a Simple n8n Workflow with Ollama

1. Open n8n: http://localhost:5678
2. Create new workflow
3. Add nodes:
   - **Trigger**: Webhook or Manual Trigger
   - **Ollama Chat**: Configure with `http://ollama:11434`
   - **Response**: Return result

### 2. RAG System with Qdrant

```javascript
// n8n JavaScript code node example
const response = await $http.request({
  method: 'POST',
  url: 'http://qdrant:6333/collections/my_collection/points/search',
  body: {
    vector: embedding,
    limit: 5
  }
});
```

### 3. Generate Images with ComfyUI

1. Open ComfyUI: http://localhost:8188
2. Load default workflow
3. Enter your prompt
4. Queue prompt
5. Generated images saved to `./comfyui_output/`

### 4. Speech-to-Text with Whisper

```bash
# Send audio file to Whisper
curl -X POST http://localhost:9000/transcribe \
  -H "Content-Type: audio/wav" \
  --data-binary @audio.wav
```

---

## 🔧 Troubleshooting

### Common Issues

#### 1. Services Won't Start
```bash
# Check logs
docker-compose logs [service-name]

# Restart specific service
docker-compose restart [service-name]

# Rebuild containers
docker-compose down
docker-compose up -d --build
```

#### 2. Out of Memory
```bash
# Check resource usage
docker stats

# Increase Docker memory limit in Docker Desktop settings
# Or reduce services by using minimal compose file
```

#### 3. GPU Not Detected
```bash
# Verify NVIDIA runtime
docker run --rm --gpus all nvidia/cuda:11.8.0-base-ubuntu22.04 nvidia-smi

# Install NVIDIA Container Toolkit
# https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html
```

#### 4. Permission Denied Errors
```bash
# Fix volume permissions
sudo chown -R $USER:$USER .

# Or run with proper user
docker-compose down
USER_ID=$(id -u) GROUP_ID=$(id -g) docker-compose up -d
```

#### 5. Ollama Models Not Loading
```bash
# Verify model installation
docker exec -it ollama ollama list

# Re-pull model
docker exec -it ollama ollama pull llama3.2

# Check Ollama logs
docker-compose logs ollama
```

#### 6. ComfyUI Shows No Models
```bash
# Verify model files exist
ls -lh comfyui_models/checkpoints/

# Check file permissions
sudo chmod -R 755 comfyui_models/

# Restart ComfyUI
docker-compose restart comfyui
```

### Clean Restart

```bash
# Stop all services
docker-compose down

# Remove volumes (WARNING: deletes all data)
docker-compose down -v

# Clean start
docker-compose up -d
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Development

```bash
# Create a new branch
git checkout -b feature/your-feature

# Make changes and test
docker-compose up -d

# Submit PR with description
```

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

Built with amazing open-source projects:
- [n8n](https://n8n.io/) - Workflow Automation
- [Ollama](https://ollama.ai/) - LLM Runtime
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI) - Stable Diffusion GUI
- [Qdrant](https://qdrant.tech/) - Vector Database
- [Whisper](https://github.com/openai/whisper) - Speech Recognition
- [MinIO](https://min.io/) - Object Storage

---

## 📧 Support

- **Issues**: [GitHub Issues](https://github.com/AquilOpsLLC/aquilaistack/issues)
- **Discussions**: [GitHub Discussions](https://github.com/AquilOpsLLC/aquilaistack/discussions)

---

## ⭐ Star History

If you find this project useful, please consider giving it a star! ⭐

---

**Built with ❤️ by AquilOps**

*Self-hosted AI for everyone*
