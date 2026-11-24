# AI Laptop v3.16.5 — trusted memory validity

The [runtime patch](releases/v3.16.5-memory-validity-fixes.zip) upgrades an existing AI Laptop v3.16.4+ deployment. Verify it against the [SHA-256 file](releases/v3.16.5-memory-validity-fixes.zip.sha256), then extract it and run the installer from the extracted directory.

```sh
sha256sum -c v3.16.5-memory-validity-fixes.zip.sha256
sudo python3 tools/install-runtime-release.py --dry-run
sudo python3 tools/install-runtime-release.py
```

The installer backs up the current runtime image, source, configuration, and runtime volume. Use `sudo python3 tools/install-runtime-release.py --rollback BACKUP_DIRECTORY` to roll back the code and image. A data restore is a separate recovery operation.

## Changes

- Apply the same trusted-memory validity rules to ranked API results and prompt context. The start is inclusive and the end is exclusive, with UTC normalization. Invalid or reversed windows are rejected.
- Prevent a duplicate Mem0 result from restoring text excluded by the trusted-memory validity filter.
- Carry forward v3.16.4 model failure diagnostics and SQLite maintenance concurrency fixes.

## Validation

- All 13 packaged regression and compatibility suites passed in an isolated workspace with the declared runtime dependencies. The operator CLI mock exercised the root-only wrapper in a user namespace. All 98 Python files parsed, and shell scripts passed syntax checks.
- The v3.16.4 isolated live run recorded a verified governed health task in 57.77 seconds and a maintenance checkpoint with zero busy frames. The v3.16.5 deployment log reported healthy.
- A later Cockpit spot check verified both LiteLLM chat aliases, seven successful maintenance responses, and an MCP handshake listing eight tools.
- Live cross-session memory recall, user isolation, restart persistence, cleanup, and sustained SQLite contention remain open.
- The cumulative installer remains pending exact model pinning. Its installer pulls mutable tags and the recent live Qwen model ID differs from the older recorded pin.
