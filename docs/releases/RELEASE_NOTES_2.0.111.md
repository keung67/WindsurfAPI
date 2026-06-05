## v2.0.111 - native Read allowlist default

This release promotes the verified Read allowlist name from the v2.0.110 matrix:

- `Read`, `read_file`, and `view_file` now default to Cascade allowlist name `read_file` while still translating emitted trajectory steps as `view_file` internally.
- Real smoke on `claude-4.5-haiku` showed `Read:read_file` produces a top-level Cascade field-14 native step, while `Read:view_file` usually returned natural language and no tool call.
- `Bash` remains `run_command`; `Grep` and `Glob` remain unchanged until their protocol path is proven with top-level native steps.

Native bridge is still opt-in behind the existing model/route/API key/account gates.
