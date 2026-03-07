# MCPSec Skill

[![ClawHub](https://img.shields.io/badge/ClawHub-mcpsec-blue)](https://clawhub.com/pfrederiksen/mcpsec-skill)
[![Version](https://img.shields.io/badge/version-1.0.0-green)]()

An [OpenClaw](https://openclaw.ai) skill that wraps [pfrederiksen/mcpsec](https://github.com/pfrederiksen/mcpsec) to periodically audit your MCP server configurations against the OWASP MCP Top 10.

## Features

- 🔍 **Auto-discovery** — finds Claude Desktop, Cursor, VS Code, and custom MCP configs automatically
- 🔴🟠🟡🟢 **Severity classification** — critical / high / medium / low findings
- 📋 **OWASP MCP Top 10** — all 10 risk categories covered
- 🤫 **Quiet mode** — `--quiet` for cron use: silent when nothing found
- 📄 **JSON output** — `--format json` for SIEM/dashboard integration (Splunk-ready)
- 🔒 **Read-only** — never modifies any config file

## Installation

```bash
# Install mcpsec binary
brew install pfrederiksen/tap/mcpsec
# or
go install github.com/pfrederiksen/mcpsec/cmd/mcpsec@latest

# Install the skill
clawhub install mcpsec-skill
```

## Usage

```bash
# Auto-scan all known MCP config locations
python3 scripts/scan.py

# Specific file
python3 scripts/scan.py ~/Library/Application\ Support/Claude/claude_desktop_config.json

# Quiet mode (ideal for cron — silent if clean)
python3 scripts/scan.py --quiet

# JSON output
python3 scripts/scan.py --format json
```

## Requirements

- Python 3.10+
- `mcpsec` binary on PATH

## Links

- [mcpsec scanner](https://github.com/pfrederiksen/mcpsec)
- [ClawHub](https://clawhub.com/pfrederiksen/mcpsec-skill)
- [OpenClaw](https://openclaw.ai)
