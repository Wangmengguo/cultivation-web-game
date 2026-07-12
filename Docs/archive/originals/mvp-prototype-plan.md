# MVP Prototype Plan

Build a disposable single-file web prototype for the cultivation sect idle loop.

## Goal

Validate whether a lightweight web game can feel interesting when the player, as sect leader, assigns disciples, waits, and reads a central time-passage log showing disciple progress and material flow.

## Non-Goals

- No backend, account system, sync, multiplayer, or payments.
- No framework, build step, package manager, or dependency install.
- No map, animated characters, building placement, art pipeline, or images.
- No manual battle, random combat, accidents, heart demons, fatigue, gacha, story, or recruiting.

## Deliverable

Create `index.html` at the repo root.

It must run by opening the file directly in a browser. Use only inline HTML, CSS, and JavaScript.

## Core Model

### Disciples

Start with 5 fixed disciples. Each disciple has:

- `name`
- `realm`
- `cultivation`
- `aptitude`
- `martial`
- `job`

Show aptitude as a number plus label, for example `82 (天才)`.

Use these labels:

- `0-49`: `凡`
- `50-69`: `良`
- `70-84`: `优`
- `85+`: `天才`

### Realms

Use three realms:

- `炼气`
- `筑基`
- `金丹`

Each realm has a cultivation cap and breakthrough cost:

| Realm | Cultivation Cap | Breakthrough Cost |
| --- | ---: | --- |
| 炼气 | 100 | `2 丹药`, `10 凡阶材料` |
| 筑基 | 220 | `4 丹药`, `8 灵阶材料` |
| 金丹 | no next realm | none |

Breakthrough is deterministic. If the disciple has full cultivation and the sect has enough resources, clicking breakthrough always succeeds.

### Resources

Track only:

- `凡阶材料`
- `灵阶材料`
- `丹药`

Initial resources:

- `凡阶材料`: `10`
- `灵阶材料`: `0`
- `丹药`: `0`

### Jobs

Each disciple has one job:

- `修炼`: gain `round(aptitude / 10)` cultivation each day.
- `采药`: gain `2 凡阶材料` each day.
- `炼丹`: if at least `3 凡阶材料`, spend `3 凡阶材料` and gain `1 丹药`.
- `外出历练`: output depends on the selected expedition target; otherwise gain nothing and log why.

Expedition is not manual combat. It is a predictable realm/martial gate:

- `山脚`: always manageable, gain `1 凡阶材料`.
- `妖兽林`: needs `martial >= 35` or realm at least `筑基`, gain `1 灵阶材料`.

For the first prototype, expose one expedition target selector for all disciples assigned to `外出历练`.

### Priority Breakthrough Disciple

Exactly one disciple can be marked as the priority breakthrough disciple.

Effects:

- When assigned to `修炼`, cultivation gain is multiplied by `1.5`.
- If assigned to `修炼` and there is at least `1 丹药`, consume `1 丹药` once per day for that disciple and add `12` extra cultivation.

This is the prototype's main resource decision.

After breakthrough, the disciple's cultivation resets to `0`. `金丹` disciples do not have a next realm; once they reach `220` cultivation, stop increasing cultivation.

## Time

- Every `12` real seconds equals `1` game day.
- Add a `快进一天` button for testing.
- Save to `localStorage` after every simulated day and on every player change.
- On page load, calculate offline days from `Date.now()` and the saved timestamp.
- Cap offline catch-up at `100` days.
- Log a summary line when offline catch-up runs.

## UI

Use a responsive single-page layout:

1. Top: sect resources, current day, expedition target, reset button.
2. Center: time-passage log. This is the primary validation area and should be visually central.
3. Bottom/side depending on viewport: disciple list with realm, cultivation, aptitude, martial, job selector, priority selector, breakthrough button/status.

When the selected expedition target is `妖兽林`, show `能应付` or `武力不足` beside disciples assigned to `外出历练`.

Mobile must not require horizontal scrolling.

## Log Tone

Logs should be light and Kairosoft-like, but every entry that changes state must show numeric changes in parentheses.

Examples:

- `韩立在静室里憋了一整天，修为真的涨了（修为 +18）`
- `林青云把丹药当糖豆吃，进展很快（丹药 -1，修为 +12）`
- `小竹在山脚捡了一筐能用的东西（凡阶材料 +1）`
- `赵铁柱盯着妖兽林看了半天，觉得今天还是算了（灵阶材料 +0，武力不足）`

Keep only a bounded recent log, around 80 entries.

## Acceptance Criteria

1. Opening `index.html` directly in a browser starts the game with fixed disciples and resources.
2. Changing a disciple's job affects the next simulated day and is visible in both resources/cultivation and log entries.
3. A full loop works: gather `凡阶材料` -> refine `丹药` -> prioritize one disciple -> gain cultivation -> pay breakthrough costs -> realm advances.
4. Expedition outcome is predictable from UI: the player can see whether `妖兽林` is manageable before the next day resolves.
5. Closing and reopening the page applies capped offline catch-up and writes readable log entries with numeric deltas.
6. The page is usable on a phone-width viewport with no horizontal scrolling.

## Implementation Constraints

- Keep everything in `index.html`.
- No dependencies.
- Keep logic plain and inspectable.
- Do not create a reusable game engine, component framework, asset system, or data-loader abstraction.

## Verification

After implementation:

1. Open `index.html` directly or serve it with a trivial local static server only if needed for inspection.
2. Use the `快进一天` button to verify resource changes and logs.
3. Change jobs and verify the next day reflects the new assignments.
4. Mark a priority breakthrough disciple and verify丹药 consumption and cultivation bonus.
5. Force or wait for enough resources, click breakthrough, and verify deterministic realm advancement.
6. Refresh the page and verify saved state remains.
