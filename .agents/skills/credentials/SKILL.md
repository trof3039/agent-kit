---
name: credentials
description: Manage, retrieve, save, rotate, recover, audit, or provision credentials. Use whenever work touches passwords, API keys, tokens, SSH keys, provider authentication, secret stores, or credential setup.
---

# Credentials

Keep credential material in its approved store and make human setup resumable.

## Route the work

1. Read the repository's credential policy and existing tooling before acting.
   Repository-specific ownership, stores, schemas, and authorization boundaries
   override these personal defaults.
2. For listing, retrieving, saving, changing, rotating, or auditing credentials,
   read [storage and operations](references/storage-and-operations.md).
3. When a human must obtain, recover, or enter credentials, especially across
   several providers, read [terminal wizard UX](references/terminal-wizard.md).
4. Establish the authorized scope. Metadata inspection is read-only; creating,
   rotating, revoking, or deleting external authority still requires that work
   to be within the user's request.

## Invariants

- Secret entry happens in a provider UI or hidden terminal prompt. Keep values
  out of chat, tool output, shell history, command arguments, logs, screenshots,
  tracked files, and non-secret inventories.
- Print login and provider URLs for the user to open in the correct browser
  profile. Leave the browser and clipboard under the user's control.
- Keep one canonical owner, identifier, and store for each credential. Treat a
  provider CLI config as an operational copy, not an accidental second policy.
- List and report names, locations, versions, timestamps, scopes, and validation
  state without reading or printing payloads.
- Validate a new or recovered credential immediately through the narrowest real
  authenticated probe. Report only pass/fail and safe metadata.
- Add or validate replacement access before removing old access. Revocation and
  destruction are separate, explicit completion steps.
- Finish with a safe inventory of what was validated, skipped, or remains
  blocked; never include credential values.
