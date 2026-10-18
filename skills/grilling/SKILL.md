---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
allowed-tools: AskUserQuestion, Agent
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round, then wait for the user's answers before the next round.

## Asking questions

You MUST ask every question with the **AskUserQuestion** tool. Never ask questions in plain text.

For each question:

- **question**: the full question, ending with "?". Include any context the user needs to decide.
- **header**: a short label for the decision, 12 characters max (e.g. "Storage", "Auth", "Scope").
- **options**: 2–4 distinct, concrete choices. Put your recommended answer **first** and add " (Recommended)" to its label. Use each option's `description` for a one-line trade-off. Don't add an "Other" option; the tool adds one automatically.
- **multiSelect**: `true` only when several choices can genuinely apply together; otherwise `false`.

The tool takes at most 4 questions per call. If the frontier has more than 4 questions, make several AskUserQuestion calls in the same round, ordered by importance, before moving to the next round.

If the user picks "Other" or adds notes, treat their text as the answer and fold it into the tree.

## Updating the tree

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

## Finishing

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Summarize the settled decisions, then confirm with one final AskUserQuestion call (options: "Yes, we're aligned (Recommended)" / "No, keep going"). Do not act on the plan until the user confirms.

## Fallback

If AskUserQuestion is unavailable (headless or scheduled runs), ask in plain text instead: number each question, list its choices, mark your recommended answer with ➡️, and word each question so "yes" accepts the recommendation.