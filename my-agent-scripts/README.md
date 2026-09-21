# agent-scripts

Reusable agent definitions — role prompts that give an AI coding assistant a single, consistent
job: auditing, building, judging, and so on. Each one is a plain Markdown file with YAML
front-matter, so it can be dropped into `.claude/agents/` (Claude Code subagents), used as a
system prompt, or pasted into any assistant that takes one.

## Agents

| Agent | Job |
|---|---|
| [`agents/auditor.md`](agents/auditor.md) | Reviews a project for flaws and checks it against its own requirements. Reports with evidence; does not build. |
| [`agents/builder.md`](agents/builder.md) | Fixes the problems an auditor found and adds requested features, with minimal changes and verified results. |

Planned: `judge`.

## Using one

**As a Claude Code subagent** — copy it into the project (or your user config):

```bash
mkdir -p .claude/agents && cp agents/auditor.md .claude/agents/
```

Then ask for it by name, e.g. "use the auditor agent to review the evaluation reports".

**As a system prompt** — paste the file's body (everything below the front-matter) into whatever
assistant you're using.

## Writing a new one

Keep each agent to one job and make it specific enough to change behaviour:

- **Front-matter**: `name` and a one-line `description` of when to use it.
- **Boundaries**: what the agent must *not* do. This is what stops an auditor from quietly
  becoming a builder.
- **Method**: numbered steps in the order they should happen.
- **Recurring patterns**: the failure modes worth checking first, learned from real use.
- **Output**: the shape of the deliverable.

Write from experience. Every line in `auditor.md` traces back to a real finding it would have
caught earlier.
