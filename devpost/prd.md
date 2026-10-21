---
doc: prd
status: approved
---

# FlowMine — Product Requirements

A visualizer that finds and collapses repetitive activity logs into interactive automation flowcharts.
Source: `scope.md > The Unique Kernel`, `scope.md > The Core Loop`.

## The Core Journey
The "compressing the mess" journey starts with a cluttered log and ends with a clean, running automation.

1. **Arrival:** User opens the app. The "Monthly report" domain is pre-selected, its sample log is visible in the left pane, and the flowchart/script panes show helpful empty states.
2. **Switching Context:** User can toggle to "Support tickets" to see a different domain; the UI clears any previous results to stay fresh.
3. **The Transformation:** User clicks "Find the loop." They watch a 2.5s animation where the messy log lines highlight, stack, and fold into a clean flowchart node with a progress badge.
4. **The Reveal:** As soon as the animation lands, the generated Python automation script fades into the right pane.
5. **Simulated Execution:** User clicks "Run." The flowchart pulses to show the active step, the loop badge ticks through its iterations (1/4, 2/4...), and an output panel prints "live" actions.
6. **Success:** The flow completes, steps turn green, and the user sees a "Done" message, proving the mess has been mastered.

## Screens and Layout
A single-surface, three-pane dashboard layout:

- **Left Pane (Log Editor):** Features the domain toggle (Monthly report / Support tickets) at the top. Below is the editable log area and the "Find the loop" primary action button.
- **Middle Pane (Flowchart):** A grid-backed area for the SVG flowchart. Shows "Choose Find the loop to see the pattern" initially. Includes the "Run" button (disabled until a loop is found).
- **Right Pane (Script & Console):** The top section displays the generated Python script. A small output panel sits at the bottom to show "Run" logs.

## Look and Feel
- **Palette:** Navy text/elements, Violet for loops and active states, Amber for variables (changing data in logs), and Green for completion.
- **Background:** Light-grey grid pattern to suggest a canvas/workspace.
- **Typography:**
  - **Serif:** Application title ("FlowMine").
  - **Monospace:** Log lines, flowchart node labels, generated script, and output console.
  - **Sans-serif:** Buttons, headings, toggles, and UI instructions.
- **Motion:** Intentional, staged animation (2.5s) to visualize the data transformation.

## Features and Behavior

### 1. Pattern Detection & Animation
`scope.md > The Unique Kernel`
- **Logic:** Detects a sequence of log steps that repeats at least twice in a row.
- **Animation Stages (2.5s total):**
  - **Highlight (0.4s):** Repeating lines get a violet highlight.
  - **Stack (0.6s):** Repeated groups slide and overlap into a single stack, then fade.
  - **Fold (0.6s):** The stack shrinks into a loop node and moves to the middle pane; setup/closing nodes and arrows draw in.
  - **Land (0.3s):** The "×4" badge pops onto the loop node with a single bounce.
- **Animation Fallback:** If the full text-travel animation is too complex for the build phase, use a simpler fade: lines fade out on the left, the flowchart draws into the middle, and the badge lands last.
- **Accessibility:** If "Reduce Motion" is enabled in the OS, the animation is skipped and the chart/script appear immediately.

### 2. Interactive "Run" Simulation
- **Active Step:** The current step in the flowchart glows violet with a thicker outline.
- **Progress Tracking:** The loop badge (e.g., ×4) ticks "1/4", "2/4" etc. as the loop pulses.
- **Completion:** Finished steps turn green. The final state shows a "Done" message in the output panel and resets the badge to the total count.
- **Interlocks:** The "Run" button is disabled during animation and while a run is already in progress.

### 3. Domain Management
- **Sample Data:** "Monthly report" and "Support tickets" are hardcoded.
- **State Clearing:** Switching domains or editing the log clears the middle and right panes to prevent stale data.

## States and Boundaries

- **Initial State:** "Monthly report" pre-loaded to ensure the user sees data immediately.
- **Empty Log:** Shows message: "The log is empty. Paste one action per line, or switch to a sample."
- **No Loop Found:** Shows message: "No repeated sequence found. FlowMine needs a group of steps that appears at least twice in a row. Add more of the log and try again."
- **Animating/Running:** UI buttons are disabled to prevent state conflicts.
- **Persistence:** None; the app resets on refresh (single-session prototype).

## Product Decisions

- **Pre-loaded sample:** To make the demo video/first use immediately engaging without requiring user input.
- **2.5s Animation:** Long enough to be satisfying and "read" the movement, but fast enough to fit in a short demo video.
- **No Typing Effect:** The script fades in all at once to keep the user's focus on the result and maintain a fast demo pace.
- **Monospace vs Sans-serif:** Used to distinguish between "system data/code" and "user interface" elements.
- **Run Button Placement:** Stays in the middle pane (Flowchart area) to keep the interaction focused on the visual flow.
- **Reset Behavior:** "Find the loop" acts as the reset for any existing state.

## What We're Building
- Domain toggle and sample data loader.
- Log editor with "Find the loop" trigger.
- 4-stage collapse animation logic (with fade fallback).
- SVG Flowchart generator (logic to turn logs into nodes/loops).
- "Run" simulation with pulsing and progress tracking.
- Script generation (dynamic templates based on detected loop logic, supporting custom logs).

## Deferred From the POC
- **Draggable Nodes:** Cut to prioritize the collapse animation.
- **Exporting Script:** Users can copy-paste the text, but file export is not in scope.

## Non-Goals
- **Real-time Log Recording:** The app uses static log text only.
- **Real Code Execution:** The Python script is a text-based simulation.
- **User Accounts:** No saving or cloud features.

## Open Questions
- None. (Resolved: Run button in middle pane; Find the Loop acts as reset).
