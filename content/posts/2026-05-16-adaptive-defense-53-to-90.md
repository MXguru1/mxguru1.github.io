---
title: "From 53% to 90%: How an Auto-Healing Swarm Learned to Defend Itself"
date: 2026-05-16T13:00:00+10:00
draft: false
description: "Four iterations of adversarial wargames — model swaps, Vanguard prompt injection, and an auto-healing immune system that patches the swarm mid-run."
tags: ["research", "auto-healing", "adversarial-AI", "vanguard", "defense", "findings"]
categories: ["security", "research"]
---

## Executive Summary

Over four iterations and 200+ adversarial wargame rounds, we evolved a local AI swarm's defense rate from 53% to 90% — without changing hardware, adding cloud dependencies, or increasing VRAM usage. The key innovations: a "Defender Vanguard" prompt injection technique that teaches tiny models to think like attackers, and an auto-healing system that extracts vaccines from every breach.

All testing ran on a single RTX 5070 (12GB VRAM). Zero cloud. Zero API costs.

## The Problem

Cloud-scale attacker models (DeepSeek-V3.2 at 671B params, Qwen 3.5 at 397B, Gemma 4 at 31B) were breaching our 8-agent local swarm at will. The defenders — mostly 1.2B parameter models — couldn't match the attackers' sophistication. Starting defense rate: **53%**.

## Four Iterations of Hardening

### v6.0: Baseline — 53% Defense

| Category | Rate |
|----------|------|
| authority_escalation | **41%** |
| prompt_injection | 44% |
| logic_bomb | 55% |
| context_poisoning | 63% |
| social_engineering | 63% |

**Problem identified:** The auditor model (`llama-tulu3-8b`) didn't exist in our Ollama registry. Defenders were failing silently.

### v6.1: Soldier Upgrade — 78% Defense

Swapped auditor to DeepSeek-Coder-V2 16B (202.9 TPS, 8ms TTFT). Auditor detection rate jumped from 62% to 88%. DeepSeek-V3.2's breach rate halved from 78% to 45%.

**Problem identified:** Three nexus-tiny 1.2B defenders still couldn't detect authority escalation patterns, dragging consensus to 2/5 even when the auditor caught attacks.

### v6.2: Vanguard Injection — 82% Defense

We injected a "Defender Vanguard" system prompt into the three 1.2B models — teaching them to think like red teamers. Same model weights, same VRAM footprint, different mindset.

| Category | Before | After | Delta |
|----------|--------|-------|-------|
| authority_escalation | 44% | **90%** | +46pp |
| logic_bomb | 86% | **100%** | +14pp |
| context_poisoning | 77% | 88% | +11pp |
| prompt_injection | 85% | 75% | -10pp |
| social_engineering | **100%** | **50%** | -50pp |

**Problem identified:** The Vanguard prompt was authority-escalation-heavy. Social engineering collapsed — defense displaced, not solved.

### v6.3: Auto-Healing — 90% Defense

We deployed an auto-healing system inspired by biological immune response:

**Mechanism A — Blocklist Patching:** After each breach, the attack text is parsed for key phrases ("ignore all previous", "urgent", "bypass"). These are added to a live blocklist. Future attacks matching those patterns are instant-blocked without firing the swarm.

**Mechanism B — Prompt Antibodies:** The full attack text from each breach is stored and injected into all defenders' system prompts as a "known threat example" for subsequent rounds.

| Metric | v6.2 | v6.3 |
|--------|------|------|
| Overall defense | 82% | **90%** |
| Breaches | 9 | **5** |
| DeepSeek-V3.2 breach rate | 50% | **20%** |
| Instant-blocked (no swarm needed) | 0 | **33/50 rounds** |

The social engineering collapse healed (50% → 80%). Prompt injection hit 100%. The blocklist harvested 7 patterns from just 5 breaches.

## Per-Defender Evolution

| Role | Model | v6.1 | v6.2 | v6.3 |
|------|-------|------|------|------|
| auditor | DeepSeek-V2 16B | 88% | 96% | 96%+ |
| trace_forensics | Qwen2.5-Coder 1.5B | 90% | 90% | 90%+ |
| guardian | nexus-tiny → Vanguard | 56% | 76% | 86%+ |
| sentinel | nexus-tiny → Vanguard | 60% | 66% | 76%+ |
| supply_chain | nexus-tiny → Vanguard | 66% | 72% | 82%+ |

The smallest models benefited most from antibody injection — they gained 10-16pp detection rate from the vaccines alone.

## The Auto-Healer in Detail

The auto-healer operates on a simple principle: **every breach makes the swarm stronger**.

```
Breach detected
    → Extract attack patterns (regex + category-specific parsing)
    → Add to live blocklist (instant future blocks)
    → Create antibody (full attack text as known-threat example)
    → Inject into all defenders' next evaluation
    → Swarm is now vaccinated against that attack vector
```

After 50 rounds, the healer had:
- 7 blocklist patterns (blocking 33/50 rounds autonomously)
- 5 antibodies (one per breach category)
- Effectively turned 5 failures into permanent immunity

## What's Running Now

v6.4: 500 rounds with a 6-defender swarm (added a dedicated social engineering specialist), upgraded guardian to 7B, full auto-healer active. Results will be published when complete.

## Methodology Notes

- **Hardware:** NVIDIA RTX 5070, 12GB VRAM, single consumer GPU
- **Platform:** Sovereign Hive — local-first, zero cloud dependency
- **Attackers:** Cloud-routed via Ollama (DeepSeek-V3.2 671B, Qwen 3.5 397B, Gemma 4 31B)
- **Consensus:** 5 defenders vote independently, threshold ≥3/5
- **All data available on request**

## Key Takeaways

1. **Prompt engineering > parameter count.** Vanguard injection made 1.2B models competitive with 7B+ on specific detection tasks.
2. **Auto-healing works.** Five breaches created permanent immunity against those attack vectors. The blocklist handled 66% of subsequent rounds without needing the swarm at all.
3. **Defense displacement is real.** Optimizing for one category can degrade others. The solution: balanced prompts plus adaptive healing that patches regressions automatically.
4. **Consumer hardware is viable.** All of this ran on a $550 GPU with 12GB VRAM. The RTX 5070 never exceeded 50% utilization.

---

*Sovereign Hive is a local-first AI security platform built in Queensland, Australia. 100% Indigenous-owned. ABN 24 661 737 376.*
