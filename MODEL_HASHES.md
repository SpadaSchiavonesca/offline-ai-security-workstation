# MODEL_HASHES.md — Model Integrity Record

This repository uses a locally downloaded GGUF model file from the LM Studio Community listing on Hugging Face.  
Recording file hashes supports reproducibility and reduces supply-chain risk by letting reviewers confirm they have the **exact same artifact**.

Model source (Hugging Face):
- https://huggingface.co/lmstudio-community/Qwen3-14B-GGUF

---

## Artifact used in this repo (recorded)

| Artifact filename | Quant | File size (bytes) | SHA-256 | Date recorded |
| :--- | :--- | ---: | :--- | :---: |
| `Qwen3-14B-Q4_K_M.gguf` | `Q4_K_M` | 9001753376 | `712C0791D5124D3DD6D1E4968DE1201207AFEAE49C6E10FBEB9C58FE00C58555` | 2026-02-05 |

---

## How to verify (Windows PowerShell)

PowerShell command to compute the SHA-256 hash for the model file:

```powershell
Get-FileHash -Algorithm SHA256 "C:\Users\dada_\.lmstudio\models\lmstudio-community\Qwen3-14B-GGUF\Qwen3-14B-Q4_K_M.gguf"
