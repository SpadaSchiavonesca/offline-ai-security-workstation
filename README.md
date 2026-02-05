# 🔒 Offline AI Security Workstation (Local LLM for GRC/Security)

[![LM Studio](https://img.shields.io/badge/LM%20Studio-0.4.1-blue)](https://lmstudio.ai/docs/app/offline)
[![Model (GGUF)](https://img.shields.io/badge/Model-Qwen3--14B%20GGUF-green?logo=huggingface)](https://huggingface.co/lmstudio-community/Qwen3-14B-GGUF)
[![Quant](https://img.shields.io/badge/Quant-Q4__K__M-green)](https://huggingface.co/lmstudio-community/Qwen3-14B-GGUF/tree/main)
[![Backend](https://img.shields.io/badge/Backend-Vulkan-red?logo=vulkan)](https://www.vulkan.org/)

> Offline-first local LLM setup for GRC and Security Analyst work (threat modeling, compliance documentation, policy drafting, and IR planning) designed to reduce third‑party data exposure.

---

## Contents
- [Project goal](#-project-goal)
- [System specifications](#️-system-specifications)
- [Threat model (quick)](#-threat-model-quick)
- [Offline assurance](#-offline-assurance)
- [Model provenance (supply chain)](#-model-provenance-supply-chain)
- [Evidence (proof of execution)](#-evidence-proof-of-execution)
- [Setup guide (step-by-step)](#️-setup-guide-step-by-step)
- [Security hardening (OWASP LLM Top 10 2025)](#️-security-hardening-owasp-llm-top-10-2025)
- [GRC & Security use cases](#-grc--security-use-cases)
- [Known limitations](#️-known-limitations)
- [License & professional disclaimer](#️-license--professional-disclaimer)
- [Related files](#-related-files)

---

## 🎯 Project goal

**Requirement:** Run a reasoning-capable LLM locally to support GRC/security workflows while minimizing data-exposure risk.

**Target roles:** GRC Analyst, Cybersecurity Analyst, Compliance Analyst

**Primary deliverables:**
- Threat models
- Risk statements / risk register entries
- Control mappings
- Policy drafts
- Incident response checklists
- Vendor security questionnaires

---

## 🖥️ System specifications

### Hardware
- **CPU:** AMD Ryzen 7 3700X (8-Core)
- **RAM:** 32GB DDR4
- **GPU:** AMD Radeon RX 6700 XT (12GB VRAM)
- **OS:** Windows 11
- **Storage:** Plan ~20GB free (model file + app cache/runtimes + screenshots)

### Software stack (this build)
- **Client:** LM Studio 0.4.1
- **Backend:** Vulkan
- **Model (GGUF):** `lmstudio-community/Qwen3-14B-GGUF`[^hf-listing]
- **Quantization used (this workstation):** `Q4_K_M`
  - **File:** `Qwen3-14B-Q4_K_M.gguf`
  - **File size:** 9,001,753,376 bytes
  - **SHA-256:** `712C0791D5124D3DD6D1E4968DE1201207AFEAE49C6E10FBEB9C58FE00C58555`

> [!TIP]
> Store integrity data in-repo (see `MODEL_HASHES.md`) so reviewers can verify you’re running the exact artifact you claim.

### Hash verification (Windows)
```powershell
Get-FileHash .\Qwen3-14B-Q4_K_M.gguf -Algorithm SHA256
