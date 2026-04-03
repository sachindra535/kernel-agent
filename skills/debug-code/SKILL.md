---
name: debug-code
description: "Runs incident-grade Linux kernel debugging with Failure Radar triage, hypothesis testing, and Patch Safety Ladder validation from first signal to verified fix."
license: MIT
allowed-tools: ReadFile rg Shell
metadata:
  author: kernel-mentor-agent
  version: "1.0.0"
  category: kernel-debugging
---

# Debug Code

## When to Use
Use this skill for kernel panics, oops traces, module load failures, syscall regressions, race conditions, memory corruption suspicion, or unexplained performance drops.

## Workflow
1. Define the failure precisely: symptom, trigger, frequency, blast radius, and business impact.
2. Run Kernel Failure Radar triage:
   - map symptom to likely subsystem(s)
   - identify highest-information first probe
   - list top 3 hypotheses with confidence percentages
3. Capture environment: kernel version, config deltas, architecture, compiler, boot params, and runtime context.
4. Reproduce deterministically with the smallest scenario and a stop condition.
5. Collect evidence in this order:
   - `dmesg -T`
   - `journalctl -k -b`
   - tracepoint/ftrace/perf data
   - syscall/process traces when relevant
6. Build a ranked hypothesis list; test one variable at a time with pass/fail criteria.
7. Isolate likely subsystem/commit/patch via bisect-style narrowing.
8. Propose fix with Patch Safety Ladder level:
   - L1 for repro-only changes
   - L2 for minimal patch + targeted checks
   - L3 for regression/perf validation
9. Validate with targeted tests, regression tests, and rollback readiness.

## Required Diagnostics Checklist
- Exact error output or stack trace
- Repro command/script
- Expected vs actual behavior
- Kernel + toolchain versions
- Any recent config or code changes
- Last known good version/commit if available
- Safety constraints (prod machine, root limits, downtime tolerance)

## Advanced Tactics
- Use `git bisect` when regression window exists.
- Use lockup tooling (`echo w > /proc/sysrq-trigger`, watchdog logs) for deadlock suspicion.
- Use `kmemleak`, KASAN/KCSAN/KFENCE signals when memory/race issues are suspected.
- For perf regressions, compare before/after with identical workload and pinned CPU governor.

## Output Format
```markdown
## Debug Snapshot
- Symptom:
- Repro:
- Environment:
- Failure Radar Classification:
- Patch Safety Ladder Target:

## Likely Causes (ranked)
1. [Cause + evidence]
2. [Cause + evidence]

## Next Commands
```bash
[Command 1]
[Command 2]
```

## Decision Gate
- If [result], then [next action]
- If [result], then [alternate action]

## Fix Candidate
[Proposed change and why]

## Validation Plan
- [Targeted test]
- [Regression check]
- [Rollback verification]
```
