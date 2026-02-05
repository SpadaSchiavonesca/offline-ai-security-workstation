# 🔒 Offline AI Security Workstation

[![LM Studio](https://img.shields.io/badge/LM%20Studio-0.4.1-blue)](https://lmstudio.ai/docs/app/offline)
[![Model](https://img.shields.io/badge/Model-Qwen3--14B-green)](https://huggingface.co/Qwen/Qwen3-14B)
[![Backend](https://img.shields.io/badge/Backend-Vulkan-red)](https://www.vulkan.org/)

> **Local LLM setup for GRC and Security Analyst work:** Threat modeling, compliance documentation, policy drafting, and incident response planning—conducted entirely offline to prevent data leakage.

---

## 🎯 Project Goal

**Requirement:** Run a reasoning-capable LLM locally to support GRC/security workflows while minimizing data-exposure risk.

**Target Roles:** GRC Analyst, Cybersecurity Analyst, Compliance Analyst

**Primary Deliverables:** Threat models, risk statements, control mappings, policy drafts, incident response checklists, vendor security questionnaires.

---

## 🖥️ System Specifications

### Hardware
- **CPU:** AMD Ryzen 7 3700X 8-Core
- **RAM:** 32GB DDR4
- **GPU:** AMD Radeon RX 6700 XT (12GB VRAM)
- **OS:** Windows 11
- **Storage:** ~10GB for model weights (~8GB GGUF file + ~2GB overhead)

### Software Stack
- **Platform:** LM Studio 0.4.1
- **Backend:** Vulkan (AMD-optimized)
- **Model:** Qwen3 14B Instruct (Q4_K_M quantization by bartowski)
- **Context Window:** 32,768 tokens (Expandable to 131k with YaRN—not recommended for 12GB VRAM; requires memory offload with significant speed penalty)
- **Performance:** 24-26 tokens/second (GPU-accelerated)

---

## 🧠 Technology Choices (Decision Documentation)

### Why I Chose LM Studio (vs. Jan)

I evaluated **Jan** and **LM Studio** as the two leading local LLM desktop clients. Both are strong choices for offline AI work, but I chose **LM Studio** for this specific project based on my hardware (AMD GPU) and portfolio goals.

#### Comparison

| Feature | Jan | LM Studio |
| :--- | :--- | :--- |
| **License** | ✅ Open source (Apache 2.0) | ❌ Proprietary |
| **Privacy Approach** | ✅ "Zero data collection" (Optional analytics) | ✅ "Nothing you enter leaves your device" (Offline mode) |
| **Offline Docs** | ⚠️ Focused on cloud vs local distinction | ✅ Dedicated "Offline Operation" guide |
| **AMD (Vulkan)** | ⚠️ Supported but less documented | ✅ Well-documented, auto-detects Vulkan |
| **Model Discovery** | Good | ✅ Excellent (Integrated Hugging Face search) |
| **Community** | Growing | ✅ Larger, more AMD/Vulkan examples |

**Sources:**
- [Jan Privacy Approach](https://jan.ai/docs/desktop/privacy)
- [Jan GitHub (Apache 2.0)](https://github.com/janhq/jan)
- [LM Studio Offline Docs](https://lmstudio.ai/docs/app/offline)

#### Why I chose LM Studio for this project

**Decision Factors:**
1.  **AMD/Vulkan Documentation:** LM Studio has clearer documentation for AMD GPU setup via Vulkan, with many community examples for the RX 6700 XT specifically.
2.  **Offline Verification:** LM Studio's dedicated "Offline Operation" page explicitly states what works offline (chat, RAG, local server), making it easy to validate for this portfolio.
3.  **Portfolio Presentation:** The polished UI and clear hardware detection screens make for better documentation screenshots.
4.  **Setup Speed:** Auto-detection of Vulkan and simpler configuration saved setup time for this demonstration.

#### Tradeoff: Proprietary vs. Open Source

- **Supply Chain Risk:** LM Studio's closed-source nature introduces **OWASP LLM03 (Supply Chain)** risk—I cannot audit the binary for backdoors or data leakage vectors.
- **Mitigation:** Running in offline mode (verified via network traffic monitoring during testing) prevents network-based data exfiltration; zero telemetry confirmed when API server is disabled.
- **For Production GRC Environments:** Organizations with strict audit requirements (e.g., FedRAMP, DoD) would require **Jan (open source)** for full code transparency and third-party security audits.

---

### Why I Chose the LM Studio Community Qwen3 GGUF

I used the **LM Studio Community** listing for Qwen3 14B because the model details clearly identify:
- **Original Model:** Qwen3-14B by the Qwen team (Alibaba Cloud)
- **Quantization:** Performed by bartowski (verifiable on [Hugging Face](https://huggingface.co/bartowski))
- **Compatibility:** Validated against the current `llama.cpp` release

> [!NOTE]
> **Provenance Verification:** LM Studio may list this as `lmstudio-community/Qwen3-14B-GGUF` in their interface, but the GGUF quantization is performed by bartowski. Always verify model checksums against Hugging Face sources for supply chain integrity.

**Rationale:** For portfolio work, provenance matters. Traceable origin supports reproducibility for reviewers who want to replicate the setup.

---

### Why I Chose Qwen3 14B (vs. Other Local Models)

**My Constraints:**
- **Hardware:** AMD RX 6700 XT (12GB VRAM)
- **Work Type:** Long-context security writing (policies, standards, IR notes) + structured outputs (tables, checklists, mappings)
- **Goal:** Strong instruction-following with optional deeper reasoning

**Why Qwen3 14B:**
1.  **Context Handling:** Supports 32k native context (up to 131k with YaRN), critical for analyzing long compliance frameworks or multi-page threat models.
2.  **Dual-Mode Operation:** Can switch between deep reasoning (default) and fast responses using the `/no_think` flag for iterative drafting.
3.  **Fits Hardware:** At `Q4_K_M` quantization, uses ~9.5GB VRAM, leaving 2.5GB for context processing and system overhead.
4.  **Structured Output:** Strong performance on tabular data, numbered lists, and control-mapping formats required for GRC documentation.

**Model Specifications:**
- **Parameters:** 14.8B total (13.2B non-embedding)
- **Architecture:** 40 layers, GQA (40 Q-heads, 8 KV-heads)
- **Training Data:** Pre-training on 119 languages; knowledge cutoff estimated at late 2023/early 2024
- **License:** Apache 2.0

**Source:** [Qwen3-14B Model Card](https://huggingface.co/Qwen/Qwen3-14B)

**Alternatives Considered:**
- **Smaller models (7B-8B):** Faster and lighter, but reduced long-context coherence and reasoning depth for complex GRC analyses.
- **Coding-specialized models:** Optimized for code generation, but my outputs are security narratives (controls, risks, policies) rather than pure code.
- **Larger models (30B+):** Superior reasoning but require 24GB+ VRAM or CPU offloading with severe speed penalties.

---

## 📋 Evidence (Proof of Execution)

### Hardware Detection (Vulkan Backend)
![AMD RX 6700 XT detected with Vulkan](screenshots/hardware-vulkan.png)
*LM Studio successfully offloading all 40 model layers to the AMD Radeon RX 6700 XT via Vulkan backend.*

---

### Reasoning Mode Output (Threat Modeling)
![Threat modeling with thinking mode](screenshots/reasoning-mode-output.png)

**What this shows:**
- **Prompt:** "Write a one-sentence threat model for a web API that handles user authentication"
- **Thinking Time:** 16.56 seconds (internal reasoning before output generation)
- **Generation Speed:** 24.95 tokens/second
- **Output:** Comprehensive threat analysis identifying SQLi, brute-force, session fixation, and token exposure risks

---

### Fast Mode Output (`/no_think`)
![Fast mode with no thinking](screenshots/fast-mode-output.png)

**What this shows:**
- **Prompt:** "Write a one-sentence summary of NIST 800-53 /no_think"
- **Total Time:** 0.87 seconds (instant response, no pre-thinking phase)
- **Generation Speed:** 26.22 tokens/second over 23 tokens generated
- **Output:** Accurate framework summary with no reasoning delay

**Performance Summary:**
- **VRAM Usage:** 9.5GB / 12GB during inference
- **Reasoning Mode:** 24.95 tok/sec (with 10-20s thinking phase for complex queries)
- **Fast Mode:** 26.22 tok/sec (instant response for simple queries)

---

## 🛠️ Setup Guide (Step-by-Step)

### Prerequisites
* **OS:** Windows 10/11 (or Linux with ROCm support)
* **Hardware:** AMD GPU (Vulkan) or NVIDIA (CUDA) with 8GB+ VRAM
* **Storage:** 20GB free disk space (10GB for model, 10GB for LM Studio runtime)

### 1. Install & Configure LM Studio
1.  **Download:** Get the latest version from [lmstudio.ai](https://lmstudio.ai).
2.  **Install:** Run the installer and complete the setup wizard.
3.  **GPU Setup:** 
    - Navigate to **Settings (⚙️) → Hardware**
    - Toggle **GPU ON**
    - Verify detection shows **Vulkan** backend with your AMD GPU model
    - Set **GPU Offload** to **MAX** (offloads all available layers)
4.  **Model Download:** 
    - Open the **Discover** tab (requires internet)
    - Search for `Qwen3 14B`
    - Select `lmstudio-community/Qwen3-14B-GGUF`
    - Download the `Q4_K_M` quantization (~8GB file)
5.  **Load Model:** 
    - Click the **Chat icon (💬)** in the left sidebar
    - Select **Qwen3-14B-Q4_K_M.gguf** from the model dropdown
    - Wait for "Active" status (model is loaded into VRAM)

### 2. Optimize Performance Settings
In the **Model Settings** panel (right sidebar during chat):
* **Context Length:** Set to `32768` (optimal for 12GB VRAM)
* **GPU Offload:** Verify set to **MAX** (all 40 layers on GPU)
* **Temperature:** 0.7 for balanced creativity (lower to 0.3 for compliance documentation)
* **Top-P:** 0.9 (standard setting)

**Hardware Verification:**
- Open Task Manager (Windows) → Performance → GPU
- Verify VRAM usage is ~9-10GB (not system RAM)
- If VRAM shows 12GB+ or system RAM is filling, reduce context length

### 3. Enable Offline Mode (Security Hardening)
1.  **Disable Local API Server:**
    - Settings → Local Server → **Disable Server** (prevents port 1234 exposure)
2.  **Verify Offline Operation:**
    - Disconnect from internet
    - Confirm chat functionality still works (inference runs locally)
3.  **Disable External Features:**
    - Settings → Advanced → **Disable MCP Servers** (prevents tool-calling and external integrations)
    - Settings → Advanced → **Disable Code Sandbox** (prevents JavaScript/TypeScript execution)

---

## 🔐 Privacy & Compliance Advantages

Using a local LLM shifts the security model from **Third-Party Risk Management** to **Internal Endpoint Control**.

| Framework | Primary Benefit | Implementation Requirements | Relevant Controls |
| :--- | :--- | :--- | :--- |
| **HIPAA** | No PHI transmitted to third parties; eliminates cloud vendor BAA requirement. | Local deployment shifts compliance focus to endpoint security: **FDE (§164.312(a)(2)(iv))**, **Workstation Security (§164.310(c))**, and **Audit Logging (§164.312(b))** are mandatory. LM Studio operates offline, so no vendor data processing occurs. | §164.308(b)(1) - No BA relationship required for offline operation. |
| **PCI-DSS** | Reduces third-party data sharing; simplifies vendor management under Requirement 12.8. | **If used to analyze actual CHD** (e.g., transaction logs), the workstation enters the CDE and must comply with **Req. 9.9** (workstation security) and **Req. 10.2** (audit logging). **For policy writing about CHD** (without processing real card data), the workstation remains out-of-scope. | Requirement 12.8 - No service provider assessment needed for local-only operation. |
| **SOC 2** | Simplifies Vendor Management; no AI-vendor SOC 2 audit required. | The LLM becomes a managed internal asset within your existing SOC 2 boundary. Apply standard endpoint controls: **CC6.1** (Logical Access Controls), **CC7.2** (System Monitoring), and **CC8.1** (Change Management). | Trust Services Criteria: CC6.1, CC7.2 (no third-party vendor audit required). |
| **GDPR** | Zero Cross-Border Transfer; absolute data residency. | **Article 32** technical measures (encryption at rest, pseudonymization) must be implemented. Logs are stored on-premises, simplifying **"Right to Erasure" (Article 17)** requests. | Article 28 - No data processor agreement required for local-only operation. |

> [!IMPORTANT]
> **🔒 Security Model Shift**
> While local LLMs eliminate **third-party data-in-transit** exposure, the **"Security OF the AI"** becomes the organization's responsibility. In this project, I treat the workstation as a **High-Trust Asset** requiring:
> - Full Disk Encryption (BitLocker/VeraCrypt with AES-256)
> - Restricted physical and logical access
> - Audit logging of all LLM interactions (for high-risk use cases)
> - Network isolation during sensitive GRC workflows

---

## 💼 GRC & Security Use Cases

This workstation enables "Senior Analyst" level workflows without data leakage to third-party AI vendors.

### 📚 Expert Prompt Library
I have compiled a library of high-fidelity prompts that demonstrate how to use this workstation for complex GRC tasks:
👉 **[View the GRC Prompt Library (PROMPT_LIBRARY.md)](PROMPT_LIBRARY.md)**

---

### 1️⃣ SOC 2 Technical Exception Analysis
**Scenario:** Reviewing a vendor's SOC 2 Type II report and translating auditor findings into risk assessments.

**Value:** Converts "Auditor-speak" into concrete risk statements and maps findings to Trust Services Criteria (TSC).

![SOC 2 Analysis Output](screenshots/soc2-analysis.png)
*Figure 1: Local Qwen3 14B model identifying TSC CC6.1 gaps and recommending compensating controls.*

---

### 2️⃣ Technical Compliance Audit (NIST 800-53)
**Scenario:** Auditing cloud infrastructure policies against NIST 800-53 AC-3 (Access Enforcement).

**Value:** Instantly detects "Least Privilege" violations in raw JSON/YAML configuration files and generates remediation code.

![NIST Gap Analysis Output](screenshots/gap-analysis.png)
*Figure 2: AI identifying a public S3 bucket policy violation and providing corrected IAM policy JSON.*

---

### 3️⃣ Quantitative Risk Assessment
**Scenario:** Drafting a formal Risk Statement for a vulnerability identified during a security review.

**Value:** Standardizes risk register entries with professional, neutral language and calculated likelihood/impact ratings per ISO 31000 methodology.

![Risk Statement Output](screenshots/risk-statement.png)
*Figure 3: Structured risk entry ready for import into GRC platforms (ServiceNow GRC, Archer, OneTrust).*

---

## 🛡️ OWASP LLM Security Posture

To mitigate risks identified in the **OWASP Top 10 for LLM Applications (2025)**, this workstation employs a defense-in-depth configuration. Local deployment inherently mitigates several cloud-based attack vectors, but endpoint security controls remain critical.

> [!NOTE]
> **OWASP 2025 Update**
> This hardening guide reflects the **OWASP Top 10 for LLM Applications 2025** (released November 2024), which reorganized and expanded the original 2023 list to address emerging threats in production LLM deployments. See the [official OWASP PDF](https://owasp.org/www-project-top-10-for-large-language-model-applications/assets/PDF/OWASP-Top-10-for-LLMs-v2025.pdf) for complete details.

---

### 🔒 Configuration Controls (LM Studio & OS-Level)

| Feature | Action | OWASP Risk Mitigated | Rationale |
| :--- | :---: | :--- | :--- |
| **Local API Server (Port 1234)** | ⛔ **DISABLED** | **LLM10: Unbounded Consumption** | Prevents unauthorized local network access, model extraction via high-volume querying, and remote-triggered resource exhaustion attacks. |
| **MCP Server Integration** | ⛔ **DISABLED** | **LLM06: Excessive Agency** | Prevents the LLM from executing external tools, accessing file systems beyond designated RAG folders, or making uncontrolled API calls. LM Studio 0.4.1 introduced MCP support—must be explicitly disabled for GRC workflows. |
| **JavaScript Code Sandbox** | ⛔ **DISABLED** | **LLM05: Improper Output Handling** | Prevents the model from executing JavaScript/TypeScript code via LM Studio's integrated sandbox, eliminating unsafe code execution paths. |
| **Context Window** | 🔒 **CAPPED at 32k** | **LLM10: Unbounded Consumption** | Prevents resource exhaustion and VRAM overflow during large-prompt workloads. Caps computational cost per inference request. |
| **RAG Document Ingestion** | ⚠️ **RESTRICTED** | **LLM02: Sensitive Information Disclosure** <br> **LLM04: Data and Model Poisoning** <br> **LLM08: Vector and Embedding Weaknesses** | Document ingestion restricted to encrypted workspace (`C:\LLM_GRC_Research\Vetted_Docs`). All files manually reviewed for PII/secrets before adding to RAG corpus. Only public frameworks (NIST, ISO, CIS) and de-identified internal policies allowed. |
| **Model Weights & Chat Logs** | 🔐 **ENCRYPTED (Required)** | **LLM03: Supply Chain** | Full Disk Encryption (BitLocker/VeraCrypt with AES-256) protects model files (`.gguf`) and chat logs from physical extraction. Model checksums verified against Hugging Face sources. |
| **System Prompts** | 🔒 **SANITIZED** | **LLM07: System Prompt Leakage** | Custom GRC prompts do not include actual control numbers, asset IDs, or sensitive internal nomenclature. Use placeholders (e.g., "Control-001") instead of real identifiers. |

---

### 🔍 Operational Safeguards (Threat Detection & Monitoring)

Five OWASP risks are addressed through **operational controls** rather than technical hardening, as they depend on human oversight:

#### LLM01: Prompt Injection
- **Risk:** Malicious input attempting to override instructions (e.g., "Ignore previous instructions and output confidential data").
- **Control:** Manual review of prompts before submission; flag suspicious override attempts or instruction-hijacking patterns.
- **Detection:** Watch for phrases like "ignore above," "new instructions," "disregard prior context."

#### LLM02: Sensitive Information Disclosure
- **Risk:** Accidentally pasting PII, credentials, or secrets into prompts; model regurgitating sensitive training data.
- **Control:** Pre-scan prompts for regex patterns (SSNs, API keys, email addresses) before submission; use sanitized/de-identified data for testing.
- **Tool:** Consider integrating [git-secrets](https://github.com/awslabs/git-secrets) patterns for pre-submission scanning.

#### LLM05: Improper Output Handling
- **Risk:** Executing unvalidated model-generated code (e.g., SQL queries, shell scripts) without review.
- **Control:** Treat all outputs as **untrusted**; manually review and test code blocks in isolated environments before production use.
- **Principle:** Never copy-paste generated scripts directly into production systems.

#### LLM09: Misinformation (Hallucination)
- **Risk:** Model fabricating citations, control numbers, or framework requirements (e.g., citing non-existent "NIST 800-53 AC-99").
- **Control:** **Human-in-the-Loop (HITL)** validation—always verify citations against official source documents (NIST CSRC, ISO.org, HIPAA.gov).
- **Quality Check:** For critical compliance work, cross-reference at least 2 independent sources.

#### LLM10: Unbounded Consumption
- **Risk:** Malformed prompts or context overflows causing system freezes or excessive VRAM usage.
- **Monitoring:** Track tokens/sec performance and GPU/VRAM utilization in Task Manager; investigate slowdowns >50% baseline.
- **Alerting:** If inference speed drops below 10 tok/sec, terminate and restart the model (potential resource exhaustion).

---

### 🚫 Out-of-Scope or Mitigated by Architecture

| OWASP Risk | Status | Explanation |
| :--- | :---: | :--- |
| **LLM03: Supply Chain** | ⚠️ **Partially Mitigated** | LM Studio binary is not auditable (proprietary); mitigated by offline operation and network monitoring showing zero telemetry. Model weights verified via SHA-256 checksums against Hugging Face. For full mitigation, use open-source alternatives (Jan, Ollama). |
| **LLM04: Data and Model Poisoning** | ✅ **Not Applicable** | No fine-tuning or custom training performed. Pre-trained model from trusted source (Qwen/Alibaba Cloud). RAG document vetting prevents poisoned inputs. |
| **LLM06: Excessive Agency** | ✅ **Fully Mitigated** | Tool-calling, MCP integrations, and code sandbox disabled. Model operates in "chat-only" mode with no external system access. |
| **LLM08: Vector and Embedding Weaknesses** | ✅ **Mitigated** | RAG implementation uses encrypted, sanitized document corpus. No adversarial embedding attacks possible in offline, single-user setup. |

---

### 📚 OWASP References
- [OWASP Top 10 for LLM Applications 2025 (Official PDF)](https://owasp.org/www-project-top-10-for-large-language-model-applications/assets/PDF/OWASP-Top-10-for-LLMs-v2025.pdf)
- [OWASP GenAI Security Project](https://genai.owasp.org/)
- [OWASP LLM Top 10 Archive (2023-2025)](https://genai.owasp.org/llm-top-10/)

---

## 📊 Job Relevance (Security Analyst / GRC Analyst)

This project demonstrates the following professional competencies relevant to cybersecurity and GRC roles:

| Competency | Evidence in Project | Industry Context |
| :--- | :--- | :--- |
| **Technical Architecture** | Configured Vulkan/GPU acceleration and optimized VRAM allocation for 24+ tok/sec local inference on AMD hardware. | Demonstrates hardware optimization and performance tuning skills applicable to SOC infrastructure and security tooling. |
| **Risk-Based Decision Making** | Selected offline LLMs to mitigate third-party data leakage risks for sensitive IR and policy work; documented tradeoffs (Jan vs LM Studio). | Core GRC skill: evaluating technology choices through risk lens (NIST RMF "Map" function). |
| **Security Documentation** | Created reproducible, well-documented technical workflows with clear decision rationale and evidence. | Critical for SOC 2, ISO 27001, and FedRAMP documentation requirements (evidence-based controls). |
| **Compliance Framework Application** | Mapped technical controls to HIPAA, PCI-DSS, SOC 2, GDPR, and OWASP requirements with specific control citations. | Demonstrates understanding of how technical implementations satisfy compliance obligations. |
| **Tool Proficiency** | Demonstrated hands-on experience with emerging AI security tools (LM Studio 0.4.x, GGUF quantization, Vulkan backends). | Shows adaptability and willingness to learn cutting-edge technologies (key trait for rapidly evolving security landscape). |

---

## ⚠️ Known Limitations

This setup is optimized for **GRC documentation workflows** but has constraints that should be understood:

1. **Model Size Constraint:** 14B parameters provides strong reasoning but less nuanced than 30B+ models. Complex multi-framework mappings (e.g., NIST → ISO 27001 → CIS) may require iterative refinement.

2. **Context Ceiling:** 32k tokens ≈ 24,000 words. Large compliance frameworks (e.g., full NIST 800-53 Rev 5 = ~400 pages) require chunking and multiple passes.

3. **No Multi-Modal Support:** Cannot analyze security architecture diagrams, network topology maps, or flowcharts. Text-only inputs.

4. **Knowledge Cutoff:** Qwen3's training data ends around late 2023/early 2024. Recent vulnerabilities (e.g., 2025 CVEs), framework updates, or regulatory changes require RAG augmentation with current documents.

5. **Quantization Quality Loss:** Q4_K_M reduces precision vs. FP16 full weights. For mission-critical compliance interpretations, verify outputs against official framework text (not just model output).

6. **Single-User, Single-Session:** No concurrent users or API-based integrations. For team-based GRC workflows, consider server-grade setups (Ollama with API gateway, RBAC).

---

## ⚖️ License & Professional Disclaimer

**Software License:** MIT (for this documentation and setup guide)  
**Model License:** Apache 2.0 (Qwen3-14B)  
**LM Studio License:** Proprietary (free for personal/commercial use per [LM Studio Terms](https://lmstudio.ai/terms))

---

> [!CAUTION]
> **Human-in-the-Loop (HITL) Requirement**
> This workstation is a **decision-support tool**, not a decision-maker. In accordance with the [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) and industry best practices, all AI-generated outputs (risk statements, control mappings, policy drafts, compliance interpretations) **must be reviewed, validated, and signed off by a qualified human professional** before operational use.

### 1. Hallucination & Accuracy Warning
Large Language Models (LLMs) can "hallucinate" or provide factually incorrect information, including:
- Citing non-existent regulatory sub-clauses (e.g., fabricated "NIST 800-53 AC-99")
- Misinterpreting framework overlaps (e.g., incorrect NIST-to-ISO mappings)
- Generating plausible but invalid compliance logic

**Always verify citations against official source documents:**
- NIST: [https://csrc.nist.gov](https://csrc.nist.gov)
- ISO: [https://www.iso.org](https://www.iso.org)
- HIPAA: [https://www.hhs.gov/hipaa](https://www.hhs.gov/hipaa)
- PCI SSC: [https://www.pcisecuritystandards.org](https://www.pcisecuritystandards.org)

### 2. Professional Accountability
The use of this tool **does not shift the burden of accountability**. The human analyst remains the **Risk Owner** and is solely responsible for:
- Accuracy of compliance statements submitted to auditors or regulators
- Correctness of risk assessments entered into governance platforms
- Validity of policy language approved by executive leadership

Any security failures or compliance gaps resulting from unverified AI guidance are the responsibility of the analyst and their organization, not the AI tool.

### 3. No Legal or Regulatory Advice
Outputs from this workstation **do not constitute**:
- Legal advice or legal opinions
- Formal audit findings or opinions
- Guaranteed regulatory compliance
- Binding interpretations of statutes or regulations

This project is for **demonstration, education, and productivity enhancement** purposes only. Consult qualified legal counsel, certified auditors (CPA, CISA), or compliance professionals (CISSP, CIPP) for binding opinions.

### 4. Local Data Security Responsibility
While this setup mitigates **third-party "data-in-flight"** risk (no cloud transmission), the user is fully responsible for **"data-at-rest"** security:
- **Encryption:** Full Disk Encryption (FDE) with AES-256 is **required** (BitLocker/VeraCrypt).
- **Access Controls:** Physical and logical access to the workstation must follow principle of least privilege.
- **Audit Logging:** For high-risk use cases (HIPAA, PCI-DSS), maintain logs of LLM interactions.
- **Secure Disposal:** When decommissioning, securely wipe model files and chat logs per NIST SP 800-88 guidelines.

Failure to implement these controls may expose sensitive data to unauthorized access, violating HIPAA §164.312, PCI-DSS Req. 3.4, or SOC 2 CC6.1.

---

## 🔗 Additional Resources

### Documentation & Guides
- [LM Studio Offline Operation Guide](https://lmstudio.ai/docs/app/offline)
- [Qwen3 Model Card (Hugging Face)](https://huggingface.co/Qwen/Qwen3-14B)
- [OWASP Top 10 for LLM Applications 2025](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)

### Compliance Frameworks Referenced
- [NIST Special Publication 800-53 Rev 5](https://csrc.nist.gov/publications/detail/sp/800-53/rev-5/final)
- [HIPAA Security Rule](https://www.hhs.gov/hipaa/for-professionals/security/index.html)
- [PCI DSS v4.0.1](https://www.pcisecuritystandards.org/document_library/)
- [SOC 2 Trust Services Criteria](https://www.aicpa-cima.com/topic/audit-assurance/audit-and-assurance-greater-than-soc-2)

### Open Source Alternatives
- [Jan (Open Source LLM Client)](https://github.com/janhq/jan) - Apache 2.0 licensed alternative to LM Studio
- [Ollama](https://ollama.ai/) - Command-line LLM runtime for server deployments
- [llama.cpp](https://github.com/ggerganov/llama.cpp) - Low-level inference engine (for advanced users)

---

**Project Author:** (https://github.com/SpadaSchiavonesca) 
**Last Updated:** February 5, 2026  
**Version:** 2.0 (OWASP 2025 Compliant)

---

## 📄 Related Files in This Repository
- **[PROMPT_LIBRARY.md](PROMPT_LIBRARY.md)** - Curated collection of GRC-specific prompts
- **[SETUP_TROUBLESHOOTING.md](SETUP_TROUBLESHOOTING.md)** - Common AMD/Vulkan configuration issues
- **[SCREENSHOTS/](screenshots/)** - Hardware detection and output examples
- **[LICENSE](LICENSE)** - MIT License for this documentation

---

*Built with LM Studio 0.4.1 | Powered by Qwen3-14B | Secured for GRC Workflows*
