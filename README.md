# Meta-Skill True Evolution

Your agent skills go stale, and the usual fixes make them worse: a learnings
file that only grows, or an agent that rewrites its own instructions with
nobody checking. This skill turns each run of a skill into a short, reviewed
improvement to that skill's source.

At the end of a task that used a skill, the agent looks at what happened in the
run: your corrections, failed steps, workarounds and stale facts. It proposes
at most a few concrete edits, with the evidence, and you reply `apply 1,3` or
`skip`. The point is what it leaves out:

- **No bikeshedding.** Rewording, reordering and nice-to-haves are dropped
  before you see them. A lesson reaches you only if it happened in this run,
  would recur, and would change what a future agent does.
- **No cruft.** Lessons are written into the skill where they belong, and the
  text they make wrong is deleted. Dates, incident stories and changelogs stay
  out of the skill; the evidence goes in the commit message.
- **No leaks.** Credentials, other people's details, hostnames and the like are
  kept out of the skill. When a skill really needs one, the agent asks you
  where it should live instead.

A skill can also opt in to fixing itself: with `evolution: auto`, verified,
obvious fixes, such as a corrected command, are applied and reported. Anything
questionable still comes to you.

## Try it

```sh
npx skills add NightMachinery/meta-skill-true-evolution --global \
  --agent claude-code codex --skill meta-skill-true-evolution
```

Then add one line to your global agent instructions (`~/.claude/CLAUDE.md`,
`~/.codex/AGENTS.md`, ...):

```markdown
At the natural end of a task in which you used a skill, run the
`meta-skill-true-evolution` skill on it.
```

Use any skill, correct the agent once along the way, and see what it proposes
when the task ends.

## More

- [docs/design.md](docs/design.md): evolution modes and how a skill declares
  one, what counts as obvious, the privacy options, and the prior work this
  skill builds on.
- [The skill itself](skills/meta-skill-true-evolution/SKILL.md).

MIT licensed.
