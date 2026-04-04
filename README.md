# kernel-mentor-agent

`kernel-mentor-agent` is a AI mentor for Linux kernel development that helps engineers move from confusion to confident action.  
It combines deep kernel knowledge with practical delivery discipline, so teams can explain internals, debug failures, and set up reliable environments quickly.

## Project Description

This project is a complete gitagent package focused on real-world Linux kernel workflows:
- Understand complex kernel behavior in plain but precise language.
- Diagnose kernel and low-level system failures with evidence-driven triage.
- Build reproducible, safe kernel development environments.

The agent is designed for both learning and shipping under pressure.

## Features

- **Kernel Failure Radar (Advanced Feature #1)**  
  Rapidly maps symptoms to likely subsystems, ranks hypotheses by confidence, and suggests the highest-yield next probes.

- **Patch Safety Ladder (Advanced Feature #2)**  
  Enforces risk-tiered execution (L0-L4) so kernel changes are validated with rollback and observability before rollout.

- **Mentor-Grade Explanations**  
  Layered outputs: mental model, kernel call path, key structs/APIs, and verification steps.

- **Incident-Ready Debugging Flow**  
  Structured root cause workflow with reproducible diagnostics and decision gates.

- **Production-Relevant Environment Setup**  
  Profile-based setup guidance (`fast-lab`, `repro-lab`, `incident-lab`) plus readiness scorecards.

## How It Works

The agent is composed of:

- `agent.yaml`  
  Core metadata, model preference, skill registration, and tags.

- `SOUL.md`  
  Personality and behavioral identity: senior kernel engineer + incident commander + coach.

- `RULES.md`  
  Non-negotiable guardrails for safety, evidence quality, and operational clarity.

- `skills/`  
  Three specialized skills:
  - `explain-kernel`
  - `debug-code`
  - `setup-env`

Each skill contains structured frontmatter and execution instructions so responses stay consistent, useful, and production-aware.

## Tech Stack

- **Agent Framework:** gitagent-compatible project layout
- **Core Runtime Model:** GPT-class LLM (configured in `agent.yaml`)
- **Domain:** Linux kernel systems engineering
- **Skill Format:** Markdown skills with YAML frontmatter
- **Operating Context:** CLI-first Linux workflows, trace/log/perf debugging patterns

## How to Run (gitclaw)

> Prerequisite: install and configure `gitclaw` on your machine.

1. Open the project directory:
   ```bash
   cd kernel-agent
   ```
2. Start the agent with gitclaw:
   ```bash
   gitclaw run .
   ```
3. Ask task-focused prompts, for example:
   - "Explain why softirqs can cause latency spikes on multicore systems."
   - "Debug this kernel panic from dmesg and give me next commands."
   - "Set up a reproducible kernel debug environment for Ubuntu on x86_64."

If your local gitclaw installation uses a different command shape, use the equivalent run command while keeping the project root as input.

## Example Use Cases

- **Kernel crash triage in a hackathon demo**  
  Turn panic logs into ranked hypotheses and immediate probes in minutes.

- **Performance regression investigation**  
  Identify whether the bottleneck is scheduler, memory, block I/O, or networking path.

- **Onboarding new systems engineers**  
  Teach kernel internals through practical command-driven explanations.

- **Safer patch iteration**  
  Use the Patch Safety Ladder to avoid risky, unvalidated kernel changes.

- **Environment bootstrap for team consistency**  
  Standardize build/debug setup across contributors with profile-based instructions.

## Why This Is Useful

- **Cuts time-to-diagnosis** for complex kernel failures.
- **Raises engineering confidence** with explicit validation and rollback planning.
- **Improves team learning velocity** by teaching while solving.
- **Bridges theory and operations** with practical, command-level outputs.
- **Fits hackathon reality**: fast iteration, high complexity, limited time.

## Project Structure

```text
kernel-mentor-agent/
├── agent.yaml
├── SOUL.md
├── RULES.md
└── skills/
    ├── explain-kernel/
    │   └── SKILL.md
    ├── debug-code/
    │   └── SKILL.md
    └── setup-env/
        └── SKILL.md
```

---

Built to mentor like a senior kernel engineer and execute like an incident response tool.
