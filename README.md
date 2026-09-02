# AI Engineering Council — Codex Skill

A no-paid-model-API engineering council for Codex. It coordinates independent proposals and adversarial reviews using browser-accessible AI web apps when available, then creates an auditable architecture decision before Codex implements code.

## Install

Copy this folder to your Codex skills directory:

- Linux/macOS: `~/.codex/skills/ai-engineering-council/`
- Windows: `%USERPROFILE%\\.codex\\skills\\ai-engineering-council\\`

Then restart/reload Codex so it discovers the skill.

## Invoke

Use it explicitly:

```text
$ai-engineering-council Design and implement multi-UAV task allocation for this repository.
```

Or:

```text
$ai-engineering-council full: Debate whether this project should use A*, D* Lite, or RRT* before changing the planner.
```

## Philosophy

The goal is not to make multiple models agree. The goal is to expose bad assumptions before code is written.

## No paid APIs

The skill is intentionally designed not to require Anthropic, xAI, Google, or OpenAI model API keys for external council members. When supported, it uses already-authorized browser/web-chat sessions. Provider availability, account limits, and terms still apply.
