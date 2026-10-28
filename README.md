# cc-edge-macos-process-telemetry-io

Cribl Edge pack that parses macOS Endpoint Security process lifecycle events
(exec, fork, exit) written as JSON lines by `eslogger`.

## Installation

Import the pack's `.crbl` release asset into Cribl Edge as a pack.

## Usage

Sources attach to the pipeline by its ID, `macos_process_lifecycle`. The route
in `default/pipelines/route.yml` matches events whose `datatype` is
`macos-process-lifecycle` and sends them to the same pipeline.

## Pipelines

- `macos_process_lifecycle` parses each event and sets the fields below. It
  sets `index` to `os` and `sourcetype` to `macos:process:lifecycle`, keeps
  `_raw`, and drops events that carry no exec, fork or exit record.

| Field | Source |
| --- | --- |
| `event_type` | `exec`, `fork` or `exit`, from the key under `event` |
| `pid` | `process.audit_token.pid` |
| `ppid` | `process.ppid` |
| `original_ppid` | `process.original_ppid` |
| `responsible_pid` | `process.responsible_audit_token.pid` |
| `exe_path` | `process.executable.path` |
| `signing_id` | `process.signing_id` |
| `team_id` | `process.team_id` |
| `args` | `event.exec.args` joined with spaces, capped at 1024 characters |
| `target_exe` | `event.exec.target.executable.path` |
| `exit_status` | `event.exit.stat` |
| `child_pid` | `event.fork.child.audit_token.pid` |
| `child_exe` | `event.fork.child.executable.path` |

## Layout

- `default/pipelines/macos_process_lifecycle/conf.yml`: the pipeline.
- `default/pipelines/route.yml`: the route to that pipeline.
- `tests/fixtures/macos_process_lifecycle/`: synthetic input lines and their
  expected output.
- `data/samples/macos_process_lifecycle.json`: sample lines for the UI sample
  preview.

## Testing

```sh
cd tests
pnpm install --frozen-lockfile
pnpm run lint
pnpm run typecheck
pnpm run test
```

`pnpm run test` needs a reachable Cribl Edge API, configured with the
`CRIBL_HOST`, `CRIBL_PORT`, `CRIBL_USER`, and `CRIBL_PASS` environment
variables. The tarball-parity and harness meta-tests need no instance.
