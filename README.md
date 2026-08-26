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
- A `credentials` skill that makes safe, resumable authentication a required
  part of workspace initialization and later credential work.
- An `initialize-workspace` skill that adds operator, repository, workflow,
  existing-instruction, and credential context after Matt's setup configures
  the engineering skills.

The upstream `in-progress`, `misc`, and deprecated skills are intentionally not
included. They are not part of Matt's published stable set and may be
special-purpose, unstable, or non-functional.

## First use

1. Clone this repository onto the target Mac.
2. Open this folder as a local project in ChatGPT Desktop and trust the project.
3. From a local terminal, create the personal credential vault:

   ```bash
   .agents/skills/credentials/scripts/setup-personal-gcp
   ```

4. Start a fresh task and invoke `$setup-matt-pocock-skills` to configure the
   tracker, triage labels, and domain-document layout.
5. In the same task, invoke `$initialize-workspace`.
6. Work through the grilling rounds. The agent will inspect repositories,
   systems, and existing team-provided skills itself; ask only for actual
   decisions; and write settled context as the conversation progresses.
7. Review the final diff and let the agent commit the initialized workspace.

Initialization explains the storage model before asking the first question:

- `AGENTS.md` contains durable operating instructions.
- `docs/agents/workspace.md` maps responsibilities, repositories, sources of
  truth, recurring workflows, and non-secret integration facts.
- `docs/agents/credentials.md` records the non-secret GCP Secret Manager,
  naming, browser-opening, local-materialization, and recovery policy.
- `CONTEXT.md` is created lazily and contains only canonical domain language.
- `docs/adr/` is created lazily for consequential, surprising, hard-to-reverse
  decisions that involved a real trade-off.
- Specs and tickets live in the configured issue tracker, not in an invented
  local backlog.
- Secrets never enter this repository.

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

## Independent child repositories

This control repository may contain other Git repositories as children. During
initialization, the agent records each exact path in `.gitignore`. Each child
keeps its own remote, history, branches, worktrees, instructions, checks, and
pull requests. Git commands must run from the affected repository root.

Do not use broad cleanup commands from the control repository while ignored
child repositories are present. An ignored nested repository is still valuable
data even though the parent Git repository does not track it.

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

The vendored source and commit are recorded in
`THIRD_PARTY_NOTICES.md`. Review upstream changes before replacing the local
copies; `initialize-workspace` and `credentials` are separate local skills and
must not be overwritten by an upstream update.
