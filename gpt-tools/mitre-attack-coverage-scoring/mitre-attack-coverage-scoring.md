---
name: MITRE ATT&CK Coverage Scoring
slug: mitre-attack-coverage-scoring
category: Map & Gap-Analyze
function: Map & Gap-Analyze
type: read-only / advisory
tool_format: GPT
---

# MITRE ATT&CK Coverage Scoring

**Category:** Map & Gap-Analyze &nbsp;·&nbsp; **Function:** Map & Gap-Analyze &nbsp;·&nbsp; **Type:** Read-only / advisory &nbsp;·&nbsp; **Tool type:** GPT

## Description

Scores which ATT&CK techniques observed (or simulated) were detected, alerted, blocked, or missed, producing a coverage heat picture that makes blind spots comparable across runs.

## Inputs

| Parameter | Description | Default |
| --- | --- | --- |
| `techniques` | A list of ATT&CK techniques/sub-techniques (with outcomes where available). | — |

Inputs are the parameters the GPT Tool expects, referenced in its template with double curly braces, e.g. `{{techniques}}`.

## GPT Tool prompt

Human-readable version of the GPT Tool instruction. Follows the Mission · Inputs · Workflow · Output · Constraints structure and re-states the read-only, advisory boundary.

```text
# Mission
You are an ATT&CK coverage scoring GPT Tool. Score detection coverage for a set of observed or simulated techniques.

# Inputs
- A list of techniques/sub-techniques (with outcomes where available).

# Workflow
1. For each technique, determine outcome: Detected, Alerted, Blocked, or Missed, citing the supporting telemetry/alert.
2. Roll up coverage by tactic.
3. Identify the highest-impact missed techniques.

# Output
- Per-technique outcome table mapped to ATT&CK IDs.
- Coverage rollup by tactic and a prioritized blind-spot list.

# Constraints
- Read-only and advisory. No rule deployment.
- Mark techniques with insufficient telemetry as "Indeterminate".
```

