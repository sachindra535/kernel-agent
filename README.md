# kernel-mentor-agent

A Linux kernel mentoring and debugging agent built for GitClaw.

`kernel-mentor-agent` helps engineers understand kernel internals, debug failures, and build reproducible development environments using structured, production-focused workflows.

It combines:

- Kernel Failure Radar
- Patch Safety Ladder
- Mentor-grade explanations
- Incident-driven debugging
- Environment setup guidance

---

# Features

## Kernel Failure Radar

Rapidly classifies failures into likely subsystems and provides:

- Confidence-ranked hypotheses
- Highest-yield diagnostic commands
- Fast falsification tests
- Next investigation steps

Example:

```text
Kernel panic
↓
Failure Radar
↓
Memory / Driver / Scheduler hypothesis ranking
↓
Verification commands
```

---

## Patch Safety Ladder

Every recommendation is tagged with a risk level:

| Level | Meaning |
|---------|---------|
| L0 | Observe only |
| L1 | Reproduce safely |
| L2 | Minimal targeted change |
| L3 | Regression and performance validation |
| L4 | Rollout-ready with rollback plan |

---

## Mentor-Grade Explanations

Every explanation includes:

- Mental model
- Kernel call path
- Important structs
- Relevant source files
- Verification steps

---

## Specialized Skills

### explain-kernel

Explains Linux kernel concepts and subsystem interactions.

Examples:

```bash
gitclaw run . "Use the explain-kernel skill to explain how the Linux scheduler works"
```

```bash
gitclaw run . "Use the explain-kernel skill to explain how softirqs work"
```

---

### debug-code

Incident-grade debugging workflow.

Examples:

```bash
gitclaw run . "Use the debug-code skill to analyze this kernel panic"
```

```bash
gitclaw run . "Use the debug-code skill to debug a scheduler latency spike"
```

---

### setup-env

Builds safe kernel development environments.

Examples:

```bash
gitclaw run . "Use the setup-env skill to create an Ubuntu kernel development lab"
```

```bash
gitclaw run . "Use the setup-env skill to set up a kernel debugging environment for Arch Linux"
```

---

# Project Structure

```text
kernel-mentor-agent/
├── agent.yaml
├── SOUL.md
├── RULES.md
├── memory/
└── skills/
    ├── explain-kernel/
    │   └── SKILL.md
    ├── debug-code/
    │   └── SKILL.md
    └── setup-env/
        └── SKILL.md
```

---

# Local LLM Setup (Free)

This project can run completely free using Ollama.

## 1. Install Ollama

Arch Linux:

```bash
sudo pacman -S ollama
```

Enable service:

```bash
sudo systemctl enable --now ollama
```

Verify:

```bash
systemctl status ollama
```

---

## 2. Download a Model

Recommended:

```bash
ollama pull qwen3:8b
```

Other options:

```bash
ollama pull llama3.1:8b
```

```bash
ollama pull mistral:7b
```

---

## 3. Configure GitClaw

Set environment variables:

```bash
export OPENAI_API_KEY=dummy
export GITCLAW_MODEL_BASE_URL=http://localhost:11434/v1
```

These allow GitClaw to use the local Ollama endpoint.

---

## 4. Update agent.yaml

Example:

```yaml
model:
  preferred: openai:qwen3:8b
```

---

## 5. Run the Agent

Interactive mode:

```bash
gitclaw
```

or

```bash
gitclaw run .
```

---

# Usage

The recommended format is:

```bash
gitclaw run . "Use the <skill-name> skill to <task>"
```

Examples:

```bash
gitclaw run . "Use the explain-kernel skill to explain how the Linux scheduler works"
```

```bash
gitclaw run . "Use the explain-kernel skill to explain virtual memory"
```

```bash
gitclaw run . "Use the debug-code skill to debug a kernel panic"
```

```bash
gitclaw run . "Use the setup-env skill to create a kernel development environment on Arch Linux"
```

---

# Example Output

The agent typically provides:

1. Mental model
2. Subsystem classification
3. Kernel call path
4. Important structs/APIs
5. Verification commands
6. Patch Safety Ladder recommendations
7. Concrete next actions

---

# Why Use This Agent?

- Faster kernel debugging
- Better subsystem understanding
- Safer patch development
- Consistent troubleshooting workflows
- Works entirely with free local LLMs

---

# License

MIT License
