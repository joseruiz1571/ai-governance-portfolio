# AI Vendor Due Diligence Report
**Vendor:** Anthropic, PBC
**Tool / Service:** Claude Code (AI-powered coding assistant, CLI + IDE integration)
**Prepared by:** Jose Ruiz-Vazquez, AI Governance Practitioner
**Date:** April 17, 2026
**Classification:** Internal — Confidential
**Review Cycle:** Annual or upon material vendor change / security incident

---

## Executive Summary

This report documents a due diligence assessment of Anthropic's Claude Code for potential internal deployment within a SaaS company's software development function. Claude Code is an agentic AI coding assistant delivered via CLI, IDE extensions, and API, designed to support developer workflows including code generation, debugging, and autonomous task execution.

Anthropic maintains strong governance infrastructure — including a Transparency Hub, detailed system cards, SOC 2 Type II and ISO 27001 certifications, and ISO 42001 AI Management System certification. Commercial API deployments are contractually protected from use as training data, and Anthropic has signed the EU General-Purpose AI Code of Practice. These are meaningful controls.

However, two material risks discovered during this review require documented mitigation before approval. First, on March 31, 2026, Anthropic inadvertently exposed 513,000 lines of Claude Code's TypeScript source via a mispackaged npm release — within days, a critical RCE vulnerability (exploitable via malicious `CLAUDE.md` files and prompt injection into the agent pipeline) was publicly disclosed. Second, in February 2026, Anthropic removed the training-pause requirement from its Responsible Scaling Policy under competitive pressure, weakening a previously public safety commitment.

**Decision: Approve with Conditions.** Claude Code may be deployed internally subject to the mitigations listed in Section 6. The security incident is the primary gating concern; legal and compliance review of the DPA and commercial terms is required before production rollout.

---

## 1. Vendor Overview

| Field | Detail |
|-------|--------|
| **Vendor** | Anthropic, PBC |
| **Product / Service** | Claude Code (CLI + IDE extensions + API) |
| **Use Case** | Internal developer tooling — code generation, debugging, autonomous task execution |
| **Deployment Model** | SaaS / API (cloud-hosted; no on-premise option) |
| **Data Processed** | Source code, developer prompts, repository context, potentially internal credentials if not scoped |
| **Sector Regulations** | General SaaS — SOC 2, GDPR/CCPA, EU AI Act GPAI |
| **Assessment Date** | April 17, 2026 |

---

## 2. Risk Dimension Scores

| # | Dimension | Score | Rating | Key Finding |
|---|-----------|-------|--------|-------------|
| 1 | Data Usage & Privacy | 4/5 | Strong | Commercial API/enterprise accounts explicitly excluded from model training; DPA available; 30-day retention default |
| 2 | Model Risk & Reliability | 4/5 | Strong | 212-page system card for Claude Opus 4.6; ASL safety levels documented; RSP v3.0 published |
| 3 | Transparency & Explainability | 5/5 | Exemplary | Transparency Hub launched Feb 2026; system cards for all model families; Risk Reports every 3–6 months with external review |
| 4 | Bias & Fairness | 3/5 | Adequate | Constitutional AI methodology documented; political bias assessment tool launched; no published enterprise-grade fairness benchmarks |
| 5 | Security Posture | 2/5 | Weak | SOC 2 Type II + ISO 27001 certified, BUT: source code leak March 2026 + critical RCE vulnerability (CVE-2026-21852) disclosed April 2026 |
| 6 | Regulatory Alignment | 4/5 | Strong | Signed EU GPAI Code of Practice; ISO 42001:2023 certified; GDPR DPA and HIPAA BAA available |
| 7 | Governance & Accountability | 3/5 | Adequate | RSP v3.0 published; Transparency Hub active; BUT training-pause requirement removed Feb 2026 under competitive pressure |
| | **OVERALL AVERAGE** | **3.57/5** | **Low-Moderate Risk** | |

---

## 3. Detailed Findings

### 3.1 Data Usage & Privacy — Score: 4/5

