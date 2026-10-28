# Changelog

## 0.1.0

- Adds the `macos_process_lifecycle` pipeline. It parses macOS Endpoint
  Security process lifecycle events (exec, fork, exit) into top-level
  fields, sets `index` to `os` and `sourcetype` to
  `macos:process:lifecycle`, and drops events with no exec, fork or exit
  record.
