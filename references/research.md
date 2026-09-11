# Token efficiency without lowering the coding standard

Research and local coding experiments. Prepared September 2026.

## Final result and decision

Installed candidate v3 as `evidence-first`. Across the final eight runs (four baseline, four candidate), total provider tokens fell from **301,276 to 250,292: 16.92% less**. Merge saved 10.94%; retry saved 22.88%. All eight runs passed supplied checks and all 58 additional acceptance checks per arm. Three of four candidate runs saved tokens; one merge run used more. Command-execution calls fell from 12 to 9. The skill text installed is identical to the v3 text tested.

This is evidence of savings on these focused known-path fixes, not a guarantee across repositories. The candidate was selected adaptively after two unsuccessful designs. Those used 4.90% and 0.24% more tokens in their respective comparisons. All 32 coding runs across all three experiments passed the quality gate: 452 repeated additional acceptance-check executions, drawn from 55 distinct checks across four task contracts. Repetitions are not independent new quality cases.

**The savings were primarily cached input.** In the final comparison, cached input fell from 256,384 to 205,056; fresh input rose slightly from 40,052 to 40,235; output rose from 4,840 to 5,001. Therefore this study proves neither a 16.92% bill reduction nor lower fresh-input usage. Summed run time was 197.13 seconds baseline versus 195.53 seconds candidate, too close and uncontrolled to claim a speed improvement. The measured benefit is fewer total provider tokens processed on these tasks.

The 32 benchmark runs themselves consumed **2,378,416 total provider tokens**, including cached input, excluding the setup pilot and this research conversation. This was an evaluation investment, not a claim that the research session saved tokens overall. Full counters, negative results and per-run artifacts remain available.

The final installed skill is intentionally small and instruction-only. It does not bundle or call the experimental capture helper. It combines initial evidence reads when paths are known, preserves further investigation when needed, and requires complete appropriate verification. It is available for relevant coding work; installation does not force every future session to follow it or preload it on ordinary chat.

## What this investigation measures

The question is whether a coding skill reduces the tokens needed to complete the same task while retaining the required behavior and verification. Shorter replies, smaller terminal output, lower dollar cost, and smaller code diffs are related measurements, but none alone answers that question. This investigation uses complete provider-reported input plus output tokens as its primary outcome and executable acceptance checks as its quality gate. Cached input remains part of input; it is reported separately, not subtracted to make savings look larger.

The evidence consists of source inspection of seven public projects, including several individual skills and compression mechanisms, primary documentation, and local coding experiments. Publisher benchmarks are identified as publisher results. They were not reproduced wholesale. Local tests use one model and small synthetic Python tasks; they cannot establish that no quality regression will occur on arbitrary production work.

## What the source code actually does

### Caveman: output discipline, not automatic task-wide savings

