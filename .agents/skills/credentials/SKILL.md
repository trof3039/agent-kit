---
name: credentials
description: Manage, recover, audit, or provision passwords, tokens, API keys, SSH keys, provider authentication, and secret stores.
---

# Credentials

Keep exportable machine authority recoverable without exposing its values.

## Route

1. Read `docs/agents/credentials.md` when present, then the owning repository's
   credential policy and tooling. They define names, projects, identities,
   materializations, browser behavior, and narrower authorization boundaries.
2. Read [storage and operations](references/storage-and-operations.md) for
   inventory, access, storage, rotation, recovery, or audit work.
3. Read [terminal wizard UX](references/terminal-wizard.md) only when a human
   must authenticate, navigate a provider, or enter a value.
4. Confirm that external mutations are inside the user's requested scope.

## Invariants

- Google Secret Manager is the durable source for each exportable machine
  secret. Local files, Keychain, CLI stores, mounted files, and CI secrets are
  replaceable materializations. Record every exception and its recovery path.
- Human passwords, MFA recovery, payment authority, and broad account recovery
  remain in the human-controlled password manager with no agent integration.
- Values enter through a provider UI or hidden terminal prompt and travel by
  stdin or a protected file. Keep them out of chat, output, arguments, logs,
  screenshots, tracked files, and non-secret inventories.
- Give each credential one owner, stable name, and canonical project. Use
  versions for changes; use separate projects when reader boundaries differ.
- Credential validation is single-source: inject exactly the declared
  credential into a clean client with every ambient identity, profile, agent,
  config, cache, and interactive authentication path disabled. A route or
  endpoint health check does not validate a credential and cannot authorize a
  switch, revocation, disablement, or deletion.
- Inspect and report metadata only. Validate authority with the narrowest
  single-source real probe and report safe status.
- Validate replacement access before revocation. Disablement, destruction, and
  deletion are separate explicit acts.
