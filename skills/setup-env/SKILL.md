---
name: setup-env
description: "Builds a production-relevant Linux kernel development environment with deterministic tooling, safe test boot paths, and observability-ready validation."
license: MIT
allowed-tools: "Read Write"
metadata:
  author: kernel-mentor-agent
  version: "1.0.0"
  category: kernel-tooling
---

# Setup Environment

## When to Use
Use this skill when a user needs to prepare or repair a Linux kernel dev environment, including dependencies, source tree setup, build configuration, and test boot workflow.
Always respond directly to the user with a complete environment setup plan. Provide best-effort steps immediately; ask clarifying questions only after delivering an initial working workflow.

## Setup Principles
- Prefer isolated environments (VM/QEMU/container) before host mutation.
- Pin critical tools when reproducibility matters.
- Verify each stage before moving forward.
- Keep a rollback kernel/image available at every boot experiment.

## Instructions
1. Identify platform facts: distro, kernel version, architecture, disk space, and CPU/RAM.
2. Select setup profile:
   - `fast-lab`: fastest iteration, lower reproducibility
   - `repro-lab`: pinned versions, CI-friendly
   - `incident-lab`: optimized for debug symbols and tracing
3. Install base toolchain and build dependencies.
4. Fetch kernel source (stable tag or target branch) and verify integrity.
5. Seed config (`defconfig`, distro config, or known-good baseline).
6. Enable capability flags aligned to goal (debug/perf/security).
7. Build kernel and modules with parallel jobs tuned to host capacity.
8. Prepare boot/test path (QEMU, kexec, or bootloader entry) with rollback option.
9. Run smoke, stress, and observability checks before declaring environment ready.

## Baseline Command Blocks
```bash
# System facts
uname -a
cat /etc/os-release
nproc

# Typical dependencies (example for Debian/Ubuntu-like systems)
sudo apt update
sudo apt install -y build-essential bc bison flex libssl-dev libelf-dev dwarves pahole \
  cpio rsync kmod git libncurses-dev pkg-config python3

# Source + config + build (example)
git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git
cd linux
make defconfig
make -j"$(nproc)"
make modules -j"$(nproc)"
```

## Optional Debug-First Config Additions
```bash
# Start from current config if available
cp /boot/config-"$(uname -r)" .config || true
yes "" | make oldconfig

# Helpful debug toggles (example workflow)
scripts/config --enable CONFIG_DEBUG_INFO
scripts/config --enable CONFIG_KALLSYMS
scripts/config --enable CONFIG_FTRACE
scripts/config --enable CONFIG_BPF
```

## Environment Verification
- Build completes without fatal errors.
- `vmlinux` and kernel image are produced.
- Modules install/load path is valid.
- Test boot succeeds and `uname -r` matches expected build.
- Basic logs are clean (`dmesg` contains no new critical errors).
- Failure Radar probe works (collect at least one useful kernel signal quickly).
- Patch Safety Ladder baseline defined (L1 sandbox path validated).

## Readiness Scorecard
- Toolchain health: pass/fail
- Build repeatability: pass/fail
- Test boot safety: pass/fail
- Observability depth: basic/standard/advanced
- Rollback confidence: low/medium/high

## Failure Handling
- If build fails: classify as dependency, config, compiler, or source issue first.
- If boot fails: capture serial/console logs and revert to previous kernel immediately.
- If modules fail to load: check vermagic/signing/config mismatch.
- If tracing is empty: verify debugfs mount, tracepoint availability, and required privileges.
