# Storage and operations

Read `docs/agents/credentials.md` first when it exists. It names the Google
Cloud project, operator identity, browser-opening policy, secret-name
conventions, local materializations, and explicit exceptions. These are the
workspace defaults when no narrower repository policy exists.

## Storage model

Use three layers rather than choosing a different durable store ad hoc for each
new secret:

| Layer | Owner | Contents |
| --- | --- |
| Human root of trust | Human-controlled password manager | Passwords, MFA recovery, payment authority, broad account recovery, and the Google account needed to bootstrap `gcloud`. No agent integration. |
| Durable machine-secret store | Google Secret Manager | Every exportable API secret, token document, provider config backup, runtime secret, server credential document, and SSH private key. One stable logical secret per credential. |
| Operational materialization | The consumer's expected local location | Gitignored `.env`, macOS Keychain item, provider CLI config, SSH private key, mounted file, or CI secret. Re-creatable from Secret Manager or a new provider login. |

Google Secret Manager is the default, not one option among several. Skip its
durable copy only when the secret is deliberately non-exportable, provider or
repository policy forbids backup, or the value is part of the human root of
trust. Record the reason in `docs/agents/credentials.md`.

Do not create a circular bootstrap. On an operator Mac, regain Secret Manager
access through the human Google account and `gcloud auth login`; the credential
needed to enter Secret Manager must not exist only inside that same Secret
Manager project. A server should use its repository-defined workload identity
or narrowly scoped bootstrap credential.

### Default mappings

| Credential kind | Durable copy | Operational copy |
| --- | --- | --- |
| Local application or wrapper secret | Google Secret Manager | macOS Keychain or gitignored mode-`0600` file, as the consumer requires |
| Repository runtime secret | Google Secret Manager | Generated gitignored `.env` or mounted mode-`0600` file |
| Exportable provider CLI session or config | Google Secret Manager | Provider's canonical config file, mode `0600` |
| Static API key or token | Google Secret Manager | Narrow wrapper input, Keychain, `.env`, CI secret, or provider config |
| SSH authentication | Google Secret Manager backup of the private key | Device-labelled private key in `~/.ssh`, mode `0600`; public key registered with the target |
| Human password, MFA recovery, payment authority, broad account recovery | Never copy to Secret Manager | Human-controlled password manager or provider UI only |
| Deliberately non-exportable provider session | No copied payload; record the exception | Provider-owned CLI or OS credential store |

Changing execution machines does not change the credential's logical owner.
Restoring a backed-up SSH key is acceptable for recovery; prefer issuing and
validating a new device-specific key when normal key rotation is available.

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

Use deterministic names from `docs/agents/credentials.md`. The recommended
families are `provider-<name>`, `runtime-<repo>-<environment>`,
`server-<name>`, and `operator-ssh-<label>`. Add safe labels such as
`managed-by`, `kind`, `owner`, and `environment`; never put a secret value in a
name or label.

Grant least privilege at the individual secret when practical, enable Secret
Manager Data Access audit logs, and separate environments when their readers
differ. Add a new immutable version for an update. Disable an old version before
destroying it so rollback remains possible while consumers are verified.

### Provider CLI config

Authenticate through the provider's normal hidden prompt or device flow, verify
the selected account/context, set the config file to mode `0600`, then add the
smallest complete exportable config as a new version of its designated Secret
Manager backup. Do not copy unrelated home-directory state. If the provider
intentionally makes the session non-exportable, record that exception and rely
on a fresh provider login for recovery.

### SSH

Generate a device-labelled key, keep the private file mode `0600` and public
file mode `0644`, register only the public key, and verify a fresh batch-mode SSH
session. Before this becomes the sole production access path, store the private
key as the designated Secret Manager secret and safely record its public
fingerprint. On recovery, materialize the exact key with mode `0600` and validate
it in batch mode. Remove an older key or disable an older secret version only
after the replacement session succeeds.

### Runtime files and Keychain

A local `.env`, mounted secret file, or Keychain item may duplicate a Secret
Manager value because it is a delivery format for a consumer, not another
durable authority. Keep files gitignored and mode `0600`, write only the values
the consumer needs, and regenerate them when the durable version changes.

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
