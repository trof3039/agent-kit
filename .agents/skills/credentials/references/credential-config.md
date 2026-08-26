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

## Resource naming

- Provider profile: `provider-<provider>-<profile>`
- Repository runtime: `runtime-<repo>-<environment>`
- External service: `service-<name>-<purpose>`
- SSH private key: `operator-ssh-<identity-label>`
- Machine record: `machine-<alias>`

Names identify stable authority or inventory identity. Changes use Secret
Manager versions; lifecycle labels such as `legacy`, `old`, `new`, `current`,
or `v2` never enter a stable name.

## Machine records

Machine inventory shares the credential project as strict JSON addressed by
`machine-<alias>`. The base fields are `alias`, `provider`, `serviceName`,
`ipv4`, `sshUsername`, and `role`; the address suffix must equal `alias`.
Repository-owned schemas may add validated fields. `provider` identifies the
host supplier; it does not prove that a provider API profile exists. The
validated SSH route explicitly selects an operator identity and never infers a
target-specific key from the machine alias. A workspace may share one operator
identity across machines or require target-specific identities.

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

Record the machine schema and registry location here, but not instance values,
payloads, recovery codes, private keys, tokens, passwords, or value-bearing
commands.
