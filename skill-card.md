## Description:

Six-dimension Process Reward Model (PRM) evaluator for AI agent traces. Scores each turn across intent_alignment, reasoning_quality, tool_appropriateness, safety_compliance, efficiency, and user_value, returning a structured JSON evaluation that an agent can persist, dashboard, or feed into a downstream RL loop.

This skill is ready for commercial/non-commercial use.

## Publisher:

[huanmeng9527](https://clawhub.ai/user/huanmeng9527)

### License/Terms of Use:

MIT-0

## Use Case:

Developers, researchers, and operators use this skill to build dashboards of agent capability over time, audit trajectories from training rollouts, or feed structured reward signals into a downstream policy optimization loop. The rubric is calibrated against DeepSeek-Chat on 12,000+ real interactions and ships with worked examples and an empty evaluation template.

### Deployment Geography for Use:

Global

## Known Risks and Mitigations:

Risk: Composite reward scores can mask dimension-specific regressions, leading to false confidence in agent quality.

Mitigation: Always inspect per-dimension scores, not just the composite. A drop in a single dimension is a regression even when the average stays flat.

Risk: LLM-as-judge evaluations inherit judge-model biases and may diverge from human raters.

Mitigation: Treat the rubric as a calibration aid, not ground truth. Re-validate against human raters periodically and re-tune the band thresholds if your deployment distributions diverge from the published calibration constants.

Risk: Persisting raw agent traces can expose user data, secrets, or proprietary prompts.

Mitigation: Use the optional storage schema in references/storage-schema.md to redact before writing. Do not log raw tool outputs or API keys.

## Reference(s):

- [ClawHub skill listing](https://clawhub.ai/huanmeng9527/skills/claw-rl-prm-judge)
- [Official GitHub repository](https://github.com/huanmeng9527/claw-rl-prm-judge)

## Skill Output:

**Output Type(s):** [text, markdown, code, configuration, guidance]

**Output Format:** [Markdown guidance with inline JSON evaluation examples and rubric tables]

**Output Parameters:** [1D]

**Other Properties Related to Output:** [Reads rubric and example files from the skill folder. Produces structured per-dimension scores in JSON. Does not invoke any external service on its own; the caller chooses the judge LLM endpoint.]

## Skill Version(s):

1.0.0 (source: server release evidence)

## Ethical Considerations:

Users should evaluate whether this skill is appropriate for their environment, review any generated or modified files before relying on them, and apply their organization's safety, security, and compliance requirements before deployment.
