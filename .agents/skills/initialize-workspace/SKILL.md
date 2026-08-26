---
name: initialize-workspace
description: Grill the operator to turn a configured agent repository into a durable working workspace.
---

# Initialize Workspace

Extend Matt's repository setup with the operator, repository, workflow, and
credential context it does not collect.

## Precondition

Require `docs/agents/issue-tracker.md` and `docs/agents/domain.md`; when `triage`
is installed, also require `docs/agents/triage-labels.md`. If any are missing,
stop and tell the user to run `$setup-matt-pocock-skills`, then invoke this skill
again in the same task. Do not reproduce that skill's setup flow.

## Prepare

Read the root instructions, Matt's generated `docs/agents/` configuration, and
any partial initialization output. Inspect Git state, remotes, child
repositories, source systems, integrations, safe authentication metadata, and
existing repository-, team-, or user-provided workflow skills. Read available
skills instead of asking the user to summarize them; record unavailable sources
as blockers.

Before the first question, explain where settled information will go:

- operating rules: `AGENTS.md`;
- workspace facts: `docs/agents/workspace.md` using
  [the workspace profile](references/workspace-profile.md);
- domain language and qualifying decisions: the locations configured by Matt's
  setup and maintained through `domain-modeling`;
- credential policy and safe metadata: `docs/agents/credentials.md` using the
  [credential configuration format](../credentials/references/credential-config.md);
- specs and tickets: the configured tracker;
- secret values: never in chat or Git.

Tell the user that settled context is written during the interview, not saved
as a transcript afterward.

## Grill

Use `grilling` and `domain-modeling`. Do not ask about technical ability or
whether the agent may use Git, code, scripts, APIs, tests, or browsers; full
capability is the baseline.

First agree the finish line for this initialization. Configure enough context
to run the first real workflow; defer unrelated, non-blocking branches to Open
setup items instead of exhaustively mapping the operator's entire job.

Resolve only these workspace-specific branches:

1. The operator's recurring responsibilities, outcomes, and expensive manual
   work.
2. Repositories and external systems, including which source is authoritative
   for each subject and how conflicts are resolved.
3. Existing workflow skills and instructions: provenance, current owner, scope,
   authority, overlap, and conflicts. Decide whether each relevant artifact is
   adopted, adapted, replaced, or left as a blocker.
4. Artifact contracts: audience, canonical home, required format, review, and
   completion signal. Keep human-facing requirements separate from
   agent-execution specs and tickets.
5. Recurring workflows from trigger through evidence, decision, artifact, Git
   action, validation, and human handoff. Identify repeated operations that
   justify a script or skill without creating one outside the current scope.
6. Repository-specific branch, commit, pull-request, merge, and worktree
   conventions.
7. Credential bootstrap and access for every required integration. Invoke
   `credentials`; establish the human root of trust, durable machine-secret
   store, local materializations, safe validation, browser mode, and recovery
   route before declaring an integration ready.
8. Language and communication conventions for each audience.

## Persist

Write a decision as soon as it settles:

- update `docs/agents/workspace.md` from the profile format;
- create `docs/agents/credentials.md` from the credential format;
- add exact child-repository paths to the parent `.gitignore`;
- add only behavior-changing rules and context pointers to `AGENTS.md`;
- let `domain-modeling` update its own glossary and ADR locations.

Keep project-specific rules in the owning child repository. Preserve existing
edits, update named sections instead of appending duplicates, and create no
speculative backlog, notes, templates, PRD, or scripts directory.

If interrupted, mark the Workspace profile `initialization in progress`, keep
settled content, and leave unresolved decisions under Open setup items. Resume
from that frontier on the next invocation.

## Complete

Finish only when:

- the workspace and credential profiles contain no secret values or unresolved
  decisions hidden as assumptions;
- every relevant existing workflow skill is classified rather than silently
  trusted;
- the credential bootstrap is usable, and each required local materialization
  has a durable source or an explicit recoverable exception;
- child repositories and instruction ownership are unambiguous;
- integrations are validated or listed as safe blockers;
- instructions have no duplicate owners or contradictions;
- the user confirms the final summary and diff.

Mark the Workspace profile `initialized`, then commit. Do not push, open a pull
request, or merge unless asked.
