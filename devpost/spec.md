---
doc: spec
status: approved
---

# FlowMine — Technical Specification

A technical blueprint for building the "collapse animation" and interactive flowchart in vanilla HTML/CSS/JS, with no build tools.

## 1. System State & Control
Implements `prd.md > States and Boundaries`.

### `appState` machine
- `IDLE`: Initial state. Log editor (textarea) is active.
- `ANIMATING`: Collapse in progress. `#mine` and `#run` are disabled.
- `READY`: Flowchart and script visible. `#run` enabled.
- `RUNNING`: Simulation active. `#run` disabled.
- `DONE`: Simulation complete. Output panel shows "Done." and the badge resets to ×k.

### Interaction guards and cleanup
- **Live interrupts:** Editing the log or toggling domains stays possible during `ANIMATING` and `RUNNING`.
- **`cleanup()`** runs on any log or domain change, and when the user clicks the log viewer to edit:
  - cancels all animations in the `activeAnimations` registry
  - invalidates the current run by incrementing `runId` (every step of `run()` checks the token and stops if it changed)
  - removes the `.overlay` layer and its clones
  - clears the SVG chart, script and output panes
  - swaps the viewer back to the textarea
  - resets `appState` to `IDLE`

## 2. Animation Modes
1. **Full travel:** 4-stage WAAPI sequence using the `.overlay` layer.
2. **Fallback (`TRAVEL_ENABLED = false`):** log lines fade out in the left pane, the flowchart draws in, the badge lands last, the script fades in.
3. **Reduced motion (`prefers-reduced-motion`):** chart and script appear immediately with no animation.

## 3. Data Model
Derived from the prototype's `build()` in `app.js`, extended with log line indexes:

```js
{
  raw: [...],
  pre:  [{ text: "open dashboard.internal", idx: 0 }],
  body: [{ label: "search for ticket {n}", vals: [...], varies: true,
           indices: [[1,5,9,13], ...] }],   // log line index per iteration
  post: [{ text: "log out", idx: 17 }],
  k: 4
}
```

## 4. UI Layout and Components
Project structure (repo root): `index.html`, `style.css`, `app.js`, plus `devpost/`.
- **Log editor:** a `textarea` for editing and a `div.log-viewer` shown during animation.
- **Domain toggle:** "Monthly Report" and "Support Tickets" samples. Switching loads the sample and clears results.
- **Overlay layer:** a top-level `position: fixed` div that holds the traveling clones.
- **SVG flowchart:** nodes rendered at `opacity: 0` before measurement.
- **Script pane:** Python generated from the detected loop, fading in after the Land stage.
- **Output console:** monospace panel for run logs.
- **Messages:** "No repeated sequence found..." and "The log is empty..." in the middle pane.

## 5. Animation Orchestrator (WAAPI)
Stages are sequenced with `async/await`:
1. **Highlight (0.4s):** `body` indices get a violet background.
2. **Stack (0.6s):** repeat lines slide to the first iteration's position and fade.
3. **Fold (0.6s):** render SVG nodes at `opacity: 0`, clone stacked lines into the overlay, measure start and end with `getBoundingClientRect()`, animate the difference with `transform`.
4. **Land (0.3s):** show nodes, bounce the badge once, fade in the script.

## 6. Verification and Testing
| Feature | Test step | Expected result |
|---|---|---|
| Reduced motion | Emulate reduced motion, click Find | Chart and script appear instantly |
| Fallback mode | Set `TRAVEL_ENABLED = false`, click Find | Fade-and-draw plays without travel |
| Interrupt | Toggle domain during Fold | `cleanup()` runs, new sample shown |
| Cancel run | Switch domain during Run | Run stops, no console errors |
| No loop | Paste non-repeating text | "No repeated sequence found" message |
| Empty log | Clear textarea, click Find | "The log is empty" message |
| Domain switch | Switch after a Run | Middle and right panes clear, state is IDLE |
| Support sample | Load Support Tickets, click Find | Loop of 4 steps repeated 4 times |

## 7. Where It Runs
Local browser. Open `index.html` from the repo root.
