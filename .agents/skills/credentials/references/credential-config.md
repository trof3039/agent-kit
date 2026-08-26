# Credential configuration format

Create `docs/agents/credentials.md` during workspace initialization using this
format. It contains only non-secret policy and safe metadata. Omit unused
sections and remove resolved setup items.

```markdown
# Credential configuration

## Durable store

- Provider: Google Secret Manager
- Project ID: <non-secret GCP project id>
- Operator account: <expected non-secret Google account>
- Replication: <automatic or named policy>

## Secret naming

- Provider access: `provider-<name>`
- Repository runtime: `runtime-<repo>-<environment>`
- Server access: `server-<name>`
- SSH private key: `operator-ssh-<label>`

## Browser opening

- Mode: `<automatic|manual>`
- Expected browser/profile: <safe profile label or "single default profile">

`automatic` means a wizard displays the URL and expected account, then opens
it. `manual` means it displays the same information and waits for the human to
open it in the correct profile.

## Local materializations

| Purpose | Durable secret | Local destination | File mode | Validation |
| --- | --- | --- | --- | --- |
| <consumer> | <Secret Manager name> | <safe path or Keychain service/account> | <mode or n/a> | <safe probe> |

## Explicit exceptions

| Credential | Why it is not copied to Secret Manager | Recovery path |
| --- | --- | --- |
| Human passwords and MFA recovery | Human root of trust; no agent integration | Human password manager and provider recovery |
| <non-exportable session, if any> | <provider or repository constraint> | <fresh login or other safe recovery> |

## Open setup items

- <Only unresolved configuration or safe blocker metadata.>
```

Never put a secret payload, recovery code, private key, token, password, or
value-bearing command in this file.
