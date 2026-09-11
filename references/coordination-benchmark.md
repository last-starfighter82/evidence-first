# Context Relay and Evidence First: controlled delegation benchmark

Context Relay is an anonymized label for a separate experimental coordination skill, not a bundled dependency. Raw experiment files mentioned below are not included in this sharing copy.
Same model and reasoning setting, two tasks, two repeats per arm. Totals include both coordinator and coding session, skill instructions and cached input. Positive savings means fewer tokens.

| Group | Sender tokens | Receiver tokens | Combined tokens | Change vs baseline | Accepted |
|---|---:|---:|---:|---:|---|
| baseline | 71,623 | 267,890 | 339,513 | 0.00% savings | 4/4 |
| context | 73,167 | 304,126 | 377,293 | -11.13% savings | 4/4 |
| evidence | 71,410 | 233,613 | 305,023 | 10.16% savings | 4/4 |
| combined | 73,192 | 253,348 | 326,540 | 3.82% savings | 4/4 |

## Per-task totals

| Task | Group | Total tokens | Savings | Median per trial |
|---|---|---:|---:|---:|
| merge | baseline | 187,631 | 0.00% | 93,816 |
| merge | context | 188,623 | -0.53% | 94,312 |
| merge | evidence | 151,796 | 19.10% | 75,898 |
| merge | combined | 172,564 | 8.03% | 86,282 |
| retry | baseline | 151,882 | 0.00% | 75,941 |
| retry | context | 188,670 | -24.22% | 94,335 |
| retry | evidence | 153,227 | -0.89% | 76,614 |
| retry | combined | 153,976 | -1.38% | 76,988 |

## Token accounting

| Group | Cached input | Fresh input | Output | Handoff characters |
|---|---:|---:|---:|---:|
| baseline | 285,440 | 48,167 | 5,906 | 4,375 |
| context | 312,320 | 59,220 | 5,753 | 3,218 |
| evidence | 236,288 | 63,160 | 5,575 | 4,331 |
| combined | 260,992 | 59,872 | 5,676 | 3,283 |

## Findings and review

Evidence First alone had the lowest observed aggregate total in this experiment. Context Relay alone produced shorter messages but did not save total tokens. The combined setup used 21,517 more tokens than Evidence First alone (7.05% more). This is an observation from a small benchmark, not a population ranking or proof that the combination always costs more.

Context Relay handoffs totaled 3,218 characters versus baseline 4,375 (26.45% shorter). Despite that, sender usage increased by 1,544 tokens and receiver usage increased by 36,236. Most of the observed regression was in receiver usage, not merely the size of the skill instructions. Do not claim that shortening the skill text alone would recover the entire difference.

Fresh input increased in every skill group compared with baseline; aggregate savings were in total tokens including cached input. Provider cache state was not controlled. No dollar savings or speed benefit is established.

All 16 trials (32 model sessions) passed public checks and 232 repeated additional acceptance checks. There are 29 distinct additional checks across two task contracts; repetitions do not expand functional coverage. All supplied tests stayed unchanged. Manual review of all 16 handoffs found the explicit merge/retry requirements retained and standing policy referenced. Source review grouped implementations by normalized AST, excluding docstrings: three variants, all using standard-library copying or bounded exception-selective retry. This was a non-blinded review by the experiment author, not an external code audit. Arbitrary cyclic graphs/custom object semantics and unrelated production behavior are outside the fixture coverage.

No skill was tuned during the experiment and no unfavorable trial was replaced. The installed skill bodies still match the frozen copies. No real roster agents received messages, no application deployment occurred, and no long-running session was reset or compacted. The next separate investigation would need to test long-session context management; these fresh-session handoffs do not measure that feature.

## Scope and reproduction

Context Relay applies to the coordinator; Evidence First applies to the coding recipient. In the combined arm each role receives its relevant skill. Neither skill is active in baseline. Normal user configuration and project instruction loading are disabled in ephemeral CLI sessions. A common fixture policy applies to every arm. Skills were frozen before the experiment; no tuning or selective reruns. Jobs used a fixed shuffled order, two concurrent pipelines, gpt-6-astra with medium reasoning.

These are synthetic merge/retry fixes, not a production application workload. A new coordinator and receiver start each trial. This does not measure long-session context compaction, GUI-vs-terminal provider parity, real VPS transport or dollar savings. Two repeats per task cannot establish statistical significance or universal quality preservation. A smaller message is not itself proof of total savings.

Acceptance requires supplied tests plus additional independently executed grader checks, unchanged supplied tests, and successful sessions with usage. The grader is reused from the earlier experiment and lives outside the coding workspace. Code review and message-contract review are recorded separately.

Reproduction: run.py creates fresh output folders and refuses overwrites. It needs an authenticated local Codex CLI, Python, model access and an isolated CODEX_HOME. Runtime credentials are outside this deliverable and must not be published. All 16 trial artifacts, both role prompts/events, handoffs, output code and grading results are under runs/. The experiment consumes provider tokens; counters exclude this research conversation.

Total experiment tokens: 1,348,369.

Original experiment artifacts are retained privately and are not bundled here.
