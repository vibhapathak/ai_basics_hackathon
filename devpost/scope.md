---
doc: scope
status: approved
---

# FlowMine

A visualizer that finds and collapses repetitive activity logs into interactive automation flowcharts.

## The Unique Kernel
The "Collapse Animation": A long, messy list of manual log steps physically folds and compresses into a single, clean loop block. The ×4 badge (pre-existing in the prototype) serves as the destination for the folding lines, appearing as they "arrive" to turn cognitive overload into visual order.

## Who It's For
An operations or finance staffer who handles repetitive computer chores (like monthly reports or support ticket triage) and sees the work as a "slog" but hasn't yet visualized it as a repeatable, automatable workflow.

## The Core Loop
The user chooses a domain (Monthly Report or Support Tickets), watches the messy log lines animate and fold into a loop node, and then sees the generated script appear. They then click "Run" (pre-existing) to watch the flow pulse and the loop badge tick.

## Inspiration & Identity
- **Aesthetic:** Clean light-grey grid background with a navy-and-violet color palette. 
- **Mood:** Satisfying, orderly, and empowering. It should feel like "compressing the mess."
- **Structure:** Three-pane layout: Raw Log (Left), Flowchart (Middle), Generated Script (Right).

## Why This Matters to the Learner
The learner wants to master spec-driven UI development, specifically learning how to describe complex, state-driven animated interactions so clearly that an agent can build them correctly on the first try.

## What "Working" Looks Like
A user switches to the "Support Ticket" domain, clicks "Process Log," and watches the log lines animate and fold into the pre-existing loop node. Once the animation finishes, the generated script appears on the right. They then click "Run," seeing the flow pulse through the steps while the "×4" badge ticks "1/4, 2/4..." as the simulated automation handles each ticket.

## The POC Boundary
- **In:** New "Collapse Animation" logic (vanilla JS) that feeds into the pre-existing badge.
- **In:** Domain switching logic for two samples (Monthly Report, Support Tickets).
- **In:** Script appearance triggered by animation completion.
- **In:** Refined navy-and-violet styling on a grid background.

## Later
- Draggable nodes (if time permits, but currently first on the cut list).
- Exporting the generated Python script to a file.

## Explicitly Cut
- **No real-time recording:** The logs are hardcoded samples, not captured live.
- **No real execution:** The Python script is text-only; it doesn't actually run code.
- **No accounts/saving:** It's a local-only, single-session prototype.
- **No build tools:** Strictly vanilla HTML/CSS/JS as per the constraint.
