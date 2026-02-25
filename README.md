# curllama

**Ollama LAN discovery + HTTP verification + curl toolkit** — delivered as a single operator-grade Bash script: `llamacurl.sh`.

<p align="center">
  <img alt="curllama banner" src="https://raw.githubusercontent.com/your-org/your-repo/main/.github/assets/curllama-banner.png" width="880">
</p>

<p align="center">
  <a href="#quick-start"><img alt="bash" src="https://img.shields.io/badge/bash-%E2%89%A54-4EAA25?logo=gnu-bash&logoColor=white"></a>
  <a href="#requirements"><img alt="curl" src="https://img.shields.io/badge/curl-required-00599C?logo=curl&logoColor=white"></a>
  <a href="#security-model"><img alt="remote safe" src="https://img.shields.io/badge/remote--safe-read--only%20checks-critical"></a>
  <a href="#license"><img alt="license" src="https://img.shields.io/badge/license-MIT-blue"></a>
</p>

> **Purpose:** Find Ollama servers on a LAN (optional), validate the HTTP API, run a smoke-test chat, and print a copy-paste `curl` command map for common operations.

---

## What you get ✨

- **Optional LAN discovery** (parallel probes to `GET /api/version`)
- **Required API validation**: `GET /api/version`, `GET /api/tags`, `GET /api/ps`
- **Required chat smoke test**: `POST /api/chat` (model + prompt selectable)
- **Timed prompts**: 3-second “doubletap” input windows (Enter accepts defaults)
- **Remote-safe by design**: remote targets are **read-only** (no process control)
- **Local-only extras** (optional): port checks, systemd status, UFW localhost-only lockdown
- **Shell-robust**: if launched under `sh`, it re-execs itself under `bash`

---

## Security model 🔐

**LAN reality:** you **cannot stop/kill/disable** Ollama on a *remote* host unless you have **privileged access** on that host (e.g., SSH + sudo/root, or an agent/orchestrator you manage).

This project is explicit:

- ✅ **Remote target:** discovery + HTTP/API verification only  
- ✅ **Local target:** optional diagnostics + optional firewall changes  
- ❌ **No remote stop/kill attempts** (ever)

---

## Quick start

```bash
chmod +x llamacurl.sh
./llamacurl.sh