**Key finding:** Anthropic's August 2025 privacy policy update introduced a consumer opt-in for training data use — however, this explicitly does *not* apply to commercial accounts, Claude for Work, or API usage. Enterprise and commercial customers retain the previous protections: data is not used to train models and is retained for 30 days by default. A Data Processing Addendum (DPA) is available and should be executed before deployment.

**Gaps:** Sub-processor disclosure list not publicly available (must be requested). CCPA-specific compliance documentation is not published in detail.

**Sources:** Anthropic Privacy Center; TechCrunch (Aug 28, 2025); amstlegal.com analysis

---

### 3.2 Model Risk & Reliability — Score: 4/5

**Key finding:** Anthropic publishes detailed system cards with each model family release. The Claude Opus 4.6 system card runs 212 pages — one of the most comprehensive self-assessments in the industry. Models are evaluated across CBRN risk domains, cybersecurity uplift, and autonomous AI R&D potential, assigned to safety levels ASL-2 through ASL-4. Risk Reports are produced every 3–6 months and published publicly with limited redactions.

**Gaps:** RSP v3.0 (Feb 2026) removed the requirement to pause training if capabilities outpace safety controls, introducing dependency on Anthropic's internal judgment without a defined external trigger.

**Sources:** anthropic.com/transparency; anthropic.com/responsible-scaling-policy; Vellum AI (Claude Mythos overview)

---

### 3.3 Transparency & Explainability — Score: 5/5

**Key finding:** Anthropic launched a centralized Transparency Hub in February 2026 consolidating model cards, system cards, safeguards documentation, and release notes. This is among the most structured transparency programs of any frontier AI vendor. External review of Risk Reports under defined conditions has been formalized. No other major AI vendor currently publishes a 200+ page system card at time of model release.

**Gaps:** Model output explainability at inference time remains limited — Claude Code does not expose chain-of-thought reasoning to end users by default.

**Sources:** anthropic.com/transparency; insights.marvin-42.com; x.com/@elmd_ (April 2026)

---

### 3.4 Bias & Fairness — Score: 3/5

**Key finding:** Anthropic's Constitutional AI (Constitutional RL) methodology is documented and positions bias mitigation as a design principle, not a post-hoc filter. A political bias assessment tool was published in 2025. However, no enterprise-grade fairness benchmark report is publicly available — quantified bias metrics across protected characteristics (race, gender, language) are not published for Claude Code specifically.

**Gaps:** No published bias evaluation specific to code generation use cases. No commitment to periodic third-party bias audits. RSP rollback in Feb 2026 reduces accountability signal.

**Sources:** Contrary Research bias/fairness report; ai-daily.news; CNN Business (Feb 25, 2026)

---

### 3.5 Security Posture — Score: 2/5

**Key finding — HIGH CONCERN:** Anthropic holds SOC 2 Type II, ISO 27001:2022, and ISO 42001:2023 certifications, with documentation available via trust.anthropic.com under NDA. However, two material security events occurred in close proximity:

1. **Source Code Leak (March 31, 2026):** A 59.8 MB JavaScript source map containing 513,000 lines of unobfuscated TypeScript was accidentally bundled in `@anthropic-ai/claude-code` npm package v2.1.88. The codebase was mirrored across GitHub within hours. Anthropic confirmed no customer data or credentials were exposed, attributing the incident to human error in release packaging.

2. **Critical RCE Vulnerability (April 2026, CVE-2026-21852):** Within days of the leak, a critical vulnerability was publicly disclosed: malicious `CLAUDE.md` files can inject instructions that cause Claude Code to generate 50+ subcommand pipelines, enabling remote code execution and API key exfiltration. A separate supply chain attack using weaponized npm packages impersonating Claude Code tooling was reported the same day as the leak.

Anthropic has confirmed patches are in progress. Until a patched version is confirmed and validated, deployment of Claude Code in production environments with privileged access is not recommended.

**Sources:** The Hacker News (April 2026); SecurityWeek (April 2026); Fortune (March 31, 2026); Zscaler ThreatLabz; Straiker.ai

