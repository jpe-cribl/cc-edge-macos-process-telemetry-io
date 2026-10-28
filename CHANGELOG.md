# Changelog

## 0.1.0

- Adds the `macos_process_lifecycle` pipeline. It parses macOS Endpoint
  Security process lifecycle events (exec, fork, exit) into top-level
  fields, including the fork child's `child_pid` and `child_exe`, sets
  `index` to `os` and `sourcetype` to `macos:process:lifecycle`, removes the
  parsed Endpoint Security objects, and drops events with no exec, fork or
  exit record.
