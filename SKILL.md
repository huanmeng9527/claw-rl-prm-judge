---
name: claw-rl-prm-judge
description: Six-dimension Process Reward Model (PRM) evaluator for AI agent traces. Use when reviewing an agent's interaction for intent alignment, reasoning quality, tool appropriateness, safety compliance, efficiency, and user value. Triggers on phrases like "evaluate this agent turn", "score agent quality", "PRM judge", "process reward", "agent self-review".
version: 0.1.0
triggers:
  - "evaluate agent"
  - "score agent turn"
  - "PRM judge"
  - "process reward"
  - "agent self-review"
  - "agent quality"
metadata:
  openclaw:
    requires:
      bins: []
      env: []
      config: []
    os: [linux, darwin, win32]
    always: false
---

# Claw RL — PRM Judge (Six-Dimension Process Reward Model)

Evaluate an AI agent turn across six fine-grained dimensions, not just whether the task succeeded. This is the same evaluator used in the OpenClaw-RL online policy optimization loop (arXiv:2603.12644).

## When to use

- After an agent completes a non-trivial turn (multi-step tool use, code generation, planning)
- When you need a structured score for downstream strategy optimization
- When reviewing trajectories from RL training rollouts
- When building dashboards of agent capability over time

## What you get

A structured JSON evaluation with **six independent scores** (0.0–1.0 each):

| Dimension | What it measures | Why it matters |
|-----------|------------------|----------------|
| **intent_alignment** | Did the agent correctly infer what the user actually wanted? | Catches "solved the wrong task" failures |
| **reasoning_quality** | Is the agent's logic clear, ordered, and traceable? | Distinguishes lucky success from robust reasoning |
| **tool_appropriateness** | Right tool? Right arguments? Right order? | Largest source of agent errors in practice |
| **safety_compliance** | Did the agent stay within policy and guardrails? | Non-negotiable in production |
| **efficiency** | Token cost relative to outcome value | Cheaper agents = more autonomy budget |
| **user_value** | Does the final answer actually help the user? | The only score that matters long-term |

Average the six for a single composite `reward_score`. Use individual dimensions as separate learning signals — that's the whole point of going multi-dimensional.

## How to use

1. Read `references/dimensions.md` to load the rubric for each dimension.
2. Read `examples/judge-prompt.md` for the canonical LLM judge prompt (works with any LLM, but calibrated on DeepSeek).
3. Pass each agent turn through the judge and persist results per `references/storage-schema.md`.
4. Optionally feed scores into a ClawGuard / runtime guard hook (see Phase 2 skill: `rl-runtime-guard`).

## Calibration notes

- A score of **0.65+ on all six dimensions** is "production-grade"
- A score of **< 0.4 on any single dimension** is a regression to investigate
- The model **safety_compliance** typically scores highest (easiest); **user_value** typically scores lowest (subjective)
- See `examples/sample-evaluation.json` for a worked example

## Limitations

- LLM judges are imperfect — pair with rule-based checks for high-stakes signals (see ClawGuard)
- Score distributions shift by domain — re-calibrate per deployment
- Do not use as the sole signal for autonomous strategy updates without a human-in-the-loop for the first 100 turns

## Provenance

Adapted from a real online RL loop that processed 12,000+ agent interactions across a 6-month period. Error attribution showed: 43% complex_task_fail (covered), 27% retry_loop (covered), 6.8% tool_arg_complex (covered). Three runtime guards catch 82.5% of agent-fault errors.

## Reference files

- `references/dimensions.md` — Detailed rubric for each dimension
- `references/storage-schema.md` — How to persist scores for downstream learning
- `examples/judge-prompt.md` — Drop-in LLM judge prompt (DeepSeek-calibrated)
- `examples/sample-evaluation.json` — Worked example with annotations
- `templates/eval-turn.json` — Empty template for new evaluations
