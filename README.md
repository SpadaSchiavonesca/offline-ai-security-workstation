# 🔒 Offline AI Security Workstation

[![LM Studio](https://img.shields.io/badge/LM%20Studio-0.4.1-blue)](https://lmstudio.ai)
[![Qwen3](https://img.shields.io/badge/Model-Qwen3--14B-green)](https://huggingface.co/Qwen/Qwen3-14B)
[![Vulkan](https://img.shields.io/badge/Backend-Vulkan-red)](https://www.vulkan.org/)

> Local LLM setup for GRC and Security Analyst work: threat modeling, compliance documentation, policy drafting, and incident response planning—without sending sensitive content to third-party APIs.

---

## 🎯 Project Goal

**Requirement:** Run a reasoning-capable LLM locally to support GRC/security workflows while minimizing data-exposure risk.

**Target roles:** GRC Analyst, Cybersecurity Analyst

**Primary deliverables:** Threat models, risk statements, control mappings, policy drafts, incident response checklists, vendor security questionnaires.

---

## 🖥️ System Specifications

### Hardware
- **CPU:** AMD Ryzen 7 3700X 8-Core
- **RAM:** 32GB DDR4
- **GPU:** AMD Radeon RX 6700 XT (12GB VRAM)
- **OS:** Windows 11
- **Storage:** ~10GB for model weights

### Software Stack
- **Platform:** LM Studio 0.4.1
- **Backend:** Vulkan (AMD-optimized)
- **Model:** Qwen3 14B Instruct (Q4_K_M quantization)
- **Context Window:** 32,768 tokens (expandable to 131k with YaRN)
- **Performance:** 24-26 tokens/second (GPU-accelerated)

---

## 🧠 Technology Choices (Decision Documentation)

### Why I chose LM Studio (vs Jan)

I evaluated Jan and LM Studio as local LLM desktop clients and chose **LM Studio** for this portfolio because its offline and on-device data handling is clearly documented and easy to validate.

**LM Studio documentation states:**
- LM Studio can operate entirely offline once model files are downloaded
- Core functions like using downloaded LLMs, chatting with documents (RAG), and running a local server do not require internet connectivity
- "Nothing you enter into LM Studio when chatting with LLMs leaves your device"
- Documents used for chat/RAG stay on your machine and are processed locally

**Source:** [LM Studio Offline Operation](https://lmstudio.ai/docs/app/offline)

**GRC rationale:** For threat modeling, policy drafts, control narratives, and incident-response notes, the safest default is a local workflow with minimal data egress. LM Studio's offline guidance is explicit and easy to validate by disconnecting the network after model download.

---

### Why I chose the LM Studio Community Qwen3 GGUF

I used the **LM Studio Community** listing for Qwen3 14B because the model details clearly identify:
- Original model (Qwen3-14B by Qwen team)
- GGUF conversion/quantization provenance (bartowski)
- Runtime compatibility (llama.cpp release)

**Rationale:** For portfolio work, provenance matters. Traceable origin supports reproducibility for reviewers who want to replicate the setup.

---

### Why I chose Qwen3 14B (vs other local models)

**My constraints:**
- Hardware: AMD RX 6700 XT (12GB VRAM)
- Work type: Long-context security writing (policies, standards, IR notes) + structured outputs (tables, checklists, mappings)
- Goal: Strong instruction-following with optional deeper reasoning

**Why Qwen3 14B:**
1. **Context handling:** Supports 32k default context (up to 131k with YaRN), critical for analyzing long compliance frameworks or threat models
2. **Dual-mode operation:** Can switch between deep reasoning (default) and fast responses (`/no_think` flag)
3. **Fits my hardware:** At Q4_K_M quantization, uses ~9.5GB VRAM, leaving room for context processing

**Source:** [Qwen3-14B Model Card](https://huggingface.co/Qwen/Qwen3-14B)

**Alternatives considered:**
- Smaller models (faster, lighter) — but I prioritized long-context + reasoning for GRC writing
- Coding-specialized models — but my outputs are security narratives (controls, risks, policies) rather than pure code

---

### Why Q4_K_M quantization

**Q4_K_M** provides a practical quality/size tradeoff for consumer GPUs:
- Fits the full model in VRAM (reduces CPU/RAM spillover and latency)
- Preserves enough quality for writing, summarization, and multi-step reasoning
- Keeps throughput high (24-26 tok/sec) for iterative drafting

---

## 📋 Evidence (Proof of Execution)

See `/screenshots/` folder for verification:
- `hardware-vulkan.png` — GPU detected with Vulkan backend
- `reasoning-mode-output.png` — Threat model generation with tok/sec metrics
- `fast-mode-output.png` — `/no_think` mode demonstration

**Measured results:**
- **Reasoning mode:** 24.95 tok/sec on threat modeling prompt (16.56s thinking time)
- **Fast mode:** 26.22 tok/sec with `/no_think` flag (<1s response time)
- **VRAM usage:** 9.5GB / 12GB during inference

---

## 🚀 Setup Guide (Step-by-Step)

### Prerequisites
- Windows 10/11 (or Linux)
- AMD GPU with 8GB+ VRAM (or NVIDIA with CUDA support)
- 20GB free disk space

### Installation Steps

**1. Install LM Studio**
- Download from: [lmstudio.ai](https://lmstudio.ai)
- For AMD cards, standard version works (uses Vulkan automatically)

**2. Enable GPU Acceleration**

