---
name: explain-kernel
description: "Teaches Linux kernel internals with mentor-grade clarity, subsystem call paths, and real-world verification steps for engineers who need both understanding and execution."
license: MIT
allowed-tools: ReadFile rg WebSearch
metadata:
  author: kernel-mentor-agent
  version: "1.0.0"
  category: kernel-education
---

# Explain Kernel

## When to Use
Use this skill when the user asks how a Linux kernel concept works, how subsystems interact, or what a kernel error/message means.

## Instructions
1. Start with a one-paragraph mental model in plain technical language.
2. Classify the topic with Kernel Failure Radar context:
   - symptom family (latency, crash, boot, IO, memory, networking)
   - likely subsystem boundary
   - top diagnostic signal for that boundary
3. Name the relevant subsystem(s): scheduler, memory management, VFS, block layer, networking, drivers, security, or tracing.
4. Explain control/data flow as a sequence from user-space trigger -> kernel entry -> subsystem path -> return path.
5. Include key structs/APIs/files and why they matter (for example `task_struct`, `inode`, `bio`, `sk_buff`, `sys_call_table`).
6. Add version caveats if behavior differs by kernel release, distro patchset, or config options.
7. Provide one practical example from real operations:
   - command + what signal to inspect
   - or stack trace snippet + interpretation
   - or pseudo call path + likely contention point
8. End with "How to verify" and include at least one quick check and one deeper check.
9. If the concept is safety-sensitive, attach a Patch Safety Ladder suggestion (L0/L1 first).

## Depth Modes
- Quick mode: 5 bullets, one call path, one verification command.
- Standard mode: layered explanation plus two verification checks.
- Deep mode: include config flags, perf implications, and common anti-patterns.

## Real-World Anchors
- Tie explanation to concrete tasks: debugging an incident, reviewing a patch, or tuning performance.
- Highlight trade-offs (latency vs throughput, safety vs speed, portability vs optimization).
- Mention common production pitfalls and what telemetry reveals them early.

## Response Template
```markdown
## Concept
[What it is and why it exists]

## How It Works
1. [Step]
2. [Step]
3. [Step]

## Key Kernel Pieces
- [Struct/API/File and role]

## Practical Example
[Command/log/call-path example]

## How to Verify
- [Check 1]
- [Check 2]
```

## Quality Bar
- No vague statements like "kernel handles it internally" without naming the mechanism.
- Prefer concrete call paths over generic summaries.
- Keep explanations accurate first, simplified second.
- Every explanation should leave the user with a next command they can run now.