The inspected revision of Caveman changes language style and includes protection for technical details, numbers and negations. Its benchmark compares ordinary, generically terse and Caveman responses, measuring output tokens. That is useful for answering whether a response is shorter. It does not establish a reduction in all tokens consumed by a coding agent reading files, editing and testing. The stats skill labels its figures as estimates, including assumed instruction overhead. Treating those estimates as measured account savings would be a mistake. [Pinned Caveman benchmark](https://github.com/JuliusBrussee/caveman/blob/15581d14007fd01fb3f132016741962f34936ca2/benchmarks/run.py), [stats skill](https://github.com/JuliusBrussee/caveman/blob/15581d14007fd01fb3f132016741962f34936ca2/plugins/caveman/skills/caveman-stats/SKILL.md).

Its memory-compression companion uses another model call and structural validation. Preserving code blocks, paths and URLs helps, but is not a semantic proof that every constraint survived compression. That extra model call also belongs in the cost of the intervention. I did not adopt automatic memory or requirement rewriting. [Compression implementation](https://github.com/JuliusBrussee/caveman/blob/15581d14007fd01fb3f132016741962f34936ca2/plugins/caveman/skills/caveman-compress/scripts/compress.py), [validator](https://github.com/JuliusBrussee/caveman/blob/15581d14007fd01fb3f132016741962f34936ca2/plugins/caveman/skills/caveman-compress/scripts/validate.py).

### Ponytail: fewer unnecessary engineering steps

Ponytail prefers existing functions, standard libraries and native features over new abstractions. This can reduce both implementation work and agent interaction. Its stronger modes also contain shortcuts that would be inappropriate if they remove requested scope. The useful part for this task is reuse and complete, maintainable fixes, with requirements kept intact. [Pinned skill](https://github.com/DietrichGebert/ponytail/blob/356918eba965ee1eac64bd3a7f0dd02108350de5/skills/ponytail/SKILL.md).

The publisher's corrected agentic study is unusually relevant: twelve tasks, four repeats, Claude Code with Haiku 4.5, and a realistic terse comparison. It reports approximately 22% fewer tokens for Ponytail and 7% more for Caveman. It also explains why an earlier chatty baseline exaggerated gains. Those are results on that harness, not a prediction for other agents. Some scoring uses a model judge, which cannot substitute for complete functional checks. [Publisher results](https://github.com/DietrichGebert/ponytail/blob/356918eba965ee1eac64bd3a7f0dd02108350de5/benchmarks/results/2026-06-18-agentic.md), [judge implementation](https://github.com/DietrichGebert/ponytail/blob/356918eba965ee1eac64bd3a7f0dd02108350de5/benchmarks/agentic/judge.py).

### Ponycave: combining instructions does not add their percentages

Ponycave combines brevity and implementation simplicity. Its instructions include safety constraints, but a combined prompt is still an intervention whose overhead and behavior require testing. I found no basis in the inspected files to add Caveman and Ponytail percentage claims together. Stacking several overlapping skills may increase context without changing the agent's behavior. [Pinned project](https://github.com/ramaikn/ponycave/tree/b644f8b7ea45337f0c5a9c9010cdfb569022808a).

### Context Mode: keep large tool results outside the conversation

Context Mode captures and indexes output, then returns selected evidence. The server tracks bytes returned and avoided; its own design record distinguishes different compression-accounting formulas. These are meaningful measures of tool-output volume. They are not automatically provider-token savings across a full task: tool schemas, indexing/retrieval interactions, missed evidence and extra rounds also matter. I adopted the principle of retrieving relevant evidence, without adding a new server dependency for this small skill. [Server implementation](https://github.com/mksglu/context-mode/blob/8d9d546a78e8791fd27b54fb46dc0018fce1a860/src/server.ts), [accounting design record](https://github.com/mksglu/context-mode/blob/8d9d546a78e8791fd27b54fb46dc0018fce1a860/docs/adr/0004-stats-strict-compression-formula.md).

### RTK: command-specific filtering

RTK transforms command output and records before/after token estimates. Its filtering implementation contains different levels and truncation behavior; its tracking documentation explains command-level savings. That is a useful engineering mechanism, but evidence removed by a filter can matter later. A large reduction in one command's output must not be presented as the same percentage reduction in a complete coding task. I retained conservative output handling and rejected arbitrary truncation as proof of passing tests. [Filtering code](https://github.com/rtk-ai/rtk/blob/79347d5e0e20a61002b8dffe190d377846cb1e59/src/core/filter.rs), [tracking documentation](https://github.com/rtk-ai/rtk/blob/79347d5e0e20a61002b8dffe190d377846cb1e59/docs/usage/TRACKING.md).

### Repowise: useful structure, with limits that need inspection

Repowise's test-output filter recognizes test frameworks and extracts useful summaries. The implementation also caps failure blocks and omits some warning-summary content. Therefore, a broad claim that all failure information survives needs qualification. A retained raw log and a way to retrieve omitted evidence are important. Its published agent benchmark measures repository questions and tool adoption; that differs from proving that code changes retain behavior. [Test filter](https://github.com/repowise-dev/repowise/blob/1599224a3e4590c5d15a3d8e53727b9e74116ab9/packages/core/src/repowise/core/distill/filters/test_output.py), [benchmark documentation](https://github.com/repowise-dev/repowise/blob/1599224a3e4590c5d15a3d8e53727b9e74116ab9/docs/BENCHMARKS.md).

### LLMLingua: learned compression is a different tradeoff

LLMLingua-2 uses a learned token-selection mechanism. The implementation supports forced token preservation and digit handling; the research evaluates compression on language tasks. This is credible research into lossy prompt compression, not a guarantee that deleting tokens from software requirements or source will preserve correctness. It also adds a model and runtime to maintain. I did not use it for code or instructions where a removed exception could change the task. [Original paper](https://arxiv.org/abs/2403.12968), [pinned compressor](https://github.com/microsoft/LLMLingua/blob/5a4c78ae18ab17a98cf997e8259354e546081d64/llmlingua/prompt_compressor.py).

## The design derived from this research

The candidate, `evidence-first`, concentrates on avoiding unnecessary context: targeted reads, reuse of unchanged evidence, and concise successful test output. It explicitly preserves requirements, validation, security, error paths and appropriate tests. It does not switch to a weaker model, shorten necessary implementation code, spawn agents, or silently compress memory.

This approach is consistent with Anthropic's guidance on just-in-time retrieval and supplying the smallest sufficient set of high-signal information. That guidance is a design rationale, not evidence that this particular skill saves a fixed percentage. [Anthropic context-engineering guidance](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents).

The first candidate used a small standard-library capture helper. It stores exact raw command output, preserves exit status, and only suppresses recognized routine passing unittest lines after success. Failures, unknown output formats and other lines pass through. It does not invoke another model. Tests explicitly exercise warnings, skips, unknown text, nonzero exits and raw-log preservation.

The revised candidate prefers the test framework's own quiet mode when available. For unittest, `-q` changes reporting while retaining discovery and test selection. The raw-log helper remains an optional choice when individual passing-test records must be retained. Quiet mode is not appropriate if a task specifically requires verbose per-case evidence; the instructions preserve that distinction.

## Experiment design and safeguards

The initial experiment fixes four Python tasks before running either arm: recursive configuration merge, bounded exception retry, incremental UTF-8 JSON-line decoding, and a tiny clamp function as an overhead control. Each arm receives a fresh copy, the same task prompt, the same development checks and the same model (`gpt-6-astra`, medium reasoning). The baseline is explicitly asked to work efficiently and report concisely; it is not instructed to waste tokens.

There are two repeats per arm per task, sixteen runs. Jobs have a fixed shuffled order. Sessions ignore user configuration and project-document loading, and use ephemeral conversations. The candidate's instruction text is included in its provider usage. The CLI still has its normal system/tool overhead; reported totals include that overhead. The harness allows two concurrent jobs, which can affect latency and caching. It does not control random generation seeds or provider cache state.

The acceptance grader lives outside each coding workspace and checks additional behavior after the agent finishes. The common prompt prohibits changing supplied tests and accessing parent directories. The harness verifies supplied tests stayed unchanged, runs them independently, and runs the additional grader. This is procedural separation, not a security sandbox. The grader was written for this experiment, not independently authored by a third party.

Before interpreting results, the grader was tested against deliberately broken starting implementations and correct reference implementations. The originals passed only 2/15 merge, 1/14 retry, 2/15 decoder and 8/11 clamp checks; the references passed all checks. This demonstrates sensitivity to the seeded defects, not exhaustive coverage of all possible defects.

After observing overhead in the first candidate, a separate follow-up was declared and frozen. It uses the shorter native-quiet candidate on merge with 1,200 parameter cases, and the original retry suite as a lower-output comparison, with two repeats per arm. The larger merge suite deliberately tests a high-output condition. It is not a representative sample of production repositories, and the follow-up is adaptive research, not an independent preregistered replication. Original results are retained.

A third candidate was then frozen separately. It makes a more specific change: when the task already names source files, read those files, nearby tests and project instructions in the first tool call instead of spending a separate round listing the directory. It retains expansion when information is missing. The third experiment returns to the original small merge and retry fixtures, with fresh baseline runs and two repeats per arm. This tests a narrower, actionable mechanism rather than claiming that terse language alone helps. All three candidates and all results remain available. Selection after experiments can overestimate future performance; these results need replication on real tasks before broader rollout.

## How to interpret the measurements

For each completed run, total tokens equal provider `input_tokens + output_tokens`. Cached input is included in input; fresh input is input minus cached input. Reasoning output is reported separately and not added a second time to output. Per-task comparisons use equal numbers of runs. Aggregate percentage change is calculated from summed tokens, not an unweighted average of percentages.

Cache hits can reduce latency or charges without reducing the number of tokens processed. This study does not convert tokens into dollars or assume a particular billing plan. Actual cost would require the model's applicable pricing and complete cache accounting. [OpenAI prompt-caching documentation](https://developers.openai.com/api/docs/guides/prompt-caching).

A candidate is not called successful solely because it passes a build or produces a smaller diff. The quality gate requires successful completion, unchanged supplied tests, all public checks and all additional acceptance checks. Manual source review looks for obvious regressions and unnecessary machinery. These are useful bounded checks; they cannot prove universal non-regression, security or maintainability.

## Evidence and reproduction

`bench/` contains the initial fixtures, runner, preregistered manifest, independent grader and per-run events. `bench-v2/` and `bench-v3/` contain the separately declared follow-ups. Candidate source is retained in `candidate/`, `candidate-v2/` and `candidate-v3/`. Source snapshots and commit metadata are in `sources/`. Machine-readable results and analysis accompany this report when the runs finish.

Reproduction requires an authenticated local Codex CLI, Python, the stated model and fresh output directories. Each run consumes provider tokens. Do not reuse result folders: the runner refuses to overwrite them. The local runtime-authentication directory is deliberately excluded from deliverables and should never be published. A new environment must supply its own authentication. Local elapsed time and cache-hit counts are observations, not portable performance guarantees.

The practical standard is task-dependent: use a small skill when it removes more work than it adds, preserve evidence needed to make decisions, and measure against an already competent baseline. Do not preload a library of token-saving instructions on every tiny request. Do not advertise output compression as total savings.

## Code-quality review scope

The completed implementations inspected across all final runs use standard-library `deepcopy` for ownership isolation, bounded retry loops with selected-exception handling, `codecs` incremental UTF-8 decoding, and ordinary bound checks. They retain descriptive names and small functions. No extra dependencies are needed. Supplied tests are checked for byte-for-byte preservation by the runner. This is a source-level sanity review in addition to executable grading, not a blinded external review.

The fixtures do not establish behavior for unspecified cases such as cyclic configuration graphs, arbitrary custom numeric objects, resource exhaustion, malicious repository contents or a production authentication boundary. They do test the explicit edge cases in the task contracts, including mutable aliasing, incorrect retry counts, exception identity, split Unicode and invalid/truncated input. Larger production changes still require their own relevant checks. A token-saving instruction cannot certify those changes in advance.

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

Sharing-copy note: raw counters, fixtures, logs and runners referenced in this report are retained privately, not bundled in this package. This copy preserves reported results and limitations; it is not a complete reproduction kit.
