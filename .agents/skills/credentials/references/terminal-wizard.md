# Terminal wizard UX

Use a task-specific terminal wizard when credential setup includes multiple
items, browser/provider steps, account-profile choices, or a flow the user may
need to resume. For a single simple item, one command with a hidden prompt is
usually enough.

## Contract

- Generate one executable command under `~/.local/bin` and give the user that
  one command in chat. The terminal owns the remaining instructions.
- Name each stage by provider and purpose. Explain what the credential grants
  before asking the user to obtain it.
- Print the exact URL and tell the user which account/profile is expected. Never
  open the browser automatically.
- Accept secrets only through a hidden terminal prompt or directly inside the
  provider UI. Avoid clipboard automation.
- Save each successful item immediately in its canonical location, create any
  approved encrypted backup, and run a narrow validation before advancing.
- Detect already-valid state and skip completed stages on rerun. Resumption is
  derived from canonical stored state, not a plaintext progress file.
- Offer a skip path for a blocked credential and continue with independent
  stages. The final summary names completed, skipped, and failed items without
  values.
- Reuse a password that the user has already generated and saved by directing
  its entry into the provider UI or hidden prompt; never request it in chat.

Use the global `wizard` skill's library when available. Keep provider-specific
logic below that library so the resulting script stays readable and reusable.

## Completion gate

Before handing over the command:

1. Run the shell syntax check and make the script executable only for the user.
2. Verify that no literal credential, token-shaped placeholder, clipboard
   mutation, tracked-file write, or stage that invokes an automatic URL opener
   is present.
3. Exercise safe state-detection and skip branches where practical.
4. Ensure an interrupted run can be restarted without repeating validated
   stages or losing credentials already saved.
5. Ensure the closing screen reports safe locations and exact remaining work.

The existing `restore-provider-access` command is the local UX precedent: a
single resumable terminal flow with manual URLs, hidden entry, immediate save,
validation, and a safe summary.
