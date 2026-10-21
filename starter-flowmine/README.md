# FlowMine

Paste a log of repetitive work. FlowMine finds the loop hiding in it, draws the workflow as a flowchart, lets you run it, and generates an automation script.

## Run
Open `index.html` in a browser, or serve the folder: `python3 -m http.server 8000`. No install or build step.

## How it works
1. Each log line is cleaned (timestamps removed) and turned into a template (`report_jan.xlsx` becomes `{file}`).
2. A search finds the contiguous group of steps that repeats the most.
3. Steps that change each run (amber dot) become value lists in the generated script.

## Hackathon (Build With AI: Basics)
This starter does not replace the required planning docs. Run the Devpost Learn skills in your agent and keep `scope.md`, `prd.md` and `spec.md` in this repo.
