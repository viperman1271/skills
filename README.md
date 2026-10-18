# skills

A collection of [Agent Skills](https://docs.claude.com/en/docs/claude-code/skills) for Claude Code.

## Skills

| Skill | Invocation | What it does |
| --- | --- | --- |
| [`grilling`](skills/grilling/SKILL.md) | Automatic, or `/grilling` | Interviews you relentlessly about a plan, decision, or idea until you and Claude share an understanding of it. Claude invokes it when you want to stress-test your thinking or use a "grill" phrase ("grill me on this"). |
| [`grill-me`](skills/grill-me/SKILL.md) | `/grill-me` only | A shortcut that starts `grilling`. Claude never invokes it on its own (`disable-model-invocation: true`). |

### How `grilling` works

- **Design tree.** Claude maps the plan as a tree of decisions, where each decision branches into the ones that depend on it.
- **Rounds.** Each round, Claude asks every question on the *frontier*: the decisions whose prerequisites are already settled. Then it waits for your answers and works out the next frontier.
- **Structured questions.** Every question goes through the `AskUserQuestion` tool, with 2–4 concrete options. Claude's recommended option comes first and is labeled "(Recommended)".
- **Facts vs. decisions.** Claude looks up facts itself, sending sub-agents to explore the environment, and only asks you for decisions. Questions that don't depend on a running lookup aren't held up by it.
- **Finishing.** When the frontier is empty, Claude summarizes the settled decisions and asks you to confirm before acting on the plan.
- **Fallback.** If `AskUserQuestion` isn't available (headless or scheduled runs), Claude asks numbered plain-text questions and marks its recommendation with ➡️.

## Installation

Claude Code loads skills from `~/.claude/skills/` (personal, for every project) and `.claude/skills/` (one project only). Each skill is a folder containing a `SKILL.md`.

Clone the repository:

```bash
git clone https://github.com/viperman1271/skills.git
```

Copy the skills into your personal skills directory:

```bash
mkdir -p ~/.claude/skills && cp -r skills/skills/* ~/.claude/skills/
```

To use them in a single project, copy them into that project's `.claude/skills/` instead. Install `grilling` and `grill-me` together, since `grill-me` only forwards to `grilling`.

## Usage

Describe your plan and ask Claude to grill you:

```text
I'm planning to move our job queue from Redis to Postgres. Grill me on it.
```

Or start a session explicitly:

```text
/grill-me
```

## Adding a skill

1. Create `skills/<skill-name>/SKILL.md`.
2. Add YAML frontmatter with at least `name` and `description`. The description is what Claude reads to decide when to use the skill, so say what it does and when to trigger it.
3. Write the instructions in Markdown below the frontmatter.
4. Add a row for it to the table above.
