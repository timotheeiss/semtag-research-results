# Injection cost — `2026-09-05_sonnet`

One-off cost of injecting `data-semtag-*` hints, measured from the Agent
SDK's `ResultMessage` — the same fields the testing agent reports, so the
two are directly comparable.

14/14 app(s) are valid for aggregation (skill invoked and no injection error).

## Per app

| app | valid | duration (s) | turns | tools | cost (USD) | in | cache-create | cache-read | out | elements | errors |
|---|:---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| WebTestBench_0001 | yes | 558.60 | 64 | 62 | 5.07 | 126 | 188,839 | 5,529,257 | 45,116 | 103 | 0 |
| WebTestBench_0003 | yes | 816.40 | 90 | 87 | 6.28 | 124 | 243,051 | 4,964,844 | 67,007 | 75 | 0 |
| WebTestBench_0004 | yes | 893.80 | 84 | 81 | 6.35 | 122 | 218,297 | 5,620,064 | 68,405 | 76 | 0 |
| WebTestBench_0005 | yes | 764.60 | 98 | 96 | 5.89 | 124 | 134,491 | 6,701,884 | 67,885 | 131 | 0 |
| WebTestBench_0006 | yes | 833.60 | 105 | 102 | 8.25 | 174 | 198,681 | 9,665,653 | 70,356 | 101 | 0 |
| WebTestBench_0010 | yes | 601.00 | 76 | 74 | 5.62 | 114 | 200,093 | 5,729,832 | 60,123 | 79 | 0 |
| WebTestBench_0019 | yes | 596.60 | 71 | 69 | 4.70 | 100 | 121,761 | 4,940,573 | 58,665 | 76 | 0 |
| WebTestBench_0023 | yes | 1,037 | 110 | 107 | 8.01 | 156 | 231,348 | 7,760,105 | 87,490 | 118 | 0 |
| WebTestBench_0045 | yes | 399.10 | 59 | 57 | 2.85 | 96 | 79,736 | 3,200,207 | 29,919 | 32 | 0 |
| WebTestBench_0054 | yes | 665.90 | 74 | 72 | 4.67 | 122 | 110,646 | 5,484,622 | 49,559 | 38 | 0 |
| WebTestBench_0063 | yes | 534.20 | 69 | 67 | 4.48 | 108 | 114,241 | 5,236,727 | 45,868 | 44 | 0 |
| WebTestBench_0074 | yes | 1,060 | 77 | 92 | 6.52 | 124 | 200,613 | 6,137,529 | 63,798 | 36 | 0 |
| WebTestBench_0089 | yes | 651.30 | 77 | 75 | 5.49 | 118 | 199,079 | 5,855,192 | 52,777 | 52 | 0 |
| WebTestBench_0097 | yes | 654.10 | 83 | 81 | 6.38 | 164 | 162,420 | 8,141,777 | 51,655 | 42 | 0 |

## Across valid apps

| metric | median | mean | min | max |
|---|---:|---:|---:|---:|
| duration (s) | 660.00 | 719.04 | 399.10 | 1,060 |
| turns | 77.00 | 81.21 | 59 | 110 |
| tool calls | 78.00 | 80.14 | 57 | 107 |
| cost (USD) | 5.75 | 5.75 | 2.85 | 8.25 |
| input tokens | 123.00 | 126.57 | 96 | 174 |
| cache-creation tokens | 193,760 | 171,664 | 79,736 | 243,051 |
| cache-read tokens | 5,674,948 | 6,069,162 | 3,200,207 | 9,665,653 |
| output tokens | 59,394 | 58,473 | 29,919 | 87,490 |
| hinted elements | 75.50 | 71.64 | 32 | 131 |
| validator errors | 0.0000 | 0.0000 | 0 | 0 |

Normalised by output, since injection cost scales with app size:

- cost per hinted element: median **$0.0837**, range $0.0450–$0.1810

_Source: `summary/injection.csv` (raw rows followed by Google Sheets formula statistics). Testing-agent cost remains separate._
