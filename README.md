# curlllama

**Ollama LAN discovery + HTTP verification + curl toolkit**  
Delivered as a single operator-grade Bash script: `llamacurl.sh`

<p align="center">
  <img src="https://raw.githubusercontent.com/Professor-Codephreak/curlllama/main/gfx/curlllama.png"
       alt="curlllama architecture diagram"
       width="850"/>
</p>

<p align="center">
  <a href="#quick-start">
    <img alt="bash" src="https://img.shields.io/badge/bash-%E2%89%A54-4EAA25?logo=gnu-bash&logoColor=white">
  </a>
  <a href="#requirements">
    <img alt="curl" src="https://img.shields.io/badge/curl-required-00599C?logo=curl&logoColor=white">
  </a>
  <a href="#security-model">
    <img alt="remote safe" src="https://img.shields.io/badge/remote--safe-read--only%20checks-critical">
  </a>
  <a href="#license">
    <img alt="license" src="https://img.shields.io/badge/license-MIT-blue">
  </a>
</p>

## 🎯 Purpose

curlllama is a hardened operator utility for:

- Discovering Ollama servers on a LAN (optional)
- Verifying HTTP API health
- Running a deterministic chat smoke test
- Printing a copy-paste curl command map
- Operating in remote-safe mode by design



## ✨ Features

### 🌐 Optional LAN Discovery
Parallel probes to:


GET /api/version


Fast, controlled, timeout-based scanning.

### 🔍 API Verification (Required)

Validates:

- `GET /api/version`
- `GET /api/tags`
- `GET /api/ps`

Ensures model host is reachable and responsive.


### 💬 Chat Smoke Test

Performs:


POST /api/chat


- Operator-selected model
- Operator-selected prompt
- Structured response parsing


### 🧠 Smart Prompt UX

- 3-second “doubletap” input windows
- Enter accepts defaults
- Clear operational feedback


### 🔐 Security Model

| Target Type | Allowed Actions |
|-------------|-----------------|
| Remote Host | Discovery + HTTP verification only |
| Local Host  | Optional diagnostics + firewall controls |
| Remote Stop/Kill | ❌ Never attempted |

> Remote control requires privileged access. This tool does not escalate.


## 🚀 Quick Start

```bash
chmod +x curlllama.sh
./curlllama.sh
```
📦 Requirements

Bash ≥ 4

curl

jq (recommended for structured output)

Optional:

systemctl (local diagnostics)

ufw (local firewall lockdown)

🏗 Architecture
```txt
          +----------------------+
          |  LAN Discovery Mode  |
          +----------+-----------+
                     |
                     v
          +----------------------+
          |  HTTP API Validator  |
          |  /api/version        |
          |  /api/tags           |
          |  /api/ps             |
          +----------+-----------+
                     |
                     v
          +----------------------+
          |  Chat Smoke Test     |
          |  POST /api/chat      |
          +----------+-----------+
                     |
                     v
          +----------------------+
          |  Curl Command Map    |
          +----------------------+
```
🔬 Example Manual curl Calls
```bash
curl http://localhost:11434/api/version
curl http://localhost:11434/api/tags
curl http://localhost:11434/api/ps
curl http://localhost:11434/api/chat \
  -d '{
        "model": "llama3",
        "messages": [
          {"role": "user", "content": "ping"}
        ]
      }'
```
🛡 Operational Philosophy

No hidden behavior

No background agents

No remote process control

No privilege escalation

Deterministic outputs

This is a verification and tooling utility — not a control framework.

📜 License

MIT License

This is a verification and tooling utility — not a control framework.
