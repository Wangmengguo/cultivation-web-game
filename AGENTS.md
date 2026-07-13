# Project Agent Instructions

## Standing Delegation Authorization

The user gives standing project-level authorization for Codex to invoke subagents (`spawn_agent`) and hand off work to other model backends without asking for permission each time.

Proactively delegate independent implementation, analysis, research, review, or verification work when doing so improves speed, quality, coverage, or context isolation.

Work directly for trivial, single-step, urgent, or tightly coupled tasks where delegation adds no practical value.

Codex remains responsible for task decomposition, reading delegated results, integration, verification, and the final answer.

## Delegation Cost Rule

`handoff` has startup, prompt-packaging, and result-verification costs. Do not use it for simple, bounded tasks where those costs outweigh its benefits. For simple delegated work, prefer a normal Codex subagent (`spawn_agent`) using the ordinary model; reserve `handoff` for work where a specialized backend or independent model materially improves speed, quality, coverage, or cost.

## Preferred Handoff Routing

- For tasks with an explicit plan, prefer `handoff run --backend grok` (Grok 4.5) for as many suitable delegated steps as practical because it is inexpensive.
- Use another backend when its specialization materially improves the result or when `grok` is unavailable.
- Backend work: prefer `handoff run --backend go-glm --pro` (GLM-5.2).
- Writing and prose work: prefer `handoff run --backend sonnet` (Sonnet 5 alias).
- Difficult, ambiguous, or high-reasoning work: prefer `handoff run --backend fable`.
- Frontend work: prefer `handoff run --backend go-kimi --pro` (Kimi K2.7 Code).
- For mixed work, use the dominant task type.
- When the main issue is a hard blocker rather than routine implementation, prefer `fable`.
- If the preferred backend is unavailable, use the best available fallback and report the substitution.
