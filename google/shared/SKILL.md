---
name: gws-shared
description: "gws CLI: Shared patterns for authentication, global flags, and output formatting."
---

# gws — Shared Reference

## Installation

If `gws` is not installed, install it:

```bash
# Download and install the latest release
curl -sfL https://github.com/googleworkspace/cli/releases/latest/download/gws-x86_64-unknown-linux-gnu.tar.gz \
  | tar xz --strip-components=1 -C /usr/local/bin gws-x86_64-unknown-linux-gnu/gws
```

Verify it works:

```bash
gws --version
```

## Authentication

Authentication is handled automatically by ErinOS. The `GOOGLE_WORKSPACE_CLI_TOKEN`
environment variable is injected when running commands with `provider: "google"`.
Do NOT use `gws auth login` — it is not needed.

## Global Flags

| Flag | Description |
|------|-------------|
| `--format <FORMAT>` | Output format: `json` (default), `table`, `yaml`, `csv` |
| `--dry-run` | Validate locally without calling the API |
| `--sanitize <TEMPLATE>` | Screen responses through Model Armor |

## CLI Syntax

```bash
gws <service> <resource> [sub-resource] <method> [flags]
```

### Method Flags

| Flag | Description |
|------|-------------|
| `--params '{"key": "val"}'` | URL/query parameters |
| `--json '{"key": "val"}'` | Request body |
| `-o, --output <PATH>` | Save binary responses to file |
| `--upload <PATH>` | Upload file content (multipart) |
| `--page-all` | Auto-paginate (NDJSON output) |
| `--page-limit <N>` | Max pages when using --page-all (default: 10) |
| `--page-delay <MS>` | Delay between pages in ms (default: 100) |

## Security Rules

- **Never** output secrets (API keys, tokens) directly
- **Always** confirm with user before executing write/delete commands
- Prefer `--dry-run` for destructive operations
- Use `--sanitize` for PII/content safety screening
