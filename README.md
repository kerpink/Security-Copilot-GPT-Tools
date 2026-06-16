# Security Copilot — Analyst-Task GPT Tool Pack

A library of **30 read-only / analysis GPT Tools** for Microsoft Security Copilot agents. Each GPT Tool is an *analyst task* — it investigates, scores, correlates, drafts, documents, or reports. **None of them change state** in Defender, Sentinel, Entra, Intune, or any ticketing system; none isolate, disable, block, send, or deploy. Because nothing here touches production, no approval gating is required.

Every GPT Tool ships as a self-contained Markdown file (`<slug>.md`) with:

1. A plain-language **description** of what it does.
2. Its **inputs**, where it takes any.
3. A ready-to-use **prompt** (Mission · Inputs · Workflow · Output · Constraints) that re-states the read-only, advisory boundary.

> **Status:** Internal / Preview. Built for Security Copilot; the prompt bodies are portable to other agent platforms with adaptation.

---

## About GPT Tools

In Security Copilot, **tools** are the capabilities an agent can use to perform specialized tasks. Developers can author two tool types in the agent builder:

- **GPT** — a generative-AI prompt that uses an LLM to reason over natural language or data.
- **KQL** — a parameterized Kusto query that returns data from Defender, Sentinel, or an Azure Data Explorer (Kusto) cluster.

