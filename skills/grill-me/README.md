# grill-me

A slash command that starts a relentless interview to sharpen a plan or design.

## Source

This skill comes from Matt Pocock's skills repository:
<https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me>

## Usage

```text
/grill-me
```

`grill-me` does no work itself. It calls the [`grilling`](../grilling/SKILL.md) skill, which runs the interview. Install both together.

Only you can start it. Claude never invokes it on its own (`disable-model-invocation: true`). To have Claude start a grilling session when you ask for one in plain language, use `grilling` directly.
