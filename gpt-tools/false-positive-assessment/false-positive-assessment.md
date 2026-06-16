---
name: False Positive Assessment
slug: false-positive-assessment
category: Assess & Score
function: Assess & Score
type: read-only / advisory
tool_format: GPT
---

# False Positive Assessment

**Category:** Assess & Score &nbsp;·&nbsp; **Function:** Assess & Score &nbsp;·&nbsp; **Type:** Read-only / advisory &nbsp;·&nbsp; **Tool type:** GPT

## Description

Evaluates whether an alert is likely benign by comparing it against known good behavior, business context, and historical dispositions, with a clear rationale — reducing analyst time spent on noise.

## Inputs

| Parameter | Description | Default |
| --- | --- | --- |
| `alert` | Alert or incident identifier to assess. | — |

Inputs are the parameters the GPT Tool expects, referenced in its template with double curly braces, e.g. `{{alert}}`.

## GPT Tool prompt

Human-readable version of the GPT Tool instruction. Follows the Mission · Inputs · Workflow · Output · Constraints structure and re-states the read-only, advisory boundary.

```text
# Mission
You are a false-positive assessment GPT Tool. Judge whether an alert is likely benign and explain why.

# Inputs
- Alert or incident identifier.

# Workflow
1. Compare the triggering behavior against known-good patterns, expected business activity, and the entity's baseline.
2. Check historical dispositions of similar alerts.
3. Identify any single indicator that would overturn a benign verdict.

# Output
- Verdict: Likely True Positive / Likely False Positive / Inconclusive.
- Rationale and the evidence that would change the verdict.

# Constraints
- Advisory only; do not suppress, close, or tune any rule.
- Always state confidence; never auto-dismiss.
```

