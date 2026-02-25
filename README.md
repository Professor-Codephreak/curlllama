# curllama
**A professional, batteries-included Bash toolkit** for discovering Ollama servers on your LAN, validating their HTTP API, running a smoke-test chat, and generating a copy-paste **curl command map** for day-to-day ops.

<p align="center">
  <img alt="llamacurl banner" src="https://raw.githubusercontent.com/your-org/your-repo/main/.github/assets/llamacurl-banner.png" width="820">
</p>

<p align="center">
  <a href="#quick-start"><img alt="bash" src="https://img.shields.io/badge/bash-%3E%3D4.0-4EAA25?logo=gnu-bash&logoColor=white"></a>
  <a href="#requirements"><img alt="curl" src="https://img.shields.io/badge/curl-required-00599C?logo=curl&logoColor=white"></a>
  <a href="#license"><img alt="license" src="https://img.shields.io/badge/license-MIT-blue"></a>
  <a href="#security-model"><img alt="lan-safe" src="https://img.shields.io/badge/LAN--safe-no%20remote%20kill-critical"></a>
</p>

---

## Why this exists

You can `curl` Ollama’s HTTP API, but when you’re working across a LAN you typically need:

- a fast way to **find** servers,
- a **clean sanity check** that a server is alive and reachable,
- a repeatable **smoke test** against a real model, and
- a cheat-sheet of **battle-ready curl commands** you can paste into terminals or CI.

`llamacurl.sh` does exactly that — without pretending you can administrate remote machines you don’t control.

---

## Highlights ✨

- **Optional LAN discovery**: probes `GET /api/version` across a configurable subnet and port set
- **Verifies** `GET /api/version`, `GET /api/tags`, `GET /api/ps`
- **Runs a required chat smoke test** using `POST /api/chat`
- **3-second “doubletap” inputs**: all interactive prompts auto-default after 3 seconds
- **Remote-safe by design**:
  - remote targets → **read-only HTTP validation**
  - local targets → optional local checks (ports/systemd) + optional UFW lockdown
- **Prints a full curl command map** for common Ollama endpoints (generate/chat/pull/show/etc.)
- **Self-healing shell**: if executed under `sh`, it re-execs itself under `bash`

---

## Security model (read this) 🔐

**LAN reality:** you **cannot stop/kill/disable** an Ollama process on a *remote* LAN host unless you have privileged access on that host (e.g., SSH + sudo/root, or an agent/orchestrator you manage).

This project is explicit about that:

- ✅ Remote target: **probe & verify only**
- ✅ Local target: may perform local checks and optional firewall rule changes
- ❌ No remote stop/kill attempts (ever)

---

## Quick start

```bash
chmod +x llamacurl.sh
./llamacurl.sh
You’ll be prompted for target host/port (3s timeout each). Press Enter for defaults.

What the script does
1) (Optional) Discover Ollama servers on your LAN
When enabled, it scans a subnet by probing:

http://<ip>:<port>/api/version

It reports discovered ip:port and (best-effort) model summary via /api/tags.

2) Verify the target server’s API (required)
GET /api/version

GET /api/tags (model list)

GET /api/ps (running models)

3) Run a smoke test chat (required)
POST /api/chat with:

selected model (default: first in /api/tags)

default prompt (editable; 3-second timeout)

4) Print a curl “command map”
A ready-to-copy set of API calls: tags, ps, generate, chat, pull, show, create, copy, push, delete, embed.

Requirements
Required
bash (the script re-execs itself into bash if needed)

curl

python3 (used for safe JSON escaping)

Recommended
jq (pretty JSON output)

xargs (fast parallel LAN scan)

Optional (local-only diagnostics / hardening)
lsof

netstat or ss

systemctl (systemd)

ufw + sudo (firewall lockdown)

Defaults
Setting	Default
Target host	10.0.0.155
Target port	18080
Prompt input window	3s
Curl max-time	3s
LAN scan ports	18080 11434
LAN scan parallelism	64
LAN connect timeout	1s
Configuration (environment variables)
Override defaults without editing the script:

Always show details

DEFAULT_HOST=10.0.0.155 DEFAULT_PORT=18080 ./llamacurl.sh
Available knobs:

DEFAULT_HOST — default target host/IP

DEFAULT_PORT — default target port

READ_TIMEOUT_SECS — timed input window (default 3)

CURL_TIMEOUT_SECS — curl --max-time for API requests (default 3)

DEFAULT_MODEL — model for /api/chat test (default: first from /api/tags)

DEFAULT_PROMPT — default prompt text for /api/chat

SCAN_PARALLEL — parallel workers for LAN scan (default 64)

SCAN_CONNECT_TIMEOUT — connect timeout during discovery (default 1)

SERVICE_NAME — systemd service name (default ollama)

Example workflows
Verify a known remote server
Always show details

DEFAULT_HOST=10.0.0.155 DEFAULT_PORT=18080 ./llamacurl.sh
Discover on a /24 subnet (interactive)
Run the script

Answer y to the scan prompt

Provide:

subnet prefix (e.g., 10.0.0)

range (1..254)

ports (18080 11434)

Then select a discovered ip:port target (optional, 3-second timeout).

API endpoints covered
The printed command map includes these common Ollama endpoints:

GET /api/version

GET /api/tags

GET /api/ps

POST /api/chat

POST /api/generate

POST /api/pull

POST /api/show

POST /api/create

POST /api/copy

POST /api/push

DELETE /api/delete

POST /api/embed

Troubleshooting
line 1: from: command not found
Your script file is not Bash (it likely starts with Python, e.g., from ...), or your editor pasted extra content above the shebang.

Fix: ensure the file begins with exactly:

Always show details

#!/usr/bin/env bash
syntax error near unexpected token '('
This almost always indicates the script executed under sh/dash, not bash.

Fix:

Always show details

bash ./llamacurl.sh
This tool also auto re-execs itself under bash when possible.

Unable to reach /api/version
Common causes:

wrong host/port

service not running

server bound only to localhost

firewall rules block LAN access

VLAN/subnet routing isolation

If the server is localhost-only, run the script on that host or use an SSH tunnel.

Repository layout
Always show details

.
├── llamacurl.sh
├── README.md
└── LICENSE
Contributing
Issues and PRs welcome. Keep contributions:

shellcheck-friendly where possible

safe defaults (no destructive behavior for remote targets)

focused on operator usability

License
MIT © 2026 Professor Codephreak
See LICENSE.