---

### 3.6 Regulatory Alignment — Score: 4/5

**Key finding:** Anthropic announced intent to sign the EU General-Purpose AI Code of Practice in July 2025, aligning to transparency, safety, and accountability obligations under the EU AI Act. EU AI Act GPAI rules (applicable to Claude as a general-purpose AI model) took effect August 2025. ISO 42001:2023 (AI Management Systems) certification is held. GDPR DPA and HIPAA BAA are available for commercial customers.

**Gaps:** No public statement on CCPA-specific compliance program. EU high-risk system rules (August 2026 enforcement) may impose additional obligations depending on deployment context.

**Sources:** anthropic.com/news/eu-code-practice; legalnodes.com (EU AI Act 2026 updates); Anthropic Privacy Center

---

### 3.7 Governance & Accountability — Score: 3/5

**Key finding:** Anthropic maintains one of the more mature AI governance programs among frontier AI vendors — including a versioned Responsible Scaling Policy (v3.0, Feb 2026), a public Transparency Hub, Constitutional AI methodology, and dedicated safety/policy leadership. However, the February 2026 revision to the RSP removed the requirement to pause model training when capabilities outpace safety controls, citing competitive pressure. This was publicly reported as abandoning a core safety commitment.

**Gaps:** No independent external board with authority over safety decisions. Named AI ethics function exists but reporting structure not publicly disclosed.

**Sources:** anthropic.com/responsible-scaling-policy; CNN Business (Feb 25, 2026); edtechinnovationhub.com

---

## 4. NIST AI RMF Alignment

| Function | Vendor Capabilities | Gaps | Enterprise Controls Needed |
|----------|---------------------|------|---------------------------|
| **GOVERN** | RSP v3.0; Transparency Hub; Constitutional AI policy; ISO 42001 certified | RSP training-pause commitment removed Feb 2026; no independent oversight board | Supplement with internal AI use policy; require Anthropic contractual safety commitments |
| **MAP** | System cards document failure modes and use case risks; ASL risk levels defined | No Claude Code-specific use case risk mapping for agentic coding workflows | Internal context mapping: document data flows, privileged access scope, developer trust boundaries |
| **MEASURE** | 212-page system cards; Risk Reports every 3–6 months; external review under defined conditions | No published bias benchmarks for code generation; no quantified reliability SLAs | Internal monitoring: log Claude Code outputs for anomaly detection; periodic internal red team |
| **MANAGE** | SOC 2 Type II; ISO 27001; DPA available; incident response exists | Active unpatched RCE vulnerability (CVE-2026-21852); no public patch timeline | Delay production deployment until patch confirmed; restrict CLAUDE.md trust settings; pin npm package version |

---

## 5. Risk Register

| Risk ID | Description | Likelihood | Impact | Risk Level | Recommended Control | Owner | Status |
|---------|-------------|-----------|--------|-----------|---------------------|-------|--------|
| DD-01 | CVE-2026-21852: RCE via malicious CLAUDE.md prompt injection — exploitable in current release | High | Critical | **Critical** | Do not deploy current version; monitor Anthropic security advisories; deploy only after confirmed patch | Security | Open |
| DD-02 | Source code leak (March 2026) increases attack surface — undiscovered vulnerabilities may be present | Medium | High | **High** | Pin to patched npm version; run SAST against Claude Code in CI/CD pipeline | Security | Open |
| DD-03 | Consumer training opt-in policy (Aug 2025) — risk of misclassification if commercial vs. consumer accounts not clearly scoped | Low | High | **Moderate** | Execute DPA before deployment; confirm account type is commercial/enterprise; document in vendor inventory | Legal/Compliance | Open |
| DD-04 | RSP rollback (Feb 2026) weakens Anthropic's training pause commitment — future model capability jumps may not trigger pause | Low | Medium | **Moderate** | Track RSP changes annually; include contractual model change notification clause in enterprise agreement | Compliance | Open |
| DD-05 | No published bias benchmarks for code generation use cases — risk of biased code suggestions | Low | Medium | **Low-Moderate** | Conduct internal red team on code outputs for biased patterns before broad rollout | Engineering | Open |
| DD-06 | Agentic execution scope — Claude Code can run shell commands, write files, and interact with APIs; over-privileged deployment increases blast radius | Medium | High | **High** | Deploy with minimal privilege; restrict filesystem and shell access; enforce human-in-the-loop for destructive commands | Security/Eng | Open |

