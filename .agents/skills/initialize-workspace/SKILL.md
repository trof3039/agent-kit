---
name: initialize-workspace
description: Run the first guided grilling session that turns this portable template into a durable workspace profile.
---

# Initialize Workspace

Turn the uninitialized control repository into a concrete working environment
through one resumable grilling session. This is a state transition, not a
generic brainstorming exercise.

## Operating contract

Before acting:

1. Read the root `AGENTS.md` and any existing initialization output.
2. Use the `writing-for-agents`, `grilling`, and `domain-modeling` skills.
3. Use the templates alongside `setup-matt-pocock-skills` when writing issue
   tracker, triage-label, and domain-layout configuration. Integrate those
   decisions here; do not ask the user to leave this session and run another
   setup interview.
4. Use the `credentials` skill for every authentication or secret-related step,
   and `wizard` only for steps the agent genuinely cannot perform.

For credential work, the `credentials` policy overrides the wizard's generic
defaults. Follow the browser-opening mode recorded in
`docs/agents/credentials.md`: always display the URL and expected account;
automatically open it only when the workspace chose `automatic`.

Treat full local agent capability as the baseline. Do not ask whether the
operator is technical, whether the agent may write code, or whether it may use
Git. The project configuration has already selected high autonomy. Ask only for
real product, workflow, source-of-truth, and human-handoff decisions.

Find facts from the workspace and available tools yourself. Never ask the user
for a repository URL, current file layout, installed skill, remote, or tool
state that can be inspected safely.

## Round zero: inspect and explain

Inspect before asking the first question:

- initialization status and any partially written profile;
- the control repository, its remote, and Git state;
- independent child repositories and their closest instruction files;
- installed project skills and project Codex configuration;
- existing `CONTEXT.md`, ADRs, `docs/agents/`, specs, and ticket conventions;
- currently available integrations and their safe, non-secret authentication
  status.

Report the relevant facts. Then explain, in plain language, what this session
may write:

- stable behavior goes in `AGENTS.md`;
- workspace facts go in `docs/agents/workspace.md` using
  [the workspace profile format](references/workspace-profile.md);
- canonical domain terms go in `CONTEXT.md` as soon as they are settled;
- qualifying architectural or workflow decisions go in `docs/adr/`;
- tracker mechanics go in `docs/agents/issue-tracker.md`, label mappings in
  `docs/agents/triage-labels.md`, and domain layout in
  `docs/agents/domain.md`;
- non-secret credential storage, naming, browser, and recovery policy goes in
  `docs/agents/credentials.md`;
- specs and tickets go to the configured tracker;
- secret values are never written to any of these places.

Make clear that the session records settled context while it proceeds, not as a
transcript dump at the end.

## Grill the workspace into focus

Use the `grilling` design-tree method: ask the entire current frontier in a
numbered round, give a recommended answer for every decision, wait, then
recompute the frontier. Do not ask downstream questions whose prerequisites are
still unsettled.

Cover every branch below, but let inspected facts collapse questions that are
already answered.

### Purpose and working reality

Settle the operator's actual role, recurring responsibilities, repeated pain,
and outcomes they own. Separate what happens every week from exceptional work.
Identify where time is lost to slow manual interaction, copying, formatting,
searching, testing, or coordination so later automation has a concrete target.

### Repositories and sources of truth

Map each repository and external system by what it is authoritative for. For
conflicting sources, settle precedence explicitly: for example, whether chat is
an input, a decision record, or merely a notification channel. Determine which
repositories belong inside the control workspace and add only their exact paths
to `.gitignore`.

Do not copy child-repository rules into the root. Put common rules here and
project-specific rules in the owning child repository.

### Artifact contracts

For every important artifact, settle its audience, canonical home, required
format, review path, and completion signal.

Explicitly distinguish:

- human-facing requirements or documentation changes used by a team; and
- agent-facing specs and tickets used to plan and execute work.

When both exist, map how one feeds the other without making them the same
artifact. Inspect any organization-specific writing skill before adopting or
editing it; determine whether its constraints are genuine project requirements
or accidental prompt choices.

### Recurring workflows

Trace each high-value workflow end to end: its trigger, evidence sources,
decision points, artifact, review or approval, Git action, validation, and final
handoff. Adapt the installed flow rather than inventing generic folders:

- use `wayfinder` only when the route to a large destination is genuinely
  unclear;
