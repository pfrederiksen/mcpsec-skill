---
name: mcpsec
description: "Scan MCP server configuration files for security vulnerabilities using mcpsec (OWASP MCP Top 10). Use when: auditing MCP tool configs for prompt injection, hardcoded secrets, missing auth, insecure transport, or excessive permissions. Auto-discovers config files for Claude Desktop, Cursor, VS Code, and custom paths. Reports findings by severity. Read-only — never modifies any config."
---

# MCPSec

Security scanner for Model Context Protocol (MCP) server configurations, built on [pfrederiksen/mcpsec](https://github.com/pfrederiksen/mcpsec). Covers all 10 OWASP MCP Top 10 risk categories.

## Usage

```bash
# Auto-discover and scan all known MCP config locations
python3 scripts/scan.py

# Scan a specific config file
python3 scripts/scan.py ~/Library/Application\ Support/Claude/claude_desktop_config.json

# Only show critical and high findings
python3 scripts/scan.py --severity critical,high

# JSON output (for dashboards/SIEM)
python3 scripts/scan.py --format json

# Quiet mode: only output if findings exist (good for cron)
python3 scripts/scan.py --quiet
```

## What It Scans

Auto-discovers configs at these paths:
- `~/Library/Application Support/Claude/claude_desktop_config.json` (Claude Desktop)
- `~/Library/Application Support/Claude/Claude Extensions/` (DXT extensions)
- `~/.cursor/mcp.json` (Cursor)
- `~/.vscode/mcp.json` (VS Code)
- `~/.openclaw/workspace/mcp-config.json` (custom)

## OWASP MCP Top 10 Coverage

| ID | Risk | Severity |
|---|---|---|
| MCP01 | Prompt injection in tool descriptions | High |
| MCP02 | Excessive tool permissions | Critical/High |
| MCP03 | Missing authentication | Critical/High |
| MCP04 | Hardcoded secrets in env vars | Critical |
| MCP05 | Unsafe resource URIs (SSRF) | High |
| MCP06 | Tool definition spoofing | High/Medium |
| MCP07 | Insecure transport (HTTP, weak TLS) | Critical/High |
| MCP08 | Missing input validation schemas | Medium |
| MCP09 | Missing logging/audit config | Medium/High |
| MCP10 | No rate limiting | Medium |

## Security Design

- `subprocess` used exclusively with `shell=False`
- All file paths validated against an allowlist pattern before use
- All exceptions caught by specific type — no bare `except`
- Full type hints and docstrings throughout
- Read-only — no config files are modified

## System Access

- **Reads:** MCP config JSON files at known paths (or paths you specify)
- **Executes:** `mcpsec scan` (read-only binary, no network calls)
- **No writes**

## Requirements

- Python 3.10+
- `mcpsec` binary on PATH — install from <https://github.com/pfrederiksen/mcpsec>
