# Agent workspace instructions

This repository is a portable control workspace for operating across one or
more independent repositories. Keep it generic until the initialization
workflow records actual workspace facts.

## First-time initialization

- Before `$initialize-workspace`, run `$setup-matt-pocock-skills` when its
  `docs/agents/issue-tracker.md`, `domain.md`, or applicable `triage-labels.md`
  output is missing.
- When the Workspace profile says `not initialized`, run
  `$initialize-workspace` before the first substantive task.
- Resume partial initialization from the files already written. Do not restart
  the interview or discard settled decisions.
- Do not invent a role, repository layout, tracker, source system, workflow, or
  project-specific skill before initialization establishes it.

## Repository scope

- Treat only repositories explicitly named by the user or recorded for the
  current workflow as task scope.
- Read and follow the closest instructions in every affected child repository.
  Child instructions and validation boundaries override this control
  repository for files inside that child.
- Run Git, build, test, and repository-specific commands from each affected
  repository root.
- Keep nested repositories independent. Add their exact paths to this
  repository's `.gitignore`; never rely on blanket patterns that could hide
  unrelated workspace files.
- Never run a broad Git clean from this control repository while ignored child
  repositories are present.

## Durable context

- `AGENTS.md` contains operating rules that should change future agent
  behavior. Keep facts and long explanations in the pointed-at document that
  owns them.
- `docs/agents/workspace.md` is the factual map of responsibilities,
  repositories, systems of record, recurring workflows, artifact contracts,
  and non-secret integrations.
- `CONTEXT.md` is a living domain glossary only. Create it lazily when the first
  canonical term is settled; never use it as a plan, transcript, or technical
  specification.
- `docs/adr/` contains only consequential, non-obvious, hard-to-reverse
  decisions made through a real trade-off. Create it lazily.
- Issue-tracker mechanics, triage labels, and domain-document layout live under
  `docs/agents/` after initialization.
- Put every durable fact or rule in one canonical home and link to it elsewhere.
  Do not duplicate instructions across files.
- Do not save chat transcripts or speculative answers as durable context.

## Capabilities and workflow

- Do not reduce the available workflow because the operator is not a software
  developer. Use code, scripts, API calls, tests, Git branches, worktrees,
  commits, pull requests, and merges whenever they are the best tools for the
  task.
- Use `$ask-matt` when choosing among the installed workflows.
- Treat human-facing requirement documents and agent-facing execution specs as
  separate artifacts. Follow the initialized workspace contract for each; do
  not let one silently replace the other.
- Work discovered outside the current request belongs in the configured issue
  tracker rather than an invented backlog directory.

## Credentials

- Before any password, token, API key, SSH key, provider login, or secret-store
  work, read and follow `.agents/skills/credentials/SKILL.md`.
- After initialization, read `docs/agents/credentials.md` for the workspace's
  non-secret GCP project, naming, materialization, browser, and recovery policy.
- Secret values belong only in a provider UI, hidden terminal prompt, or
  approved secret store. Never place them in chat, command arguments, logs,
  screenshots, tracked files, or non-secret inventories.
- Google Secret Manager is the default durable store for exportable machine
  secrets. Local `.env`, Keychain, CLI, SSH, mounted, and CI copies are
  operational materializations unless a recorded exception says otherwise.

## Workspace profile

Status: **not initialized**

Run `$initialize-workspace`. This section will then point to the completed
workspace profile and the skill configuration created during that session.
