# Agent workspace instructions

This repository is a portable control workspace for operating across one or
more independent repositories. Keep it generic until the initialization
workflow records actual workspace facts.

## First-time initialization

Both setup skills are user-invoked: no skill and no agent can reach them. Ask
the user to run the missing one and wait, rather than substituting your own
setup flow.

- When `docs/agents/issue-tracker.md`, `domain.md`, or the applicable
  `triage-labels.md` is missing, tell the user to run
  `$setup-matt-pocock-skills` first.
- When the Workspace profile says `not initialized`, tell the user to run
  `$initialize-workspace` before the first substantive task.
- Resume partial initialization from the files already written. Do not restart
  the interview or discard settled decisions.
- Do not invent a role, repository layout, tracker, source system, workflow, or
  project-specific skill before initialization establishes it. Treat existing
  team-provided workflow instructions as evidence until initialization records
  their scope, current owner, and authority.

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

## Access and secrets

- The Integrations table in `docs/agents/workspace.md` owns how each system is
  reached and under which account. Record the route there; never record a
  secret value in it or in any other tracked file.
- When capturing a value needs a human, use `wizard`. It writes what it
  captures into a gitignored `.env` rather than into the conversation.
- This workspace mandates no secret store. Follow one only after initialization
  records it as a convention, and follow a child repository's own credential
  policy for files inside that child.

## Workspace profile

Status: **not initialized**

Run `$initialize-workspace`. This section will then point to the completed
workspace profile and the skill configuration created during that session.
