# Evidence First

**Give your coding agent the evidence it needs in fewer steps.**

Evidence First is a small instruction-only AI coding agent skill for focused code fixes. When a task names source files, it tells the agent to read those files, nearby tests and project instructions together, reuse what it already knows, and run the complete appropriate checks.

No extra service, model download or API key required by the skill. Your coding agent still needs its normal setup.

## See the difference

Suppose you ask: "Fix retries in `retry.py` and preserve the existing error handling."

| Separate discovery steps | With Evidence First |
|---|---|
| List the directory before opening the named file | Read the named file, nearby tests and instructions together |
| Repeat unchanged reads | Reuse evidence already collected |
| Investigate, implement and verify | Investigate, implement and verify |

This illustrates the intended workflow. It is not a measured trace or a promise that every task needs fewer calls. When paths or requirements are unclear, the agent still investigates.

## Use it

Install this folder through your agent's local skill installation process. Keep `SKILL.md`, `agents/` and `references/` together under the folder name `evidence-first`.

Then ask:

```text
Use evidence-first to fix the retry behavior in retry.py.
Preserve error handling and run the relevant tests.
```

You can also ask an agent with local file access to read `SKILL.md` and apply it to a specific task. Installation and automatic discovery depend on your agent.

## Compatibility

Tested with Codex. The instructions can also be used in Claude Code and other agents that support skills, but token savings have not been verified on those agents.

This is an instruction-only package, not a provider integration. Follow your agent's own skill installation process; compatibility does not imply identical behavior or measured savings.

## How it works

1. Read known source files, nearby tests and project instructions together.
2. Expand the investigation wherever evidence is missing.
3. Reuse unchanged evidence and existing code.
4. Preserve requirements, readable implementation, security checks and error handling.
5. Run complete appropriate checks and report the result concisely.

The skill does not switch models, rewrite memory, compress requirements or skip validation. It does not require Caveman or another token-saving skill.

## Measured results

The final development comparison used two small Python task contracts, two repeats per task and configuration, and the same model and reasoning setting.

| Final comparison | Baseline | Evidence First |
|---|---:|---:|
| Total provider tokens | 301,276 | 250,292 |
| Accepted runs | 4 of 4 | 4 of 4 |
| Additional acceptance-check executions | 58 | 58 |

**16.92% fewer total tokens in this comparison.** Three of four skill-assisted runs used fewer tokens; one used more. Both configurations passed the evaluated checks.

Most savings were cached input. Fresh input and output rose slightly, so this does not establish equivalent bill savings or faster completion. The final candidate was selected after two earlier versions used 4.90% and 0.24% more tokens. Small synthetic tasks and adaptive selection limit what these results establish about other work.

Read the [benchmark summary](references/benchmark.md) for full accounting and negative results. The [research notes](references/research.md) explain the design and link to inspected public sources.

## When to use it

Best suited to focused fixes where source paths are already known. Savings may be small or absent when the agent already batches its reads. Unknown repositories and larger changes still need discovery and their own verification.

Combining skills does not make their savings additive. See the [coordination comparison](references/coordination-benchmark.md) and [later combined comparison](references/latest-combined-benchmark.md) for different outcomes and their limitations.

## What is included

- `SKILL.md`: the instructions used by the agent.
- `agents/openai.yaml`: display and invocation metadata.
- `references/`: research and benchmark summaries.

Raw trial logs, counters, fixtures and benchmark runners are not bundled. These summaries are not a complete reproduction kit. The skill has no executable helper, telemetry service or credential store.

## License

[MIT](LICENSE). Third-party projects linked in the research retain their own licenses.
