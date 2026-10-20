# Design

The skill file is the specification. This page explains its choices.

## Evolution modes

A skill declares its mode in its frontmatter, under the Agent Skills `metadata`
map:

```yaml
---
name: my-skill
description: ...
metadata:
  evolution: auto   # or confirm (the default), or off
---
```

- `confirm` proposes every change and applies only what you approve.
- `auto` applies obvious fixes, reports each one with its commit, and asks
  about the rest.
- `off` leaves the skill alone.

Your own instructions override the declaration in either direction. A skill
whose source is not in a repository you control is treated as `off`. For such
a skill, the agent suggests a local overlay in your instructions or drafts an
upstream issue instead.

An **obvious** change fixes or adds a fact that the run verified, such as a
path, command, flag, version or link, or a pitfall together with the workaround
that worked. It must also be small, fit the skill's purpose, and contain
nothing private. Everything else is **questionable**, and always comes to you:

- changes to a skill's scope or trigger;
- changes to safety rules, permissions, cost or scale;
- removing a rule the run did not prove false;
- restructuring;
- anything private;
- any change to the `evolution` field or to this skill. This means auto mode
  cannot loosen its own limits.

## Why history stays out of the skill

Every line of a skill is read on every run. A dated note, an incident story or
a "previously X, now Y" costs tokens forever and teaches nothing that the rule
it explains does not already say. So the evidence for each change is shown
when you confirm it and kept in the commit message, and `git log -p` on the
skill is its history. Two things that look like history may stay: a short
reason that makes a rule easier to follow, and a scope condition such as "as of
tool 0.10" that tells a future agent when to recheck.

## Private information

When a skill genuinely needs a private detail, the agent asks you where it
should live, and recommends one of these:

1. An environment variable that the skill names.
2. A local file outside the repository, which the skill reads when it exists.
3. A private companion skill or repository.
4. The skill itself, only if its repository is private and will stay so.

Credentials never go into a skill or into a file the agent creates.

## Prior work

- [anthropics/skills](https://github.com/anthropics/skills): `skill-creator`
  improves a skill through evals and human review of each iteration. It keeps
  its evidence in a workspace beside the skill, and warns against overfitting.
- [hansvangent/reflect-skill-claude](https://github.com/hansvangent/reflect-skill-claude):
  `/reflect` proposes configuration changes. This skill borrows its "what not to
  propose" list and its `apply 1,3` reply grammar.
- [BayramAnnakov/claude-reflect](https://github.com/BayramAnnakov/claude-reflect):
  hooks capture corrections, and `/reflect` routes them to CLAUDE.md files or to
  the command they concern. Its own backlog found that most captured items
  were noise, and that a noisy queue stops being reviewed.
- [unisone/self-improving-skills](https://github.com/unisone/self-improving-skills):
  hooks observe skill runs, and commands amend, evaluate and roll back a skill.
  Its amendments are capped in size and count.
- [blader/Claudeception](https://github.com/blader/Claudeception): extracts new
  skills from a session's non-obvious fixes, without review.

What none of them does, and this skill does:

- end each run of a skill with proposals for that skill;
- let a skill declare its own evolution mode;
- test explicitly for bikeshedding;
- rewrite rather than append, with git as the only history;
- handle privacy beyond matching credential patterns.
