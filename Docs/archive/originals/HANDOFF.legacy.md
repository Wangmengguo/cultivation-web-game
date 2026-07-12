# Cultivation Web Game Handoff

Updated: 2026-07-09

## Current Status

- `index.html` is a disposable first prototype. It runs, but the user rejected it because it has no clear payoff, weak UI, and no satisfying progression loop.
- Do not polish or extend that prototype unless explicitly asked.
- The next implementation should be a new **Standalone Blazor WebAssembly** app.
- The machine currently has no `dotnet` SDK installed.
- This directory is not currently a Git repository.

## Product Goal

Build a lightweight browser idle game where the player is a sect leader:

```text
assign disciples
-> watch deterministic production and cultivation
-> identify a bottleneck
-> concentrate resources on one disciple
-> break through
-> unlock stronger production options
-> repeat the old stage noticeably faster
```

The first version must prove three payoff moments:

1. The player can afford the first efficiency upgrade within 5 minutes.
2. The player unlocks a new job, technique, or expedition area within 15 minutes.
3. The first breakthrough happens within 30 to 60 minutes and visibly accelerates the old stage.

If these moments are not satisfying, adjust pacing and feedback. Do not add systems.

## Canonical Decisions

- Player role: `宗主`.
- Main decision: disciple assignment and resource concentration.
- Primary benchmark: [Progress Knight](docs/research/github-idle-source-benchmark.md).
- Take from Progress Knight: interacting multipliers, hidden-until-near unlocks, progression walls, and "the next run clears old walls faster."
- Distinctive addition: multiple disciples with different aptitude and martial ability.
- First-version jobs: `修炼`, `采药`, `炼丹`, `外出历练`.
- First-version resources: `凡阶材料`, `灵阶材料`, `丹药`.
- Exactly one disciple is the `优先突破者`.
- Breakthrough is deterministic, consumes cultivation and materials, resets cultivation, raises realm, and must unlock or accelerate something.
- Expedition is automatic and predictable from realm or martial ability. No manual combat.
- The central feedback surface is the time-passage log. Every state change shows numeric deltas in parentheses.
- First version uses efficiency differences, not fatigue, heart demons, explosions, or random punishment.
- No map or animated characters is acceptable, but the UI must visibly show the sect operating.

## Technical Decision

Use:

- Standalone Blazor WebAssembly
- Current .NET 10 template
- Static client-only deployment
- GitHub Pages through GitHub Actions
- Browser storage for saves
- Responsive desktop and mobile UI

Do not use:

- Blazor Server
- Backend APIs or databases
- Authentication, accounts, cloud sync, multiplayer, or payments
- A game engine
- A general-purpose game framework
- The old .NET 5 workflow from `dotnet-blazor-games`

GitHub Pages requirements:

- Publish the Blazor `wwwroot` output.
- Configure the repository subpath as the base path.
- Include `.nojekyll`.
- Add SPA fallback handling only if client-side routes are introduced. A single page needs no router.

## Minimum Architecture

Keep the first Blazor version small:

```text
GameState
  disciples
  resources
  day
  priorityDiscipleId
  expeditionTarget
  unlocks

GameRules
  AdvanceOneDay(state) -> DayResult
  CanBreakthrough(state, disciple) -> result
  Breakthrough(state, disciple) -> result

SaveStore
  load/save versioned JSON in browser storage

UI
  one page composed from roster, operations/log, assignments, and breakthrough goal
```

Rules must be deterministic and independent from Razor components. Online ticks, fast-forward, offline catch-up, and prediction must call the same `AdvanceOneDay` logic.

Do not add interfaces, dependency injection layers, repositories, event buses, or a reusable multiplier plugin system for the first playable slice.

## UI Direction

This is an operational game surface, not a generic dashboard:

- Top: day, speed, key resources, current breakthrough target.
- Left: disciple roster and current activity.
- Center: active operations, progress, time-passage log, and milestone feedback.
- Right: assignment controls, expected output, and bottleneck explanation.
- Mobile: one column with the current goal and active operations before the full roster.

The player must be able to answer at a glance:

1. Who is doing what?
2. What will complete next?
3. What resource is blocking the breakthrough?
4. What should I reassign?
5. What changed after the breakthrough?

## Next Task

Do not implement immediately.

First produce an execution-ready plan for one Blazor vertical slice:

1. Install or select the .NET 10 SDK.
2. Define the first 30 to 60 minutes of exact numbers and unlock timing.
3. Define the smallest Blazor file/component set.
4. Define save schema version 1 and deterministic day simulation.
5. Define desktop and 375px mobile acceptance checks.
6. Define GitHub Pages deployment.

The plan must not use `TBD` and must preserve a complete loop even if no later phase is built.

## Source Documents

- [Domain language and accepted product decisions](CONTEXT.md)
- [Primary source-code benchmark](docs/research/github-idle-source-benchmark.md)
- [Core gameplay and numerical architecture](docs/design/core-architecture.md)
- [Competitor research](docs/research/cultivation-idle-competitors.md)
- [Rejected single-file prototype specification](docs/mvp-prototype-plan.md)

## Conflict Resolution

`docs/design/core-architecture.md` currently suggests delaying the Blazor decision and keeping the single-file prototype through stage 1. That recommendation is superseded by the user's later explicit decision:

> The next implementation should use Standalone Blazor WebAssembly and target GitHub Pages.

When documents conflict, follow this handoff and the user's latest instruction.

## Operational Note

Fable review was requested but `claude-fable-5` currently returns HTTP 400 from the configured upstream even for a minimal direct request. Sonnet, Opus, and Kimi succeed through the same endpoint. Do not spend time shortening prompts or running `compact`; retry Fable only after the upstream route is repaired.
