# Terminal wizard UX

Use one task-specific command under `~/.local/bin` when a human flow has several
providers, browser steps, account choices, or resumable stages. A single value
normally needs one command with a hidden prompt, not a multi-stage wizard.

Read `docs/agents/credentials.md` for the project, identity, names, browser mode,
and expected account. Then make the command:

- show each stage, purpose, exact URL, and expected account before navigation;
- follow the recorded browser mode, with the human retaining profile and
  clipboard control;
- accept values only in provider UI or hidden terminal input;
- validate through a narrow real probe, save successful exportable values to
  their stable Secret Manager resource, and create only required
  materializations;
- infer completed work from canonical state and validation, so reruns resume;
- let blocked independent stages be skipped and report completed, skipped, and
  failed names without values.

Use the global `wizard` library only for a genuine multi-stage flow. Keep its
library unchanged and put provider-specific storage and validation below it.

Before handoff, run the shell syntax check and ShellCheck when available, set
user-only executable permissions, and statically verify that the command has no
literal secret, clipboard mutation, tracked-file write, or browser behavior that
violates workspace policy. Exercise preflight, already-complete, skip, and
restart paths without exposing a payload.
