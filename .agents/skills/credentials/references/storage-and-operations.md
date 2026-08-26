# Storage and operations

Use the repository's credential policy when one exists. These are the personal
defaults for work that has no narrower owner.

## Choose the store

| Credential kind | Canonical location |
| --- | --- |
| Secret consumed by a local macOS wrapper or app | macOS Keychain under a stable service and account |
| Durable provider document, machine-access record, or encrypted CLI-config backup | Google Secret Manager under one stable logical secret name |
| Active provider CLI session | The provider's canonical config file, mode `0600`; back it up to the designated Secret Manager secret when policy permits |
| MFA recovery, payment authority, broad account administration, or an already managed human password | The user's password manager |
| SSH authentication | Device-specific private key in `~/.ssh`; public key in the provider account or target `authorized_keys` |
| Repository runtime cache | A gitignored file only when the repository explicitly owns that cache; never the durable source of truth |

Changing execution machines does not change the credential's owner. Prefer a
new device-specific SSH key over exporting an old private key.

## Inspect without disclosure

- List Secret Manager resource names with `gcloud secrets list`; inspect versions
  with `gcloud secrets versions list NAME`. These operations do not read values.
- Probe a known Keychain item without `-w`, for example
  `security find-generic-password -a ACCOUNT -s SERVICE`.
- Inspect provider CLI contexts and account identity through their metadata or
  account commands. Avoid debug modes that may print authorization headers.
- Use the repository's own `credentials list`, `status`, or `audit` commands when
  present. They are the authority for repository-owned inventory.

Do not create a parallel inventory of secret values. A non-secret inventory may
record only the stable name, owner, store, purpose, and validation status.

## Save or update

### macOS Keychain

Use a hidden native prompt by putting `-w` last:

```bash
security add-generic-password -U -a "ACCOUNT" -s "SERVICE" -w
```

Use stable service and account identifiers. `-U` changes the existing item
instead of creating lookalike entries.

### Google Secret Manager

Create one resource for one logical credential, then add versions to update it.
Feed payloads through stdin or a mode-`0600` provider config file using
`--data-file=-` or `--data-file=PATH`. A generated wizard should collect a
single secret with hidden input and pipe it directly; it must not embed the
value in its source or arguments.

### Provider CLI config

Authenticate through the provider's normal hidden prompt or device flow, verify
the selected account/context, set the config file to mode `0600`, then add that
file as a new version of its designated Secret Manager backup. Back up the
smallest complete config that the CLI needs, not unrelated home-directory state.

### SSH

Generate a device-labelled key, keep the private file mode `0600` and public
file mode `0644`, register only the public key, and verify a fresh batch-mode SSH
session. Remove an older key only after the new session succeeds.

## Retrieve and consume

- When a human explicitly needs to see a value, give a terminal command for the
  human to run directly. Do not run a value-printing command through an agent
  tool whose output returns to chat.
- For automation, read the value inside a narrow wrapper and pass it directly to
  the consumer through stdin or the consumer's expected environment. Unset shell
  variables afterward and suppress command tracing.
- When a CLI requires a temporary file, create it with mode `0600`, use the
  smallest lifetime possible, and remove that exact file after validation.

## Rotate or recover

Keep the stable logical name unless ownership or scope actually changes. Add the
new version or key, validate it in the real consumer, switch the consumer, and
only then revoke or disable the old authority when authorized. A disclosed
credential is rotated, not merely copied to a different store.
