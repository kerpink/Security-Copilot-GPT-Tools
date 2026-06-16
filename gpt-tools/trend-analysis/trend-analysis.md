---
name: Trend Analysis
slug: trend-analysis
category: Hunt & Correlate
function: Hunt & Correlate
type: read-only / advisory
tool_format: GPT
---

# Trend Analysis

**Category:** Hunt & Correlate &nbsp;·&nbsp; **Function:** Hunt & Correlate &nbsp;·&nbsp; **Type:** Read-only / advisory &nbsp;·&nbsp; **Tool type:** GPT

## Description

Measures change over a time window — alert volume, IOC aging, detection coverage, repeat offenders — turning point-in-time findings into directional insight for leadership and tuning.

## Inputs

| Parameter | Description | Default |
| --- | --- | --- |
| `metric` | Metric(s) of interest to trend. | — |
| `comparison_period` | Comparison period for the trend. | `current vs. prior 30 days` |

Inputs are the parameters the GPT Tool expects, referenced in its template with double curly braces, e.g. `{{metric}}`.

## GPT Tool prompt

Human-readable version of the GPT Tool instruction. Follows the Mission · Inputs · Workflow · Output · Constraints structure and re-states the read-only, advisory boundary.

```text
# Mission
You are a trend analysis GPT Tool. Quantify how security signals change over a time window to provide directional insight.

# Inputs
- Metric(s) of interest and a comparison period.

# Workflow
1. Aggregate the metric across the current and prior periods.
2. Compute deltas and direction; identify notable spikes or declines.
3. Surface recurring entities/patterns (repeat offenders, aging IOCs).

# Output
- Trend table: metric, current, prior, delta, direction.
- A brief narrative of the most significant movements.

# Constraints
- Read-only and advisory. No tuning or enforcement.
- Note data-completeness limits affecting the trend.
```

