## Final result and decision

Installed candidate v3 as `evidence-first`. Across the final eight runs (four baseline, four candidate), total provider tokens fell from **301,276 to 250,292: 16.92% less**. Merge saved 10.94%; retry saved 22.88%. All eight runs passed supplied checks and all 58 additional acceptance checks per arm. Three of four candidate runs saved tokens; one merge run used more. Command-execution calls fell from 12 to 9. The skill text installed is identical to the v3 text tested.

This is evidence of savings on these focused known-path fixes, not a guarantee across repositories. The candidate was selected adaptively after two unsuccessful designs. Those used 4.90% and 0.24% more tokens in their respective comparisons. All 32 coding runs across all three experiments passed the quality gate: 452 repeated additional acceptance-check executions, drawn from 55 distinct checks across four task contracts. Repetitions are not independent new quality cases.

**The savings were primarily cached input.** In the final comparison, cached input fell from 256,384 to 205,056; fresh input rose slightly from 40,052 to 40,235; output rose from 4,840 to 5,001. Therefore this study proves neither a 16.92% bill reduction nor lower fresh-input usage. Summed run time was 197.13 seconds baseline versus 195.53 seconds candidate, too close and uncontrolled to claim a speed improvement. The measured benefit is fewer total provider tokens processed on these tasks.

The 32 benchmark runs themselves consumed **2,378,416 total provider tokens**, including cached input, excluding the setup pilot and this research conversation. This was an evaluation investment, not a claim that the research session saved tokens overall. Full counters, negative results and per-run artifacts remain available.

The final installed skill is intentionally small and instruction-only. It does not bundle or call the experimental capture helper. It combines initial evidence reads when paths are known, preserves further investigation when needed, and requires complete appropriate verification. It is available for relevant coding work; installation does not force every future session to follow it or preload it on ordinary chat.


# Measured results

Positive savings means fewer total provider tokens; negative means a regression. Each task has two runs per arm.

| Phase / task | Baseline total | Candidate total | Savings | Accepted B/C |
|---|---:|---:|---:|---|
| bench / control | 143,763 | 146,827 | -2.13% | 2/2 |
| bench / merge | 150,192 | 172,532 | -14.87% | 2/2 |
| bench / ndjson | 153,264 | 154,552 | -0.84% | 2/2 |
| bench / retry | 150,664 | 153,274 | -1.73% | 2/2 |
| bench / ALL | 597,883 | 627,185 | -4.90% | 8/8 |
| bench-v2 / merge | 149,103 | 150,506 | -0.94% | 2/2 |
| bench-v2 / retry | 151,420 | 150,751 | 0.44% | 2/2 |
| bench-v2 / ALL | 300,523 | 301,257 | -0.24% | 4/4 |
| bench-v3 / merge | 150,304 | 133,866 | 10.94% | 2/2 |
| bench-v3 / retry | 150,972 | 116,426 | 22.88% | 2/2 |
| bench-v3 / ALL | 301,276 | 250,292 | 16.92% | 4/4 |

All counters include instruction overhead and cached input. No dollar-cost or statistical-significance claim. Full per-run counters: runs.csv; machine-readable aggregate: analysis.json.

Original experiment artifacts are retained privately and are not bundled here. Sources and pinned citations are in research.md.

Sharing-copy note: raw counters, fixtures, logs and runners referenced in this report are retained privately, not bundled in this package. This copy preserves reported results and limitations; it is not a complete reproduction kit.
