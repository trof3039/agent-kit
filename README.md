# Agent Kit

This repository is a portable control workspace for doing real work with an
agent across one or more independent Git repositories. It is not a reduced
"non-programmer" environment: the agent can use Git, worktrees, code, scripts,
APIs, tests, browsers, and external tools whenever the task calls for them.

The template deliberately contains no assumptions about a person's role, a
company, a tracker, a documentation system, or a particular project. Two
explicit setup steps in one task establish those facts without duplicating
instructions.

## What is included

- Project-scoped Codex configuration with no approval prompts and full local
  filesystem/network access.
- The complete stable engineering and productivity skill set from
  [Matt Pocock's skills](https://github.com/mattpocock/skills), vendored at a
  known upstream commit.
- An `initialize-workspace` skill that adds operator, repository, workflow, and
  existing-instruction context after Matt's setup configures the engineering
  skills.

The upstream `in-progress`, `misc`, and deprecated skills are intentionally not
included. They are not part of Matt's published stable set and may be
special-purpose, unstable, or non-functional.

## Before you start

You need exactly two things:

- Codex, as ChatGPT Desktop or the `codex` CLI. It is what loads
  `.agents/skills/` and `.codex/config.toml`.
- `git`.

There is nothing else to install and no account to register for. A clone plus
the two setup steps below is a working workspace.

Two things are optional, and only where you want that feature:

- The [`gh` CLI](https://cli.github.com), authenticated, to keep issues in
  GitHub Issues.
- The [`glab` CLI](https://gitlab.com/gitlab-org/cli), authenticated, to keep
  them in GitLab.

Without either, setup offers a local Markdown tracker under `.scratch/` that
needs no account and no network. Setup asks before it needs one of these tools,
so an unavailable tool narrows a feature instead of blocking initialization.

## Invoking a skill

In Codex, type `$` and pick a skill from the list, or name it in the prompt:
`$initialize-workspace`. `/skills` shows everything installed.

The vendored skill text writes skill names as `/name`, which is Claude Code's
syntax; upstream publishes to both harnesses. Read those as plain labels for
skills you can pick. In Codex the trigger is always `$name`.

## First use

1. Clone this repository, then open it as a local project in ChatGPT Desktop or
   run `codex` inside it from a terminal. Trust the project: Codex applies
   `.codex/config.toml` only to a trusted project.
2. Start a fresh task and invoke `$setup-matt-pocock-skills` to configure the
   tracker, triage labels, and domain-document layout.
3. In the same task, invoke `$initialize-workspace`.
4. Work through the grilling rounds. The agent will inspect repositories,
   systems, and existing team-provided skills itself; ask only for actual
   decisions; and write settled context as the conversation progresses.
5. Review the final diff and let the agent commit the initialized workspace.

Initialization explains the storage model before asking the first question:

- `AGENTS.md` contains durable operating instructions.
- `docs/agents/workspace.md` maps responsibilities, repositories, sources of
  truth, recurring workflows, and non-secret integration facts.
- `CONTEXT.md` is created lazily and contains only canonical domain language.
- `docs/adr/` is created lazily for consequential, surprising, hard-to-reverse
  decisions that involved a real trade-off.
- Specs and tickets live in the configured issue tracker, not in an invented
  local backlog.

If either setup step is interrupted, invoke that skill again. Both resume from
the files already written instead of discarding settled progress.

## Normal work

Invoke `$ask-matt` whenever the next workflow is unclear. The common path is:

```text
optional wayfinder
    -> grill-with-docs
    -> optional research / prototype
    -> to-spec
    -> to-tickets
    -> implement
    -> review, commit, and PR
```

For a small change, `grill-with-docs -> implement` may be enough. For a large
effort whose route is not yet visible, `wayfinder` maps decision work before the
main flow begins.

A human-facing requirements document and an agent-facing execution spec are
different artifacts. A project-specific skill may create or edit the former;
`to-spec` and `to-tickets` create the latter so agents can plan and execute the
work. Initialization inspects existing project-specific skills, establishes
whether they are current and authoritative, and records how both artifact types
fit together for the actual project.

## Access and secrets

This kit mandates no secret store and ships no credential policy. Record how
each integration is reached, and by which account, in the Integrations table of
`docs/agents/workspace.md`; keep the values themselves out of the repository.

When a step needs a human, the vendored `wizard` skill generates an interactive
script that opens each URL, says what to click, captures values through hidden
prompts, and writes them into a `.env` file that `.gitignore` already excludes.
That is the default path, and it needs no external secret manager. A workspace
that has one can record it as its own convention during initialization.

## Independent child repositories

This control repository may contain other Git repositories as children. Each
child keeps its own remote, history, branches, worktrees, instructions, checks,
and pull requests, and initialization records each exact path in `.gitignore`.
An ignored nested repository is still valuable data even though the parent Git
repository does not track it. `AGENTS.md` owns the operating rules that follow
from this.

## Permission model

The checked-in `.codex/config.toml` requests:

```toml
approval_policy = "never"
sandbox_mode = "danger-full-access"
```

Codex applies project configuration only after the project is trusted. This is
intentionally a high-autonomy setup: use it only on a machine and with
repositories the operator is prepared to let the agent modify. Organization
policy may still override these settings. See the official
[Codex configuration documentation](https://learn.chatgpt.com/docs/config-file/config-basic).

## Updating the vendored skills

The vendored source and commit are recorded in `THIRD_PARTY_NOTICES.md`. Review
upstream changes before replacing the local copies. `initialize-workspace` is
local to this repository and is not replaced by a vendored upstream update.
