# Suite report — `official_opus_baseline`

Mean over all (app, rep) samples per condition. Δ = hints − baseline; %Δ relative to baseline. For efficiency metrics, negative Δ = hints is cheaper (good).

Samples — **baseline**: 140 app-runs, **hints**: 140 app-runs.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| overall F1 | 0.905 | 0.911 | +0.006 | +0.7% ✅ |
| overall precision | 0.941 | 0.928 | -0.013 | -1.3% ⚠️ |
| overall recall | 0.894 | 0.912 | +0.018 | +2.0% ✅ |
| turns | 161 | 156 | -4.700 | -2.9% ✅ |
| tool calls | 160 | 155 | -4.700 | -2.9% ✅ |
| duration (s) | 612 | 599 | -13.585 | -2.2% ✅ |
| cache-read tokens | 9,074,596 | 7,672,962 | -1,401,634 | -15.4% ✅ |
| output tokens | 32,229 | 32,877 | +647 | +2.0% ⚠️ |

## Latency decomposition

`model time` + `tool time` + `other` = `duration (s)`. **model time** is request round-trip (prefill + decode); **tool time** is Playwright/MCP execution; **other** is rate-limit backoff, retries and harness overhead — noise, not a latency signal, and the floor a latency claim must clear. `output tokens per turn` vs `turns` separates *fewer steps* from *less said per step*; only the product of the two moves wall clock, because `effective output tok/s` is near-fixed for a given model.

Timing available for **140/140** baseline and **140/140** hints app-runs (needs the Claude-Code transcript or a cached `timing.json`).

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| model time (s) | 460 | 449 | -11.088 | -2.4% ✅ |
| tool time (s) | 98.008 | 84.009 | -13.999 | -14.3% ✅ |
| other / overhead (s) | 54.737 | 66.239 | +11.502 | +21.0% ⚠️ |
| model s per turn | 2.925 | 2.919 | -0.007 | -0.2% ✅ |
| tool s per call | 0.639 | 0.553 | -0.086 | -13.5% ✅ |
| output tokens per turn | 207 | 217 | +9.562 | +4.6% ⚠️ |
| effective output tok/s | 70.716 | 74.594 | +3.878 | +5.5% ✅ |

## Dispersion across reps

Spread of the critical loop/latency/token metrics across reps per condition. **median** = typical value (robust to a runaway rep), **CV** = std/mean spread (lower ⇒ steadier), **p95** = worst-case tail. All lower-is-better. ⚠️ CV pools 14 apps, so it mixes between-app and between-rep variance — read per-app for a clean stability signal.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| turns — median | 160 | 148 | -11.000 | -6.9% ✅ |
| turns — CV | 0.291 | 0.279 | -0.013 | -4.3% ✅ |
| turns — p95 | 244 | 245 | +0.300 | +0.1% ⚠️ |
| tool calls — median | 158 | 148 | -11.000 | -6.9% ✅ |
| tool calls — CV | 0.293 | 0.281 | -0.013 | -4.3% ✅ |
| tool calls — p95 | 243 | 244 | +0.300 | +0.1% ⚠️ |
| duration (s) — median | 544 | 542 | -2.350 | -0.4% ✅ |
| duration (s) — CV | 0.310 | 0.333 | +0.022 | +7.2% ⚠️ |
| duration (s) — p95 | 980 | 966 | -13.980 | -1.4% ✅ |
| model time (s) — median | 404 | 403 | -1.200 | -0.3% ✅ |
| model time (s) — CV | 0.327 | 0.354 | +0.027 | +8.3% ⚠️ |
| model time (s) — p95 | 782 | 753 | -29.110 | -3.7% ✅ |
| output tokens — median | 28,840 | 30,326 | +1,486 | +5.2% ⚠️ |
| output tokens — CV | 0.307 | 0.312 | +0.004 | +1.4% ⚠️ |
| output tokens — p95 | 51,685 | 52,335 | +650 | +1.3% ⚠️ |

_Source: `summary/metrics.csv`._
