# Storage and operations

Read `docs/agents/credentials.md` first when present. It owns the workspace's
non-secret project, identity, naming, materialization, browser, and exception
policy; repository policy may narrow it further.

## Trust model

| Layer | Contents |
| --- | --- |
| Human root | Password manager: passwords, MFA and recovery codes, payment and broad account authority, and the Google account that bootstraps `gcloud`. |
| Durable machine store | Secret Manager: exportable tokens, API secrets, provider-config backups, runtime secrets, recoverable SSH private keys, and validated machine records. |
| Materialization | The consumer's required `.env`, Keychain item, CLI config, SSH file, mount, or CI secret. |

Bootstrap Secret Manager through the human Google account and a keyless,
least-privilege identity. The identity must be able to access recoverable
payloads but need not disable, destroy, or delete them. Enable Secret Manager
Data Access logs. Multiple projects are valid IAM boundaries; a credential has
one canonical project.

## Inspect

- Use Secret Manager names, labels, version state, timestamps, and IAM metadata;
  use Keychain existence checks and provider identity/context commands.
- Prefer the repository's credential list, status, audit, and probe commands.
- A non-secret inventory contains only owner, stable name, location, purpose,
  materialization, and validation state.

## Store and materialize

- Create one Secret Manager resource per logical credential and add immutable
  versions. Send payloads through stdin or the smallest mode-`0600` source file.
- Put only safe routing metadata in names and labels. Store machine inventory
  as strict `machine-<alias>` records under the configured schema; refuse ad
  hoc `server-*` documents.
- Back up the smallest complete exportable provider config, then keep its live
  file at the provider's canonical path with mode `0600`.
- Before an SSH private key becomes the only working production path, back it
  up as `operator-ssh-<target-alias>`. Keep the local private/public pair at
  `0600`/`0644`, register only the public key, and validate a fresh batch-mode
  connection.
- Keep local runtime files gitignored and limited to the consumer's fields.
  Recreate materializations when the durable version changes.

If a human explicitly needs a value, provide a command for them to run outside
agent tooling. Automation reads inside a narrow wrapper, passes the value
directly to the consumer, suppresses tracing, and clears it afterward.

For a non-destructive GCP-to-GCP copy, use
[`copy-gcp-secrets`](../scripts/copy-gcp-secrets). It resolves the target from
the personal vault config, compares payload bytes, and never revokes the source.

## Recover or rotate

Restore the canonical payload to its expected materialization, apply its mode,
then validate the real consumer. For rotation, add and validate the new version
or key, switch consumers, and only then perform the separately authorized
disablement or revocation. A disclosed credential requires provider rotation,
not relocation alone.
