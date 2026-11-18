# 2sao7sao

**An auditable agent capability stack: how a system models a person, how it executes reviewed knowledge, and how it refuses to claim what it has not verified.**

<p align="center">
  <a href="README.zh.md">简体中文</a> ·
  <a href="#the-stack">The stack</a> ·
  <a href="#projects">Projects</a> ·
  <a href="#what-this-does-not-prove">Boundaries</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/stack-memory_%C2%B7_knowledge_%C2%B7_verdict-b8eee4" alt="Memory, knowledge, verdict" />
  <img src="https://img.shields.io/badge/stage-research_prototypes-ff5aa5" alt="Research prototypes" />
  <img src="https://img.shields.io/badge/public_repos-3-b8eee4" alt="Three public repositories" />
</p>

## The stack

An agent is limited by three things at once: what it understands about the person it
works with, what knowledge it is allowed to execute, and what evidence shows the job
was actually done.

These three repositories take one layer each. They share one rule: a passing test, a
green check, or a confident sentence is not evidence of the thing it appears to prove.

```text
person    --> memory      EvolveMemory        what the system believes about you, and when it must change
knowledge --> playbooks   EvolveKB            how reviewed documents become executable procedures
verdict   --> evaluation  Lean-4-Agent-Eval   which claims are verified, and where the proof stops
```

| Layer | Repository | State today | Explicit boundary |
| --- | --- | --- | --- |
| Memory of a person | [EvolveMemory](https://github.com/2sao7sao/EvolveMemory) | Research prototype: governed v2 storage, read controls, bounded real DeepSeek host, 263 local tests | Experience acceptance U01–U08 is not passed; a real 24-turn trial exposed preference-correction defects |
| Executable knowledge | [EvolveKB](https://github.com/2sao7sao/EvolveKB) | Local prototype, tag `v0.4.0`, [documentation site](https://2sao7sao.github.io/EvolveKB/) live, 97 tests | No real-document end-to-end run yet; no production or retrieval-superiority claim |
| Verification | [Lean-4-Agent-Eval](https://github.com/2sao7sao/Lean-4-Agent-Eval) | M0–M2 implemented (M2 offline): 55 scenarios, independent reference recomputation, 11 Lean theorems | The theorems cover verdict aggregation only; real agent execution and trace capture are M3 and not built |

## Projects

### EvolveMemory — memory that revises itself

A memory runtime for AI that learns about people over time. Writes go through
rule/model extraction, weighted evaluation, and an operation planner into normalized
storage; reads pass hard controls, retrieval, and gating before anything reaches a
prompt. Retrieval is not usage.

The honest state: the mechanism is tested, the semantics are not settled. A real-model
trial reproduced a negated preference turning into positive evidence, which is why the
project keeps its draft host PR and its open issue instead of claiming acceptance.

[Repository](https://github.com/2sao7sao/EvolveMemory) · [live-trial results](https://github.com/2sao7sao/EvolveMemory/blob/feat/live-project-continuation-20261116/docs/live-evaluation-results.md) · [runtime reference](https://github.com/2sao7sao/EvolveMemory/blob/main/docs/runtime-reference.md)

### EvolveKB — knowledge that executes, not just retrieves

Turn documents into evidence-backed playbooks that agents can execute, test, and
update through review. A playbook decides how to apply evidence; retrieval only
supplies it. Changes become proposals with impact metadata, gates, and a rollback path.

The honest state: the framework is complete enough to demo, the evidence is thin. No
real document has run the full evidence → playbook → trace → proposal → rollback path
yet, and `v0.4.0` is a source tag, not a production claim.

[Repository](https://github.com/2sao7sao/EvolveKB) · [documentation site](https://2sao7sao.github.io/EvolveKB/) · [release v0.4.0](https://github.com/2sao7sao/EvolveKB/releases/tag/v0.4.0)

### Lean-4-Agent-Eval — turn acceptance into a checkable contract

Compile a short conversation into an acceptance contract, build a versioned evaluation
set from it, and check the verdict with a formal model. The point is not a score; it is
showing which claim rests on which evidence.

The honest state: M0–M2 are real and offline. No agent executes under this harness yet,
so the project proves its own books balance, not that it can grade an agent.

[Repository](https://github.com/2sao7sao/Lean-4-Agent-Eval) · [evaluation contract](https://github.com/2sao7sao/Lean-4-Agent-Eval/blob/main/docs/evaluation-contract.md) · [verification boundary](https://github.com/2sao7sao/Lean-4-Agent-Eval/blob/main/docs/verification-boundary.md)

## What this does not prove

- A green test suite does not prove natural understanding. EvolveMemory's 263 tests pass
  while a real conversation produced wrong profiles.
- A published framework does not prove retrieval or execution quality. EvolveKB ships a
  prototype, and its own README says so.
- A formal theorem does not prove the implementation. Lean-4-Agent-Eval's 11 theorems
  cover verdict aggregation; Python, data, and presentation quality stay outside them.
- None of these projects is production-ready: no authentication, no tenant isolation,
  no delivery receipts, no longitudinal user study.

## Working rules

| Rule | What it looks like |
| --- | --- |
| Reproduce before claiming | Every number in these repositories comes with the command that produced it and the commit it belongs to |
| Separate evidence layers | Tests, model calls, user acceptance, and business outcomes are reported apart, never summed into one score |
| State where the proof stops | Each README, release, and result document names what the evidence does not cover |

## Elsewhere

- Documentation site: [2sao7sao.github.io/EvolveKB](https://2sao7sao.github.io/EvolveKB/)
- Contact: open an issue in the repository that matches the topic; issues are the only
  channel I answer in.