---

## 6. Recommendation

**Decision: Approve with Conditions**

Claude Code is a well-governed AI tool with strong transparency, solid regulatory alignment, and enterprise-grade certification. For a SaaS company's internal developer use, it is appropriate — *subject to the following conditions being met before production deployment:*

1. **Gate on security patch:** Do not deploy in production until CVE-2026-21852 is patched and confirmed by Anthropic. Pin npm package version to a patched release.
2. **Execute DPA:** Legal must review and execute Anthropic's Data Processing Addendum to formally establish commercial account protections and data retention terms.
3. **Restrict agentic scope:** Deploy with explicit configuration limits — disable or require human approval for shell execution, restrict CLAUDE.md trust to internal repositories only, enforce allowlists for tool use.
4. **Vendor inventory entry:** Register Claude Code in the enterprise AI tool inventory with this assessment on file, review cycle set to annual or triggered by material change.
5. **Annual RSP monitoring:** Track Anthropic's Responsible Scaling Policy for further rollbacks; include model change notification requirement in enterprise contract.

**Review Trigger:** Reassess immediately upon: (a) new Anthropic security advisory, (b) material RSP revision, (c) change in Anthropic's EU AI Act compliance posture after August 2026 high-risk enforcement deadline.

---

## 7. Sources & Evidence

| # | Document | Date Retrieved | URL |
|---|----------|---------------|-----|
| 1 | Anthropic Privacy Center — Data Training Policy | April 17, 2026 | https://privacy.claude.com/en/articles/10023580-is-my-data-used-for-model-training |
| 2 | TechCrunch — Anthropic opt-in training change | April 17, 2026 | https://techcrunch.com/2025/08/28/anthropic-users-face-a-new-choice-opt-out-or-share-your-data-for-ai-training/ |
| 3 | Anthropic Transparency Hub | April 17, 2026 | https://www.anthropic.com/transparency |
| 4 | Anthropic Responsible Scaling Policy | April 17, 2026 | https://www.anthropic.com/responsible-scaling-policy |
| 5 | Anthropic Trust Center | April 17, 2026 | https://trust.anthropic.com/ |
| 6 | The Hacker News — Claude Code Source Leak | April 17, 2026 | https://thehackernews.com/2026/04/claude-code-tleaked-via-npm-packaging.html |
| 7 | SecurityWeek — Critical Vulnerability | April 17, 2026 | https://www.securityweek.com/critical-vulnerability-in-claude-code-emerges-days-after-source-leak/ |
| 8 | Fortune — Second Security Lapse | April 17, 2026 | https://fortune.com/2026/03/31/anthropic-source-code-claude-code-data-leak-second-security-lapse-days-after-accidentally-revealing-mythos/ |
| 9 | CNN Business — RSP Rollback | April 17, 2026 | https://www.cnn.com/2026/02/25/tech/anthropic-safety-policy-change |
| 10 | Anthropic EU Code of Practice | April 17, 2026 | https://www.anthropic.com/news/eu-code-practice |
| 11 | Zscaler ThreatLabz — Claude Code Leak Analysis | April 17, 2026 | https://www.zscaler.com/blogs/security-research/anthropic-claude-code-leak |
| 12 | Straiker — Agent Trust Analysis | April 17, 2026 | https://www.straiker.ai/blog/claude-code-source-leak-with-great-agency-comes-great-responsibility |

---

*Report generated using the AIVendorDD assessment framework, aligned to NIST AI RMF v1.0 and applicable global AI regulations. This assessment reflects information available as of the date of review and should not be construed as legal advice.*
