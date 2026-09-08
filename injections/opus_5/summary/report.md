# Injection cost — `2026-09-05_opus`

One-off cost of injecting `data-semtag-*` hints, measured from the Agent
SDK's `ResultMessage` — the same fields the testing agent reports, so the
two are directly comparable.

14/14 app(s) are valid for aggregation (skill invoked and no injection error).

## Per app

| app | valid | duration (s) | turns | tools | cost (USD) | in | cache-create | cache-read | out | elements | errors |
|---|:---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| WebTestBench_0001 | yes | 870.90 | 95 | 93 | 6.64 | 162 | 129,449 | 8,387,781 | 65,450 | 103 | 0 |
| WebTestBench_0003 | yes | 793.90 | 94 | 92 | 6.00 | 154 | 122,416 | 7,511,516 | 59,283 | 97 | 0 |
| WebTestBench_0004 | yes | 1,314 | 137 | 134 | 10.28 | 252 | 228,826 | 12,593,180 | 86,950 | 93 | 0 |
| WebTestBench_0005 | yes | 1,109 | 127 | 124 | 9.04 | 206 | 180,105 | 11,166,886 | 80,356 | 160 | 0 |
| WebTestBench_0006 | yes | 886.70 | 125 | 122 | 8.25 | 178 | 182,357 | 10,202,515 | 65,450 | 130 | 0 |
| WebTestBench_0010 | yes | 1,238 | 128 | 125 | 8.59 | 198 | 197,908 | 9,830,886 | 81,502 | 118 | 0 |
| WebTestBench_0019 | yes | 847.50 | 119 | 117 | 7.22 | 206 | 121,338 | 9,911,500 | 60,296 | 107 | 0 |
| WebTestBench_0023 | yes | 1,175 | 130 | 127 | 8.76 | 194 | 172,224 | 10,224,299 | 88,018 | 149 | 0 |
| WebTestBench_0045 | yes | 564.70 | 73 | 71 | 3.62 | 116 | 85,744 | 4,271,021 | 37,704 | 37 | 0 |
| WebTestBench_0054 | yes | 609.30 | 82 | 80 | 4.03 | 106 | 99,869 | 4,613,818 | 43,718 | 43 | 0 |
| WebTestBench_0063 | yes | 1,109 | 122 | 120 | 7.42 | 190 | 146,690 | 9,557,941 | 69,093 | 84 | 0 |
| WebTestBench_0074 | yes | 704.40 | 87 | 85 | 5.11 | 142 | 106,703 | 6,244,287 | 52,839 | 49 | 0 |
| WebTestBench_0089 | yes | 729.60 | 94 | 92 | 5.40 | 160 | 98,516 | 6,825,307 | 54,846 | 83 | 0 |
| WebTestBench_0097 | yes | 713.90 | 92 | 90 | 5.74 | 164 | 117,270 | 7,411,363 | 52,148 | 82 | 0 |

## Across valid apps

| metric | median | mean | min | max |
|---|---:|---:|---:|---:|
| duration (s) | 859.20 | 904.76 | 564.70 | 1,314 |
| turns | 107.00 | 107.50 | 73 | 137 |
| tool calls | 105.00 | 105.14 | 71 | 134 |
| cost (USD) | 6.93 | 6.87 | 3.62 | 10.28 |
| input tokens | 171.00 | 173.43 | 106 | 252 |
| cache-creation tokens | 125,932 | 142,101 | 85,744 | 228,826 |
| cache-read tokens | 8,972,861 | 8,482,307 | 4,271,021 | 12,593,180 |
| output tokens | 62,873 | 64,118 | 37,704 | 88,018 |
| hinted elements | 95.00 | 95.36 | 37 | 160 |
| validator errors | 0.0000 | 0.0000 | 0 | 0 |

Normalised by output, since injection cost scales with app size:

- cost per hinted element: median **$0.0688**, range $0.0565–$0.1106

_Source: `summary/injection.csv` (raw rows followed by Google Sheets formula statistics). Testing-agent cost remains separate._
