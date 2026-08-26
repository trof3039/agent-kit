# Terminal wizard UX

Use a task-specific terminal wizard when credential setup includes multiple
items, browser/provider steps, account-profile choices, or a flow the user may
need to resume. For a single simple item, one command with a hidden prompt is
usually enough.

## Contract

- Read `docs/agents/credentials.md`. It supplies the GCP project, expected
  operator account, secret naming, and browser-opening mode.
- Generate one executable command under `~/.local/bin` and give the user that
  one command in chat. The terminal owns the remaining instructions.
- Name each stage by provider and purpose. Explain what the credential grants
  before asking the user to obtain it.
- Always print the exact URL and expected account/profile before navigation. In
  `automatic` mode, open it after displaying those facts and fall back to a
  manual link if opening fails. In `manual` mode, never invoke a URL opener.
- Accept secrets only through a hidden terminal prompt or directly inside the
  provider UI. Avoid clipboard automation.
- Save each exportable machine secret immediately to its stable Google Secret
  Manager name, materialize the consumer's local copy when needed, and run a
  narrow validation before advancing. Never make `.env`, Keychain, a CLI config,
  or `~/.ssh` the only recoverable copy.
- Detect already-valid state and skip completed stages on rerun. Resumption is
  derived from Secret Manager metadata, local materialization state, and a real
  validation probe, not a plaintext progress file.
- Offer a skip path for a blocked credential and continue with independent
  stages. The final summary names completed, skipped, and failed items without
  values.
- Reuse a password that the user has already generated and saved by directing
  its entry into the provider UI or hidden prompt; never request it in chat.

Use the global `wizard` skill's library when available. Keep provider-specific
GCP storage, materialization, browser-policy, and validation logic below that
library so the resulting script stays readable and reusable.

## Completion gate

Before handing over the command:

1. Run the shell syntax check and make the script executable only for the user.
2. Verify that no literal credential, token-shaped placeholder, clipboard
   mutation, or tracked-file write is present. Verify that URL-opening behavior
   matches the configured `automatic` or `manual` mode.
3. Exercise safe state-detection and skip branches where practical.
4. Ensure an interrupted run can be restarted without repeating validated
   stages or losing credentials already saved.
5. Ensure the closing screen reports safe locations and exact remaining work.

The existing `restore-provider-access` command is the local UX precedent: a
single resumable terminal flow with explicit account context, hidden entry,
immediate durable save, validation, and a safe summary. Its manual URL policy is
a workspace choice, not a universal credential rule.
