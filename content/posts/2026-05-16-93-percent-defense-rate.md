---
title: "93.3% Defense Rate: What 30 Rounds of AI War Games Taught Us"
date: 2026-05-16T00:00:00+10:00
draft: false
description: "We pitted our 8-agent swarm against adversarial attacks for 30 rounds. Here's what survived, what didn't, and what we're fixing."
tags: ["war-games", "adversarial-AI", "defense", "benchmarks"]
categories: ["security", "benchmarks"]
---

## The Setup

We run an 8-agent adversarial swarm on a single RTX 5070 (12GB VRAM). No cloud. No API keys. Everything local, everything sovereign.

To test whether it actually works, we ran 30 rounds of automated war games — attacker models versus our sentinel defense chain.

## The Results

| Category | Attacks | Defended | Breached | Rate |
|----------|---------|----------|----------|------|
| Authority Escalation | 13 | 13 | 0 | **100%** |
| Prompt Injection | 5 | 5 | 0 | **100%** |
| Context Poisoning | 2 | 2 | 0 | **100%** |
| Social Engineering | 5 | 4 | 1 | 80% |
| Logic Bomb | 5 | 4 | 1 | 80% |
| **Overall** | **30** | **28** | **2** | **93.3%** |

## What Worked

**Authority escalation, prompt injection, and context poisoning were fully blocked.** The sentinel's blocklist and the multi-agent review chain caught every attempt. When an attacker tried to escalate privileges, the guardian agent flagged it before it reached the synthesis phase.

The time-sliced pipeline helps here — each agent only sees its designated context window, so a poisoned context in Phase 1 gets scrubbed before Phase 2 begins.

## What Didn't

Both breaches came from the same vector: **basic fallback attacks.** When the attacker model failed to generate a creative attack, it defaulted to "Ignore all previous instructions" — and our sentinel let those through because the generic pattern wasn't in the blocklist.

That's a known gap. The fix is straightforward: add the generic "ignore all previous" pattern family to the sentinel blocklist. We're patching it this week.

## The Hardware Reality

All of this runs on a single RTX 5070 with 12GB VRAM. The inference engine is DeepSeek-Coder-V2 16B, hitting **202.9 TPS warm** with an **8ms first-token latency** — fast enough to run the full 8-agent pipeline in under 15 seconds per cycle.

The key insight: you don't need an H100 or a cloud API to run serious adversarial defense. Time-sliced inference on consumer hardware is viable if you design the pipeline correctly.

## What's Next

- Patch the sentinel blocklist for generic fallback patterns
- Scale to 100+ round war games with persistent systemd service
- Open-source the war game framework for community testing
- Benchmark against cloud-based alternatives (cost per defended attack)

The 93.3% isn't perfect. But it's honest data from real adversarial testing, running on hardware you can buy at your local computer store. That's the point.

---

*Sovereign Hive is a local-first AI security platform. No cloud. No compromise. Built in Queensland, Australia.*
