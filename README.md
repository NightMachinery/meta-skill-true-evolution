# Meta-Skill True Evolution

An agent skill that improves your other skills from the evidence of the runs
that use them. When a task that used a skill ends, the agent reviews what
happened: corrections, failed steps, workarounds, stale facts. It proposes
concrete edits to the skill's source, and it applies them once you confirm.

It is built around four rules:

- **Bikeshedding is dropped silently.** A lesson reaches you only if it
  happened in this run, would recur, would change what a future agent does, is
  not already in the skill, and earns the lines it costs. Rewording, reordering
  and nice-to-haves never reach you.
- **The skill is rewritten, not appended to.** Each lesson goes where its topic
  already lives, and the text it makes wrong is deleted. The skill holds no
  dates, incident stories, changelogs or "previously" notes. The evidence is
  shown to you when you confirm and is kept in the commit message, so the
  commit log is the skill's history.
- **A skill can opt in to evolving on its own.** With `evolution: auto` the
  agent applies obvious fixes itself, such as a corrected path, command or flag
  it verified in the run, and reports each one with its commit. Anything
  questionable still comes to you, as do changes to scope, safety, cost or
  privacy. So does any change to the evolution rules themselves.
- **Private information stays out.** Before writing, the agent checks the new
  text for credentials, other people's details, hostnames and identifiers,
  private paths, costs and security posture, and checks whether the skill's
  repository is public. When a private detail is genuinely needed, it asks you,
  with options: an environment variable, a local file outside the repository, a
  private companion skill, or writing it in when the repository is private.

## Install

```sh
npx skills add NightMachinery/meta-skill-true-evolution --global \
  --agent claude-code codex --skill meta-skill-true-evolution
```

For local development, symlink `skills/meta-skill-true-evolution/` into each
agent's skills directory instead.

## Turning it on

The skill's description lets an agent pick it up by itself at the end of a
task. To make that reliable, add one line to your global agent instructions
(`~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`, ...), so that no skill needs its
own copy of the reminder:

```markdown
At the natural end of a task in which you used a skill, run the
`meta-skill-true-evolution` skill on it.
```

## Declaring a skill's evolution mode

In the skill's frontmatter, under the Agent Skills `metadata` map:

```yaml
---
name: my-skill
description: ...
metadata:
  evolution: auto   # or confirm (the default), or off
---
```

- `confirm` proposes every change and applies only what you approve.
- `auto` applies obvious fixes and asks about the rest.
- `off` leaves the skill alone.

Your own instructions override the declaration in either direction. Skills
whose source is not in a repository you control are treated as `off`. For
those, the agent suggests a local overlay or drafts an upstream issue instead.

## Prior work

These projects take on the same problem, and this skill borrows from several
of them:

- [anthropics/skills](https://github.com/anthropics/skills): the
  `skill-creator` skill improves a skill through evals and human review of each
  iteration.
- [hansvangent/reflect-skill-claude](https://github.com/hansvangent/reflect-skill-claude):
  a `/reflect` command that proposes configuration changes. It has a "what not to
  propose" list and a short reply grammar for approving them.
- [BayramAnnakov/claude-reflect](https://github.com/BayramAnnakov/claude-reflect):
  hooks that capture corrections and route them to CLAUDE.md files or to the
  command they concern.
- [unisone/self-improving-skills](https://github.com/unisone/self-improving-skills):
  hooks that observe skill runs, with commands to amend, evaluate and roll back.
- [blader/Claudeception](https://github.com/blader/Claudeception): extracts
  new skills from a session's non-obvious fixes.

What this skill adds to them:

- every run of every skill can end in proposals for that skill;
- a skill can declare its own evolution mode;
- an explicit test for bikeshedding;
- rewriting instead of appending, with git as the only history;
- privacy checks that go beyond a credential pattern match.

## License

MIT.
