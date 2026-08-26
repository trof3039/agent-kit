---
name: credentials
description: Manage, retrieve, save, rotate, recover, audit, or provision credentials. Use whenever work touches passwords, API keys, tokens, SSH keys, provider authentication, secret stores, or credential setup.
---

# Credentials

Keep exportable machine credentials durably recoverable in Google Secret
Manager, materialize only the local copies their consumers need, and make human
setup resumable.

## Route the work

1. Read `docs/agents/credentials.md` when it exists, then read the repository's
   narrower credential policy and existing tooling. Repository-specific
   ownership, schemas, and authorization boundaries override workspace
   defaults, but an exception to the durable-store policy must state why.
2. For listing, retrieving, saving, changing, rotating, or auditing credentials,
   read [storage and operations](references/storage-and-operations.md).
3. When a human must obtain, recover, or enter credentials, especially across
   several providers, read [terminal wizard UX](references/terminal-wizard.md).
4. Establish the authorized scope. Metadata inspection is read-only; creating,
   rotating, revoking, or deleting external authority still requires that work
   to be within the user's request.

## Invariants

- Google Secret Manager is the durable store for every exportable non-human
  secret by default. Local `.env` files, Keychain items, provider CLI configs,
  and SSH files are operational copies, not the only recoverable copy.
- Human passwords, MFA recovery, payment authority, and broad account recovery
  remain in the human-controlled password manager. Never integrate with it or
  copy those values into Secret Manager.
- Secret entry happens in a provider UI or hidden terminal prompt. Keep values
  out of chat, tool output, shell history, command arguments, logs, screenshots,
  tracked files, and non-secret inventories.
- Follow the workspace's browser-opening policy. Always show the URL and
  expected account first; open it automatically only when the workspace
  explicitly permits that and the account/profile is unambiguous.
- Keep one canonical owner, identifier, and store for each credential. Treat a
  local materialization as an operational copy, not an accidental second
  policy.
- Back up an exportable SSH private key to its designated Secret Manager secret
  before production access depends on that key as the only working path.
- List and report names, locations, versions, timestamps, scopes, and validation
  state without reading or printing payloads.
- Validate a new or recovered credential immediately through the narrowest real
  authenticated probe. Report only pass/fail and safe metadata.
- Add or validate replacement access before removing old access. Revocation and
  destruction are separate, explicit completion steps.
- Finish with a safe inventory of what was validated, skipped, or remains
  blocked; never include credential values.
