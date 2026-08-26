# Credential configuration format

Create `docs/agents/credentials.md` during workspace initialization from the
applicable sections below. It contains policy and safe metadata only. Remove
resolved setup items.

```markdown
# Credential configuration

## Durable store

- Provider: Google Secret Manager
- Project ID: <project id>
- Bootstrap account: <Google account>
- Keyless operator: <principal>
- Replication: <policy>
- Data Access audit logging: <enabled or open setup item>

## Secret naming

- Provider access: `provider-<name>`
- Repository runtime: `runtime-<repo>-<environment>`
- External service: `service-<name>-<purpose>`
- SSH private key: `operator-ssh-<device-label>`

## Browser opening

- Mode: `<automatic|manual>`
- Expected browser/profile: <safe label or "single default profile">

## Local materializations

| Purpose | Durable secret | Local destination | Mode | Validation |
| --- | --- | --- | --- | --- |
| <consumer> | <secret name> | <safe path or Keychain identity> | <mode or n/a> | <safe probe> |

## Explicit exceptions

| Credential | Reason | Recovery path |
| --- | --- | --- |
| Human root of trust | No agent integration | Password manager and provider recovery |
| <non-exportable credential> | <constraint> | <fresh login or safe recovery> |

## Open setup items

- <Safe blocker metadata only.>
```

Store machine and server inventory in the workspace map. Never put a payload,
recovery code, private key, token, password, or value-bearing command here.
