# Suite report — `official_haiku`

Mean over all (app, rep) samples per condition. Δ = hints − baseline; %Δ relative to baseline. For efficiency metrics, negative Δ = hints is cheaper (good).

Samples — **baseline**: 140 app-runs, **hints**: 140 app-runs.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| overall F1 | 0.470 | 0.569 | +0.099 | +21.1% ✅ |
| overall precision | 0.616 | 0.750 | +0.134 | +21.7% ✅ |
| overall recall | 0.418 | 0.511 | +0.092 | +22.1% ✅ |
| turns | 114 | 147 | +33.593 | +29.6% ⚠️ |
| tool calls | 113 | 146 | +33.607 | +29.9% ⚠️ |
| duration (s) | 370 | 483 | +113 | +30.4% ⚠️ |
| cache-read tokens | 7,403,724 | 7,908,014 | +504,290 | +6.8% ⚠️ |
| output tokens | 23,840 | 30,230 | +6,390 | +26.8% ⚠️ |

## Latency decomposition

`model time` + `tool time` + `other` = `duration (s)`. **model time** is request round-trip (prefill + decode); **tool time** is Playwright/MCP execution; **other** is rate-limit backoff, retries and harness overhead — noise, not a latency signal, and the floor a latency claim must clear. `output tokens per turn` vs `turns` separates *fewer steps* from *less said per step*; only the product of the two moves wall clock, because `effective output tok/s` is near-fixed for a given model.

Timing available for **140/140** baseline and **140/140** hints app-runs (needs the Claude-Code transcript or a cached `timing.json`).

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| model time (s) | 258 | 326 | +67.980 | +26.4% ⚠️ |
| tool time (s) | 28.408 | 46.710 | +18.302 | +64.4% ⚠️ |
| other / overhead (s) | 84.418 | 111 | +26.264 | +31.1% ⚠️ |
| model s per turn | 2.315 | 2.245 | -0.069 | -3.0% ✅ |
| tool s per call | 0.260 | 0.327 | +0.067 | +25.7% ⚠️ |
| output tokens per turn | 216 | 210 | -6.051 | -2.8% ✅ |
| effective output tok/s | 93.403 | 93.744 | +0.342 | +0.4% ✅ |

## Dispersion across reps

Spread of the critical loop/latency/token metrics across reps per condition. **median** = typical value (robust to a runaway rep), **CV** = std/mean spread (lower ⇒ steadier), **p95** = worst-case tail. All lower-is-better. ⚠️ CV pools 14 apps, so it mixes between-app and between-rep variance — read per-app for a clean stability signal.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| turns — median | 117 | 146 | +29.000 | +24.8% ⚠️ |
| turns — CV | 0.239 | 0.223 | -0.016 | -6.8% ✅ |
| turns — p95 | 164 | 200 | +36.050 | +22.0% ⚠️ |
| tool calls — median | 116 | 145 | +29.000 | +25.0% ⚠️ |
| tool calls — CV | 0.241 | 0.224 | -0.017 | -7.0% ✅ |
| tool calls — p95 | 163 | 199 | +36.050 | +22.1% ⚠️ |
| duration (s) — median | 371 | 490 | +119 | +32.0% ⚠️ |
| duration (s) — CV | 0.180 | 0.189 | +0.009 | +4.9% ⚠️ |
| duration (s) — p95 | 484 | 614 | +130 | +26.9% ⚠️ |
| model time (s) — median | 258 | 333 | +74.950 | +29.0% ⚠️ |
| model time (s) — CV | 0.202 | 0.199 | -0.003 | -1.4% ✅ |
| model time (s) — p95 | 342 | 427 | +85.680 | +25.1% ⚠️ |
| output tokens — median | 23,460 | 30,734 | +7,274 | +31.0% ⚠️ |
| output tokens — CV | 0.171 | 0.169 | -0.002 | -1.1% ✅ |
| output tokens — p95 | 30,781 | 37,677 | +6,896 | +22.4% ⚠️ |

_Source: `summary/metrics.csv`._
