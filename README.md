# claw-rl-prm-judge

Six-dimension PRM (Process Reward Model) evaluator for AI agent traces. Adapted from the OpenClaw-RL online policy optimization loop (arXiv:2603.12644).

## Install

```bash
openclaw skill install claw-rl-prm-judge
```

## Use

Ask your OpenClaw agent:

> "Evaluate the last agent turn with the PRM judge skill"

Or use as a slash command: `/skill claw-rl-prm-judge`

## Files

- `SKILL.md` — Skill manifest and instructions for the agent
- `references/dimensions.md` — Six-dimension rubric with calibration bands
- `references/storage-schema.md` — How to persist scores (SQLite + JSONL options)
- `examples/judge-prompt.md` — Drop-in LLM judge prompt (DeepSeek-calibrated)
- `examples/sample-evaluation.json` — Worked example with annotations
- `templates/eval-turn.json` — Empty template for new evaluations

## What this skill does NOT do

- Does not run as a runtime hook (zero side effects on agent behavior)
- Does not auto-apply policy updates
- Does not modify any files outside your explicit storage path
- Does not require an API key (bring your own LLM endpoint)

For the hook version that does intercept bad behavior at runtime, see the companion skill `rl-runtime-guard` (Phase 2).

## Provenance

Adapted from a real online RL loop that processed 12,000+ agent interactions across a 6-month period. The full loop includes three runtime guards that catch 82.5% of agent-fault errors at runtime — but those are out of scope for this skill's purpose (evaluation only).

## License

MIT
