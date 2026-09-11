---
name: evidence-first
description: Save coding tokens by combining initial evidence reads when the task names source files. Keep full requirements and verification. Use for focused code fixes; not ordinary chat.
---

# Evidence first

When source paths are already given, make the first tool call read those files plus nearby tests and project instructions together. Avoid a separate directory-listing round before reading a known file. Discover unknown paths in that same call when practical. Expand evidence as needed; never guess missing contracts.

Reuse unchanged evidence and existing code. Preserve every requirement, security check and error path. Keep implementations readable. Run the complete appropriate checks; repeat after relevant changes or unresolved failures. Native quiet test output is fine; preserve failures and warnings. Never infer success from truncated output.

Do not compress requirements, change models or skip validation to save tokens. Report changes, checks and limits concisely. Measure complete provider input plus output, including this skill. Savings are workload-dependent; see references/benchmark.md only when evidence is requested.
