# Agent team

This repository uses a small custom agent team to build Mona's Project Pulse dashboard. Each agent is defined under .github/agents and coordinated from the GitHub Copilot CLI running in this Codespace.

- Orchestrator — model: Claude Opus 4.7
  - Responsibility: Coordinate Planner, Coder, and Designer; break plans into phases, assign file scopes, run safe parallel tasks, and verify integration.
  - Definition: .github/agents/orchestrator.agent.md

- Planner — model: Claude Opus 4.7
  - Responsibility: Research the codebase and produce practical implementation plans with ordered steps, file assignments, dependencies, validation criteria, and open questions.
  - Definition: .github/agents/planner.agent.md

- Coder — model: GPT-5.5 (copilot)
  - Responsibility: Implement code changes, unit-testable behavior, and runnable-app support (e.g., create deterministic .vscode/launch.json for Project Pulse when assigned).
  - Definition: .github/agents/coder.agent.md

- Designer — model: Gemini 3.1 Pro (copilot)
  - Responsibility: Provide UI/UX, accessibility, information architecture, responsive visual design, and deterministic CSS hooks for the dashboard.
  - Definition: .github/agents/designer.agent.md

Note: Git operations (commit, push) remain under the learner's control via Copilot CLI prompts; agents report changes and validation recommendations but do not perform git actions directly.