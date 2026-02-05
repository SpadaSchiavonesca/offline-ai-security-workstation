# 📚 GRC Professional Prompt Library

This library contains high-fidelity prompts optimized for **Qwen3 14B**. These are designed to handle sensitive data locally, ensuring compliance with internal data protection policies.

---

## 🔍 Use Case 1: SOC 2 Technical Exception Mapping
**Scenario:** A vendor's SOC 2 Type II report shows a "Testing Exception" regarding their database encryption or access reviews. 
**Goal:** Determine if this exception creates a "Residual Risk" for your organization.

> **Prompt:**  
> "Act as a Senior GRC Analyst. I am reviewing a vendor's SOC 2 Type II report. In Section IV, the auditor noted an exception: 'For 3 out of 25 samples, the vendor could not provide evidence of timely access revocation for terminated employees.' 
> 
> Task:
> 1. Identify which Trust Services Criteria (TSC) this impacts (e.g., CC6.1).
> 2. Assess the risk to our data confidentiality if we share PII with this vendor.
> 3. Provide three 'Compensating Controls' we should ask the vendor if they have in place to mitigate this specific failure."

---

## 🛠️ Use Case 2: NIST 800-53 Control Gap Analysis (Technical Evidence)
**Scenario:** You have a raw configuration output (e.g., from an AWS bucket policy) and need to see if it meets NIST standards.
**Goal:** Objective technical audit.

> **Prompt:**  
> "Context: I am auditing our cloud environment against NIST 800-53 AC-3 (Access Enforcement). 
> 
> Evidence to Analyze:
> 'S3 Bucket Policy: {Effect: Allow, Principal: *, Action: s3:GetObject, Resource: arn:aws:s3:::hr-records/*}'
> 
> Task:
> 1. Does this policy comply with the principle of Least Privilege?
> 2. Write a formal 'Audit Finding' including the Condition, Criteria (NIST AC-3), Cause, and Effect.
> 3. Provide the exact JSON policy snippet that would remediate this to restrict access only to the 'Finance-Role' IAM role."

---

## ⚠️ Use Case 3: Quantitative Risk Statement Generation
**Scenario:** You found a vulnerability (e.g., No MFA on a legacy VPN) and need to present it to management.
**Goal:** Moving from "this is bad" to a professional Risk Statement.

> **Prompt:**  
> "Generate a formal Risk Statement for the following finding: 'Our legacy Cisco VPN used by the dev team does not support MFA and relies on local passwords.'
> 
> Format the output using the following structure:
> 1. **Risk Event:** [Description]
> 2. **Threat Actor:** [Likely adversary]
> 3. **Vulnerability:** [The specific weakness]
> 4. **Likelihood Rating:** [1-5 Scale] with justification based on modern credential stuffing trends.
> 5. **Impact Rating:** [1-5 Scale] considering our database is behind this VPN.
> 6. **Mitigation Options:** Provide a 'Short-term' (Low cost) and 'Long-term' (Strategic) solution."
