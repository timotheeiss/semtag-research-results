# Suite report — `official_faulty_baseline`

Mean over all (app, rep) samples per condition. Δ = hints − baseline; %Δ relative to baseline. For efficiency metrics, negative Δ = hints is cheaper (good).

Samples — **baseline**: 140 app-runs, **hints**: 0 app-runs.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| overall F1 | 0.477 | — | — | — |
| overall precision | 0.592 | — | — | — |
| overall recall | 0.446 | — | — | — |
| turns | 121 | — | — | — |
| tool calls | 119 | — | — | — |
| duration (s) | 414 | — | — | — |
| cache-read tokens | 7,128,264 | — | — | — |
| output tokens | 22,932 | — | — | — |

## Latency decomposition

`model time` + `tool time` + `other` = `duration (s)`. **model time** is request round-trip (prefill + decode); **tool time** is Playwright/MCP execution; **other** is rate-limit backoff, retries and harness overhead — noise, not a latency signal, and the floor a latency claim must clear. `output tokens per turn` vs `turns` separates *fewer steps* from *less said per step*; only the product of the two moves wall clock, because `effective output tok/s` is near-fixed for a given model.

Timing available for **140/140** baseline and **0/0** hints app-runs (needs the Claude-Code transcript or a cached `timing.json`).

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| model time (s) | 310 | — | — | — |
| tool time (s) | 47.141 | — | — | — |
| other / overhead (s) | 56.633 | — | — | — |
| model s per turn | 2.530 | — | — | — |
| tool s per call | 0.397 | — | — | — |
| output tokens per turn | 189 | — | — | — |
| effective output tok/s | 76.646 | — | — | — |

## Dispersion across reps

Spread of the critical loop/latency/token metrics across reps per condition. **median** = typical value (robust to a runaway rep), **CV** = std/mean spread (lower ⇒ steadier), **p95** = worst-case tail. All lower-is-better. ⚠️ CV pools 14 apps, so it mixes between-app and between-rep variance — read per-app for a clean stability signal.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| turns — median | 114 | — | — | — |
| turns — CV | 0.311 | — | — | — |
| turns — p95 | 190 | — | — | — |
| tool calls — median | 113 | — | — | — |
| tool calls — CV | 0.312 | — | — | — |
| tool calls — p95 | 188 | — | — | — |
| duration (s) — median | 352 | — | — | — |
| duration (s) — CV | 0.542 | — | — | — |
| duration (s) — p95 | 970 | — | — | — |
| model time (s) — median | 259 | — | — | — |
| model time (s) — CV | 0.518 | — | — | — |
| model time (s) — p95 | 711 | — | — | — |
| output tokens — median | 19,186 | — | — | — |
| output tokens — CV | 0.469 | — | — | — |
| output tokens — p95 | 45,215 | — | — | — |

_Source: `summary/metrics.csv`._
