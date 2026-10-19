# grilling

Claude interviews you relentlessly about a plan, decision, or idea until you both understand it the same way.

## Source

This skill comes from Matt Pocock's skills repository:
<https://github.com/mattpocock/skills/tree/main/skills/productivity/grilling>

## Usage

Claude starts it on its own when you want to stress-test your thinking or use a "grill" phrase:

```text
I'm planning to move our job queue from Redis to Postgres. Grill me on it.
```

You can also start it with `/grilling`, or with the [`grill-me`](../grill-me/README.md) shortcut.

## How it works

- **Design tree.** Claude maps the plan as a tree of decisions, where each decision branches into the ones that depend on it.
- **Rounds.** Each round, Claude asks every question on the *frontier*: the decisions whose prerequisites are already settled. Then it waits for your answers and works out the next frontier.
- **Structured questions.** Every question goes through the `AskUserQuestion` tool, with 2–4 concrete options. Claude's recommended option comes first and is labeled "(Recommended)".
- **Facts vs. decisions.** Claude looks up facts itself, sending sub-agents to explore the environment, and only asks you for decisions.
- **Finishing.** When the frontier is empty, Claude summarizes the settled decisions and asks you to confirm before acting on the plan.
- **Fallback.** If `AskUserQuestion` isn't available (headless or scheduled runs), Claude asks numbered plain-text questions and marks its recommendation with ➡️.

While the skill is active, Claude can use the `AskUserQuestion` and `Agent` tools without a permission prompt (`allowed-tools`).
