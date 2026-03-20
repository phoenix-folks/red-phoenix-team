# Ember Configuration

## Orchestration
max_parallel: 4
review: mandatory for all plan tasks (two-stage: spec-compliance then code-quality)

## Model Routing

Ember subagents are dispatched via the Agent tool with model parameters to balance cost and quality:
- **ember-verifier**: `model="haiku"` (fast, cheap verification checks)
- **ember-reviewer, ember-debugger**: `model="sonnet"` (strong reasoning at moderate cost)
- **ember-exec**: inherits parent model (no model param — matches task complexity)

Read-only enforcement uses `subagent_type="Explore"` for reviewer, debugger, and verifier — this is platform-enforced by Claude Code.

For your global model, consider Sonnet as a cost-effective default. Reserve Opus for complex planning sessions where deeper reasoning pays off. You can set this per-session in Claude Code's model selector.
