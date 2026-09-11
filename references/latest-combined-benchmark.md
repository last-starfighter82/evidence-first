# Latest Context Relay alone versus Evidence First combination

Context Relay is an anonymized label for a separate experimental coordination skill, not a bundled dependency. Raw experiment files mentioned below are not included in this sharing copy.
## Result and caveat

The latest Context-only result repeated closely: 463,071 tokens now versus 462,970 previously. The combination used 490,444, which is 27,373 tokens (5.91%) more than Context alone. This is an observed end-to-end result on two reused tasks, not universal proof.

**Experimental caveat:** two combined handoffs unnecessarily told the worker to apply context-relay without supplying its body or verifying recipient availability. In interv-combined-2 the worker searched outside the fixture, listing 52 skill filenames from host directories. The scope exception is retained and flagged; this is not a perfectly isolated comparison. No complete skill body was loaded by that recorded filename-search command, and its output did not contain auth.json. Do not interpret this as a comprehensive security audit.

| Configuration | Sender tokens | Worker tokens | Total tokens | Savings vs fresh baseline | Functional checks |
|---|---:|---:|---:|---:|---:|
| Neither skill | 107,572 | 448,204 | 555,776 | 0.00% | 6/6 trials passed |
| Latest Context Relay | 109,022 | 354,049 | 463,071 | 16.68% | 6/6 trials passed |
| Context + Evidence First | 109,164 | 381,280 | 490,444 | 11.76% | 6/6 trials passed |

Baseline was rerun in this experiment; the percentages should not be compared directly with percentages from earlier baselines. All totals include sender and receiver, skill instructions and cached input. Evidence First was applied only to the coding worker; Context Relay only to the coordinator in the intended configuration. Both skill bodies were frozen and remain unchanged.

## Per-task results

| Task | Baseline | Context | Combined |
|---|---:|---:|---:|
| interv | 277,605 | 221,625 | 227,128 |
| chunks | 278,171 | 241,446 | 263,316 |

## Accounting

| Configuration | Cached input | Fresh input | Output | Median trial total |
|---|---:|---:|---:|---:|
| baseline | 480,256 | 66,589 | 8,931 | 92,609.5 |
| context | 389,376 | 65,761 | 7,934 | 74,048.5 |
| combined | 409,216 | 72,574 | 8,654 | 76,313.5 |

Cached input is a subset of input, not extra usage. Most savings relative to baseline were cached input. Combined also used more fresh input than Context alone. No dollar-cost or speed claim is made.

## Interpretation

Both skills encourage a combined first read of known files; their savings cannot be added. In this batch the combination did not improve observed usage. Its increase was mostly receiver work (27,231 of 27,373 extra tokens), not the coordinator. Two unexpected skill-lookup requests contributed unnecessary work; the evidence does not identify the entire difference as instruction overhead or establish a single causal mechanism. The affected trials remain in the totals, with the limitation visible. No selective reruns or skill tuning were performed.

There is no Evidence-First-only arm in this batch, so it cannot establish whether that skill alone would be best. Context-only repeats the previous total within 0.03%, but only on these same tasks. Generalization to extended sessions, other models, live VPS agents and production projects remains unproven. Before claiming a clean combined effect, prevent unsupplied skill lookup and verify a new comparison respects the fixture boundary.

## Verification and reproduction

18 trials / 36 CLI sessions; three repetitions per task and configuration. All public tests and 270 repeated external functional acceptance checks passed (30 distinct checks across interval union and lazy chunking). Supplied tests stayed unchanged. Every handoff was checked for full requirements; all Context/combined handoffs preserved the task request verbatim. New code variants were reviewed; unchanged normalized implementations reused prior reviews. Functional success does not erase the scope exception above.

Same gpt-6-astra and medium effort, fresh isolated configurations, fixed shuffled order, two concurrent pipelines. Fixtures and graders were reused unchanged from the previously validated confirmation, with reference and mutation checks. Grader, fixture, source and skill hashes were verified. The runner uses instruction-based boundaries with a bypass-enabled CLI, not an OS-enforced sandbox; that limitation allowed the observed external filename search.

preregistered.json records settings and hashes; runs.csv and analysis.json provide counters. runs/ retains each prompt, event log, handoff, solution and test result; reviewed.json records review and the scope exception. run.py refuses to overwrite trials. Authentication is outside these evidence directories and must never be published. No live roster-agent changes, skill-body edits, app updates or deployment were performed.

This experiment consumed 1,509,291 provider tokens, excluding this reporting conversation.
