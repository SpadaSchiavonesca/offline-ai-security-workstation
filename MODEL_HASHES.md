# MODEL_HASHES.md — Model Integrity Record

This repository records SHA-256 hashes for the exact model artifact used, supporting reproducibility and basic supply-chain integrity verification.

Model source (Hugging Face):
- https://huggingface.co/lmstudio-community/Qwen3-14B-GGUF

---

## Recorded artifact (this workstation)

| Artifact filename | Quant | File size (bytes) | SHA-256 | Date recorded |
| :--- | :--- | ---: | :--- | :---: |
| `Qwen3-14B-Q4_K_M.gguf` | `Q4_K_M` | 9001753376 | `712C0791D5124D3DD6D1E4968DE1201207AFEAE49C6E10FBEB9C58FE00C58555` | 2026-02-04 |

---

## How to verify (Windows PowerShell)

### Option A — Use your LM Studio models folder (recommended)
```powershell
Get-FileHash -Algorithm SHA256 "$env:USERPROFILE\.lmstudio\models\lmstudio-community\Qwen3-14B-GGUF\Qwen3-14B-Q4_K_M.gguf"