- use `grill-with-docs` to sharpen work while building domain context;
- use `research` and `prototype` when facts or runnable evidence are needed;
- use `to-spec`, `to-tickets`, and `implement` for agent execution when the work
  is large enough to need them;
- use direct implementation for small, well-understood changes.

Identify repeated requests, transformations, API calls, cryptographic helpers,
test probes, or report generation that deserve a script or a dedicated skill.
Record the need; build it only when the user includes that work in scope.

### Git and human handoffs

Settle the actual branch, commit, pull-request, review, merge, and worktree
conventions for each repository. Prefer the repository's established practice.
Do not invent a manual-only flow because the operator can review in a web or
mobile UI; the agent may perform the Git work and the human may choose the
review surface.

### Tracker and domain configuration

Configure the engineering skills during this same session:

- select the real issue tracker and write
  `docs/agents/issue-tracker.md` from the matching template beside
  `setup-matt-pocock-skills`;
- if `triage` is installed, settle its five label mappings and write
  `docs/agents/triage-labels.md`;
- select single-context domain docs unless inspected monorepo evidence requires
  a multi-context map, then write `docs/agents/domain.md`;
- add one `## Agent skills` block to `AGENTS.md` that points to these files.

Use local markdown only when it is the chosen tracker. Never create a backlog,
PRD, notes, templates, or scripts directory merely because such a directory
might be useful someday.

### Integrations and credentials

Determine which integrations are actually needed only after workflows and
sources of truth are known. Create `docs/agents/credentials.md` from
`../credentials/references/credential-config.md`. Inspect safe `gcloud`
configuration and Secret Manager metadata to propose the project, operator
account, existing name families, and materializations; never read payloads.

Google Secret Manager is the recommended durable store for every exportable
machine secret. Human passwords, MFA recovery, payment authority, and broad
account recovery stay solely in the human-controlled password manager. Local
`.env`, Keychain, CLI, SSH, mounted, and CI copies are materializations rather
than competing durable stores.

Choose browser-opening mode from actual operator reality. When one default
profile/account is unambiguous, recommend `automatic`; when several profiles or
accounts exist, recommend `manual`. In either mode, show the exact URL and
expected account before navigation.

For each required credential, keep secret entry in the provider UI or a hidden
terminal prompt, save exportable machine material durably, validate access
immediately with the narrowest real probe, and record only the safe validation
result. An SSH private key must have its designated Secret Manager backup before
it becomes the sole production access path.

### Communication conventions

Settle the language, terminology, level of detail, and required templates for
each audience. Agent instructions remain concise and operational. Domain
language belongs in the glossary, and large explanations belong in the owning
profile or source document.

## Persist decisions inline

As soon as a decision is settled, write it to its one canonical home. Do not
wait until the end of the interview, and do not persist speculative answers.

- Create or update `docs/agents/workspace.md` from the bundled reference
  format.
- Update `AGENTS.md` only for rules that should alter future agent behavior;
  point to the profile for factual detail.
- Let `domain-modeling` create `CONTEXT.md` and ADRs lazily under its own
  criteria.
- Preserve existing user edits and update named sections in place rather than
  appending duplicates.
- If a contradiction appears, resolve it with the user before writing either
  version as canonical.

If the session stops early, change the Workspace profile status to
`initialization in progress`, keep settled material, and leave unresolved items
in the profile's Open setup items section. A later invocation resumes from that
frontier.

## Completion criteria

Initialization is complete only when:

1. `docs/agents/workspace.md` accurately maps the working environment.
2. Issue-tracker, triage-label, and domain-layout configuration exists for the
   installed engineering skills.
3. `docs/agents/credentials.md` records the GCP project, safe operator metadata,
   browser mode, deterministic naming, local materializations, and explicit
   exceptions without any secret values.
4. Every nested repository has an exact parent `.gitignore` entry and its own
   instructions have been respected.
5. Required integrations are either validated or listed as explicit safe
   blockers; no secret appears in Git or chat.
6. The root `AGENTS.md` has one non-duplicated instruction path for every
   durable rule and its Workspace profile status is `initialized`.
7. A final scan finds no contradictory instructions, duplicated ownership, or
   invented empty artifact directories.
8. The user confirms the final workspace summary and diff.

After confirmation, commit the initialization on the current branch with a
descriptive message. Do not push, open a pull request, or merge unless the user
asks for that external action.

End with a short practical orientation: use `$ask-matt` when unsure; use
`grill-with-docs` for normal work, `wayfinder` for genuinely foggy large work,
and the configured human-facing artifact workflow when producing team
documentation.
