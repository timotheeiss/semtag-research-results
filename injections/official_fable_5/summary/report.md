# Injection cost — `official-injections`

One-off cost of injecting `data-semtag-*` hints, measured from the Agent
SDK's `ResultMessage` — the same fields the testing agent reports, so the
two are directly comparable.

14/14 app(s) are valid for aggregation (skill invoked and no injection error).

## Per app

| app | valid | duration (s) | turns | tools | cost (USD) | in | cache-create | cache-read | out | elements | errors |
|---|:---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| WebTestBench_0001 | yes | 420.20 | 54 | 52 | 3.06 | 77 | 141,567 | 2,513,764 | 36,549 | 81 | 0 |
| WebTestBench_0003 | yes | 421.20 | 86 | 84 | 3.09 | 97 | 95,941 | 3,144,011 | 36,788 | 82 | 0 |
| WebTestBench_0004 | yes | 563.20 | 88 | 86 | 3.49 | 97 | 115,672 | 3,347,578 | 43,782 | 67 | 0 |
| WebTestBench_0005 | yes | 580.30 | 106 | 104 | 3.43 | 65 | 111,780 | 2,632,944 | 56,415 | 165 | 0 |
| WebTestBench_0006 | yes | 478.40 | 98 | 96 | 3.80 | 121 | 87,326 | 4,429,714 | 41,606 | 89 | 0 |
| WebTestBench_0010 | yes | 547.50 | 75 | 73 | 2.44 | 66 | 72,600 | 2,073,753 | 37,887 | 90 | 0 |
| WebTestBench_0019 | yes | 438.60 | 105 | 103 | 2.96 | 82 | 88,565 | 2,855,021 | 39,273 | 79 | 0 |
| WebTestBench_0023 | yes | 598.90 | 99 | 97 | 4.23 | 131 | 109,866 | 4,267,050 | 56,184 | 113 | 0 |
| WebTestBench_0045 | yes | 257.80 | 47 | 45 | 1.47 | 53 | 66,009 | 1,206,651 | 18,075 | 29 | 0 |
| WebTestBench_0054 | yes | 335.20 | 56 | 54 | 1.83 | 67 | 54,195 | 1,793,054 | 23,606 | 32 | 0 |
| WebTestBench_0063 | yes | 354.10 | 60 | 58 | 2.15 | 61 | 70,579 | 1,943,012 | 29,625 | 59 | 0 |
| WebTestBench_0074 | yes | 318.70 | 65 | 63 | 1.71 | 52 | 60,322 | 1,357,589 | 26,274 | 45 | 0 |
| WebTestBench_0089 | yes | 299.10 | 61 | 59 | 1.55 | 42 | 55,741 | 1,043,188 | 27,079 | 67 | 0 |
| WebTestBench_0097 | yes | 377.70 | 57 | 55 | 2.73 | 96 | 70,330 | 3,203,238 | 27,623 | 52 | 0 |

## Across valid apps

| metric | median | mean | min | max |
|---|---:|---:|---:|---:|
| duration (s) | 420.70 | 427.92 | 257.80 | 598.90 |
| turns | 70.00 | 75.50 | 47 | 106 |
| tool calls | 68.00 | 73.50 | 45 | 104 |
| cost (USD) | 2.85 | 2.71 | 1.47 | 4.23 |
| input tokens | 72.00 | 79.07 | 42 | 131 |
| cache-creation tokens | 79,963 | 85,750 | 54,195 | 141,567 |
| cache-read tokens | 2,573,354 | 2,557,898 | 1,043,188 | 4,429,714 |
| output tokens | 36,668 | 35,769 | 18,075 | 56,415 |
| hinted elements | 73.00 | 75.00 | 29 | 165 |
| validator errors | 0.0000 | 0.0000 | 0 | 0 |

Normalised by output, since injection cost scales with app size:

- cost per hinted element: median **$0.0377**, range $0.0208–$0.0571

_Source: `summary/injection.csv` (raw rows followed by Google Sheets formula statistics). Testing-agent cost remains separate._
