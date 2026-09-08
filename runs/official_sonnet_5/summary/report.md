# Suite report — `official_sonnet-5`

Mean over all (app, rep) samples per condition. Δ = hints − baseline; %Δ relative to baseline. For efficiency metrics, negative Δ = hints is cheaper (good).

Samples — **baseline**: 140 app-runs, **hints**: 140 app-runs.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| overall F1 | 0.811 | 0.836 | +0.025 | +3.1% ✅ |
| overall precision | 0.875 | 0.903 | +0.028 | +3.2% ✅ |
| overall recall | 0.789 | 0.816 | +0.027 | +3.4% ✅ |
| turns | 172 | 180 | +7.650 | +4.5% ⚠️ |
| tool calls | 170 | 178 | +8.179 | +4.8% ⚠️ |
| duration (s) | 563 | 598 | +34.860 | +6.2% ⚠️ |
| cache-read tokens | 12,170,050 | 10,954,438 | -1,215,612 | -10.0% ✅ |
| output tokens | 32,351 | 35,271 | +2,919 | +9.0% ⚠️ |

## Latency decomposition

`model time` + `tool time` + `other` = `duration (s)`. **model time** is request round-trip (prefill + decode); **tool time** is Playwright/MCP execution; **other** is rate-limit backoff, retries and harness overhead — noise, not a latency signal, and the floor a latency claim must clear. `output tokens per turn` vs `turns` separates *fewer steps* from *less said per step*; only the product of the two moves wall clock, because `effective output tok/s` is near-fixed for a given model.

Timing available for **140/140** baseline and **140/140** hints app-runs (needs the Claude-Code transcript or a cached `timing.json`).

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| model time (s) | 408 | 462 | +53.985 | +13.2% ⚠️ |
| tool time (s) | 51.320 | 66.136 | +14.816 | +28.9% ⚠️ |
| other / overhead (s) | 104 | 69.627 | -33.941 | -32.8% ✅ |
| model s per turn | 2.377 | 2.549 | +0.172 | +7.2% ⚠️ |
| tool s per call | 0.308 | 0.371 | +0.063 | +20.4% ⚠️ |
| output tokens per turn | 190 | 196 | +6.671 | +3.5% ⚠️ |
| effective output tok/s | 79.457 | 77.914 | -1.543 | -1.9% ⚠️ |

## Dispersion across reps

Spread of the critical loop/latency/token metrics across reps per condition. **median** = typical value (robust to a runaway rep), **CV** = std/mean spread (lower ⇒ steadier), **p95** = worst-case tail. All lower-is-better. ⚠️ CV pools 14 apps, so it mixes between-app and between-rep variance — read per-app for a clean stability signal.

| Metric | Baseline | Hints | Δ | %Δ |
|---|--:|--:|--:|--:|
| turns — median | 150 | 160 | +10.500 | +7.0% ⚠️ |
| turns — CV | 0.324 | 0.334 | +0.010 | +3.1% ⚠️ |
| turns — p95 | 280 | 289 | +8.400 | +3.0% ⚠️ |
| tool calls — median | 148 | 160 | +11.500 | +7.8% ⚠️ |
| tool calls — CV | 0.324 | 0.335 | +0.011 | +3.2% ⚠️ |
| tool calls — p95 | 278 | 287 | +8.450 | +3.0% ⚠️ |
| duration (s) — median | 523 | 508 | -15.300 | -2.9% ✅ |
| duration (s) — CV | 0.457 | 0.480 | +0.023 | +5.0% ⚠️ |
| duration (s) — p95 | 1,154 | 1,238 | +83.810 | +7.3% ⚠️ |
| model time (s) — median | 375 | 402 | +26.950 | +7.2% ⚠️ |
| model time (s) — CV | 0.444 | 0.465 | +0.021 | +4.7% ⚠️ |
| model time (s) — p95 | 803 | 928 | +124 | +15.5% ⚠️ |
| output tokens — median | 29,633 | 29,342 | -292 | -1.0% ✅ |
| output tokens — CV | 0.463 | 0.433 | -0.031 | -6.6% ✅ |
| output tokens — p95 | 71,278 | 68,166 | -3,112 | -4.4% ✅ |

_Source: `summary/metrics.csv`._
