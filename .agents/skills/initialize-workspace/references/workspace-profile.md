# Workspace profile format

Use this structure for `docs/agents/workspace.md`. Omit empty optional sections;
do not leave placeholder headings in the completed profile.

```markdown
# Workspace profile

## Purpose

<What this workspace exists to help accomplish.>

## Working responsibilities

- <Recurring outcome or coordination responsibility.>

## Repositories

| Repository | Local path | Canonical contents | Validation or handoff |
| --- | --- | --- | --- |
| <name> | <path relative to workspace> | <what is authoritative here> | <how a change is checked or reviewed> |

## Systems of record

| Subject | Canonical system | Access route | Precedence notes |
| --- | --- | --- | --- |
| <subject> | <system> | <tool or non-secret path> | <how conflicts are resolved> |

## Artifact contracts

| Artifact | Audience | Canonical home | Creation or update workflow | Done when |
| --- | --- | --- | --- | --- |
| <artifact> | <reader> | <tracker, repository, or system> | <skill or process> | <review/validation signal> |

## Recurring workflows

### <Workflow name>

- Trigger: <what starts it>
- Inputs: <authoritative evidence>
- Agent flow: <skills and tools used>
- Human handoff: <where a person reviews or decides>
- Validation: <observable completion check>

## Working conventions

- <Language, naming, Git, review, communication, or scheduling convention.>

## Integrations

| Integration | Purpose | Access method | Safe validation state |
| --- | --- | --- | --- |
| <service> | <why it is used> | <tool and account label, never a secret> | <validated, blocked, or not configured> |

## Open setup items

- <Only unresolved initialization decisions or blockers. Remove the section when empty.>
```

This file owns factual workspace topology and workflow mapping. Behavioral
instructions belong in `AGENTS.md`; domain definitions belong in `CONTEXT.md`;
decision rationale belongs in qualifying ADRs; secret values belong nowhere in
the repository.