**Everything in this pack is a GPT Tool.** These tools reason over context and telemetry an analyst (or another tool) supplies; they do not run queries or take action themselves. If you want a GPT Tool to pull its own data, pair it with a KQL tool or an [API / MCP plugin](https://learn.microsoft.com/en-us/copilot/security/plugin-api) that fetches the data, then let the GPT Tool reason over the result.

Reference: [Add a GPT tool to your agent](https://learn.microsoft.com/en-us/copilot/security/developer/build-agent-gpt-sample) · [Create tool from the tool types](https://learn.microsoft.com/en-us/copilot/security/developer/create-agent-tool).

---

## How to use a GPT Tool

1. Open the GPT Tool's folder under [`gpt-tools/`](gpt-tools/) (organized by category) and read its `<slug>.md`.
2. Copy the **prompt** block.
3. In the Security Copilot **agent builder**, add a custom tool of type **GPT** and paste the prompt into the template.
4. GPT Tools that list **Inputs** expect typed parameters, referenced in the template with double curly braces (e.g. `{{cve_ids}}`).
5. Keep the read-only / advisory constraints in the prompt intact — they are what make these GPT Tools safe to run without approval gating.

### Shared building blocks

Three GPT Tools are useful for nearly every agent and work well as shared primitives:

- **Confidence Scoring**
- **Audience-Tuned Summarization**
- **Similar / Historical Incident Lookup**

---

## Repository layout

```
.
├── README.md
├── LICENSE
├── gpt-tools/
│   ├── investigate-and-reconstruct/
│   │   └── <tool-slug>/<tool-slug>.md
│   ├── assess-and-score/
│   ├── hunt-and-correlate/
│   ├── map-and-gap-analyze/
│   ├── draft-and-recommend/
│   ├── communicate-and-report/
│   ├── document-and-brief/
│   └── engineer-and-prioritize/
└── .github/
    └── ISSUE_TEMPLATE/
```

Each GPT Tool lives in its own folder containing a single `<slug>.md` file, grouped into the eight functional categories below.

---

## GPT Tool catalog

### Investigate & Reconstruct

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [Timeline Reconstruction](gpt-tools/investigate-and-reconstruct/timeline-reconstruction/timeline-reconstruction.md) | Assembles an ordered, human-readable narrative of an incident from raw telemetry, sequencing alerts, sign-ins, process executions, network connections, and email events into a single chronological story analysts can read at a glance. | `incident`, `time_window` |
| [Root Cause Analysis](gpt-tools/investigate-and-reconstruct/root-cause-analysis/root-cause-analysis.md) | Traces an incident back to its initial access vector and contributing conditions, distinguishing the true root cause from downstream symptoms and identifying the control that should have prevented it. | `incident` |
| [Blast Radius Scoping](gpt-tools/investigate-and-reconstruct/blast-radius-scoping/blast-radius-scoping.md) | Enumerates the full reach of a compromise: every device, identity, file, mailbox, and resource a malicious entity touched or could have touched, so responders understand containment scope before acting. | `entity`, `time_window` |
| [Entity Relationship Mapping](gpt-tools/investigate-and-reconstruct/entity-relationship-mapping/entity-relationship-mapping.md) | Builds the relationship graph linking users, devices, files, IPs, and sessions involved in an investigation, surfacing pivot points and shared infrastructure that flat lists obscure. | `seed` |

### Assess & Score

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [Severity & Triage Scoring](gpt-tools/assess-and-score/severity-and-triage-scoring/severity-and-triage-scoring.md) | Recommends an incident/alert priority (P1-P4) with explicit rationale based on asset criticality, scope, exploitation evidence, and business impact, so the queue is ranked rather than flat. | `alert` |
| [Risk & Exposure Scoring](gpt-tools/assess-and-score/risk-and-exposure-scoring/risk-and-exposure-scoring.md) | Rates the residual risk of an asset, identity, or finding using exposure, criticality, and existing controls, producing a comparable score that lets teams prioritize across many findings. | `target` |
| [Exploit Likelihood Enrichment](gpt-tools/assess-and-score/exploit-likelihood-enrichment/exploit-likelihood-enrichment.md) | Cross-references a CVE against authoritative exploitation signals — CISA KEV listing, EPSS probability, and known public proof-of-concept availability — to contextualize urgency without inferring beyond sources. | `cve_ids` |
| [False Positive Assessment](gpt-tools/assess-and-score/false-positive-assessment/false-positive-assessment.md) | Evaluates whether an alert is likely benign by comparing it against known good behavior, business context, and historical dispositions, with a clear rationale — reducing analyst time spent on noise. | `alert` |
| [Confidence Scoring](gpt-tools/assess-and-score/confidence-scoring/confidence-scoring.md) | A shared primitive that attaches an explicit, evidence-backed confidence level to any conclusion the agent produces, with the corroborating and detracting evidence named — making every output auditable. | — |

### Hunt & Correlate

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [Similar / Historical Incident Lookup](gpt-tools/hunt-and-correlate/similar-historical-incident-lookup/similar-historical-incident-lookup.md) | Searches prior incidents and closed cases for matches on indicators, behavior, or entities — giving the agent institutional memory so analysts learn whether 'we've seen this before' and how it was resolved. | `incident` |
| [Cross-Source Correlation](gpt-tools/hunt-and-correlate/cross-source-correlation/cross-source-correlation.md) | Stitches together email, identity, endpoint, and network telemetry around a common entity or time window to reveal a unified picture that any single product view misses. | `pivot` |
| [Anomaly / Baseline Comparison](gpt-tools/hunt-and-correlate/anomaly-baseline-comparison/anomaly-baseline-comparison.md) | Compares an entity's current behavior against its own historical baseline (sign-in patterns, send/receive volume, process activity) to flag meaningful deviations rather than absolute thresholds. | `entity`, `baseline_window` |
| [Threat Actor Attribution](gpt-tools/hunt-and-correlate/threat-actor-attribution/threat-actor-attribution.md) | Maps observed tradecraft and infrastructure to known threat actors or campaigns using Defender Threat Intelligence, presenting candidate attributions with supporting overlap and explicit uncertainty. | `observables` |
| [Trend Analysis](gpt-tools/hunt-and-correlate/trend-analysis/trend-analysis.md) | Measures change over a time window — alert volume, IOC aging, detection coverage, repeat offenders — turning point-in-time findings into directional insight for leadership and tuning. | `metric`, `comparison_period` |

### Map & Gap-Analyze

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [MITRE ATT&CK Coverage Scoring](gpt-tools/map-and-gap-analyze/mitre-attack-coverage-scoring/mitre-attack-coverage-scoring.md) | Scores which ATT&CK techniques observed (or simulated) were detected, alerted, blocked, or missed, producing a coverage heat picture that makes blind spots comparable across runs. | `techniques` |
| [Control & Compliance Mapping](gpt-tools/map-and-gap-analyze/control-and-compliance-mapping/control-and-compliance-mapping.md) | Ties findings, gaps, or vulnerabilities to specific control frameworks (NIST CSF, CIS, SOC 2), so technical results translate into audit and governance language leadership can act on. | `findings`, `framework` |
| [Detection Gap Analysis](gpt-tools/map-and-gap-analyze/detection-gap-analysis/detection-gap-analysis.md) | Compares observed adversary behavior against the detections that should have fired across email, identity, and endpoint, naming missing, delayed, or underperforming controls. | `behaviors` |
| [Log Source Coverage Check](gpt-tools/map-and-gap-analyze/log-source-coverage-check/log-source-coverage-check.md) | Verifies which telemetry sources are present and healthy for a given investigation or detection goal, so analysts know whether 'no evidence' means clean or simply unmonitored. | `goal` |

### Draft & Recommend

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [Detection Rule Drafting](gpt-tools/draft-and-recommend/detection-rule-drafting/detection-rule-drafting.md) | Generates candidate KQL / analytics-rule TEXT for a confirmed gap or hunt — ready for an engineer to review and deploy — without ever deploying it itself. | `target_behavior` |
| [Remediation Playbook Recommendation](gpt-tools/draft-and-recommend/remediation-playbook-recommendation/remediation-playbook-recommendation.md) | Proposes the appropriate response runbook and step sequence for a given incident type, with prerequisites and decision points — a recommendation for humans, not an execution. | `incident_type` |
| [Hypothesis Generation](gpt-tools/draft-and-recommend/hypothesis-generation/hypothesis-generation.md) | Proposes prioritized, testable hunt hypotheses from current signal — surfacing leads worth pursuing along with the telemetry needed to validate each. | `context` |
| [Lessons Learned Extraction](gpt-tools/draft-and-recommend/lessons-learned-extraction/lessons-learned-extraction.md) | Distills a resolved incident or hunt into blameless improvement findings — what worked, what failed, and which detections, processes, or controls to strengthen. | `incident` |

### Communicate & Report

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [Audience-Tuned Summarization](gpt-tools/communicate-and-report/audience-tuned-summarization/audience-tuned-summarization.md) | Produces the same analysis at multiple altitudes — executive, SOC-technical, and customer-facing — so one investigation serves leadership, responders, and external stakeholders without rework. | `source_findings`, `audiences` |
| [Communication Drafting](gpt-tools/communicate-and-report/communication-drafting/communication-drafting.md) | Drafts the stakeholder notification, ticket body, or KB article text for an incident — formatted and ready for a human to review and send, but never sent by the agent. | `comm_type` |
| [Evidence Packaging](gpt-tools/communicate-and-report/evidence-packaging/evidence-packaging.md) | Assembles a cited, chain-of-custody-style evidence pack for an investigation — every conclusion linked to its source artifact — supporting handoff, escalation, and audit. | `incident` |

### Document & Brief

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [Technical Documentation Writer](gpt-tools/document-and-brief/technical-documentation-writer/technical-documentation-writer.md) | Turns raw incident, hunt, or process material into polished long-form documentation — runbooks, knowledge-base articles, and process docs — with consistent structure, headings, and tone an analyst can publish with minimal editing. | `source_material`, `doc_type` |
| [Executive Threat Briefing](gpt-tools/document-and-brief/executive-threat-briefing/executive-threat-briefing.md) | Distills a body of security findings into a board- or CISO-level briefing: what happened, the business risk, what is being done, and the decisions required — in plain language with no operational jargon. | `findings`, `decision_context` |
| [Incident Status Report](gpt-tools/document-and-brief/incident-status-report/incident-status-report.md) | Produces a structured, repeatable status update for an active incident — current state, what is confirmed, actions in flight, blockers, and the next update time — so responders and stakeholders stay aligned during a live response. | `incident`, `audience` |

### Engineer & Prioritize

| GPT Tool | What it does | Inputs |
| --- | --- | --- |
| [Detection Engineering Review](gpt-tools/engineer-and-prioritize/detection-engineering-review/detection-engineering-review.md) | Reviews an existing detection rule (KQL or analytic logic) for coverage gaps, false-positive risk, performance concerns, and ATT&CK alignment, returning prioritized, advisory improvement notes without changing or deploying the rule. | `rule`, `intent` |
| [Vulnerability Prioritization](gpt-tools/engineer-and-prioritize/vulnerability-prioritization/vulnerability-prioritization.md) | Ranks a set of CVEs or findings into a defensible remediation order using exploitability, asset exposure, and business context — turning a flat scanner dump into a prioritized 'fix these first' list with rationale. | `findings`, `context` |

---

## Conventions

- **Read-only by contract.** Every prompt ends with constraints that forbid state changes. If you fork a GPT Tool, preserve them.
- **GPT Tools only.** This pack contains GPT-type tools. For data retrieval, add KQL tools or API/MCP plugins alongside them.
- **Inputs are templated.** Declared inputs are referenced as `{name}` inside the prompt, per the Security Copilot GPT-tool format.
- **One folder, one GPT Tool.** Add new tools as `gpt-tools/<category>/<slug>/<slug>.md` and follow the format in any existing file.

## License

See [LICENSE](LICENSE).
