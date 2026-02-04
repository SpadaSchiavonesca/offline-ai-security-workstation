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

I evaluated Jan and LM Studio as the two leading local LLM desktop clients. Both are strong choices for offline AI work, but I chose **LM Studio** for this specific project based on my hardware (AMD GPU) and portfolio goals.

#### Comparison

| Feature | Jan | LM Studio |
|:--------|:----|:----------|
| **License** | ✅ Open source (AGPLv3) | ❌ Proprietary |
| **Privacy approach** | ✅ Explicit: "zero data collection" | ✅ States nothing leaves device |
| **Offline documentation** | ⚠️ Privacy docs focus on cloud vs local distinction | ✅ Dedicated offline operation guide |
| **AMD GPU (Vulkan) support** | ⚠️ Supported but less documented | ✅ Well-documented, tested |
| **Ease of setup (AMD)** | ⚠️ Requires some configuration | ✅ Auto-detects Vulkan, simpler |
| **Model discovery** | Good | ✅ Excellent (integrated search/download) |
| **Community** | Growing | ✅ Larger, more AMD/Vulkan examples |

**Sources:**
- Jan Privacy Approach: https://www.jan.ai/docs/desktop/privacy
- Jan GitHub (AGPLv3): https://github.com/janhq/jan
- LM Studio Offline Docs: https://lmstudio.ai/docs/app/offline

#### Why I chose LM Studio for this project

**Decision factors:**
1. **AMD/Vulkan documentation:** LM Studio has clearer, more extensive documentation for AMD GPU setup via Vulkan, with many community examples for RX 6700 XT specifically
2. **Offline operation clarity:** LM Studio has a dedicated "Offline Operation" page that explicitly states what works offline and what doesn't, making it easy to validate for this portfolio
3. **Portfolio presentation:** The polished UI and clear hardware detection screens make better screenshots for employer review
4. **Setup speed:** For this demonstration, LM Studio's auto-detection of Vulkan and simpler configuration saved setup time

#### Why you might prefer Jan instead

**Jan's advantages:**
- ✅ **Open source:** Full code transparency (AGPLv3 license) — important if you need to audit the client code
- ✅ **Privacy-first design:** Explicit privacy documentation and "zero data collection" stance
- ✅ **Flexibility:** More control over configuration and data folder management
- ✅ **Philosophy:** If open source is a requirement (not just nice-to-have), Jan is the clear choice

**For my GRC portfolio goal** (demonstrating secure, offline AI workflows on AMD hardware), LM Studio's documentation clarity and AMD/Vulkan maturity outweighed Jan's open-source advantage. For a project specifically showcasing open-source tool selection, Jan would be the better choice.


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

### Hardware Detection (Vulkan Backend)
![AMD RX 6700 XT detected with Vulkan](screenshots/hardware-vulkan.png)

**What this shows:** LM Studio successfully detected my AMD Radeon RX 6700 XT with 12GB VRAM using the Vulkan backend. GPU is enabled and ready for inference.

---

### Reasoning Mode Output (Threat Modeling)
![Threat modeling with thinking mode](screenshots/reasoning-mode-output.png)

**What this shows:**
- **Prompt:** "Write a one-sentence threat model for a web API that handles user authentication"
- **Thinking time:** 16.56 seconds (model reasoning before output)
- **Generation speed:** 24.95 tokens/second
- **Output:** Comprehensive threat analysis identifying SQLi, brute-force, and token exposure risks

---

### Fast Mode Output (`/no_think`)
![Fast mode with no thinking](screenshots/fast-mode-output.png)

**What this shows:**
- **Prompt:** "Write a one-sentence summary of NIST 800-53 /no_think"
- **Total time:** 0.87 seconds (instant response)
- **Generation speed:** 26.22 tokens/second
- **Output:** Accurate summary with no pre-thinking delay

---

**Measured results summary:**
- **VRAM usage:** 9.5GB / 12GB during inference
- **Reasoning mode:** 24.95 tok/sec (with 10-20s thinking)
- **Fast mode:** 26.22 tok/sec (instant)

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

