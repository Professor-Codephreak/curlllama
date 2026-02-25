curllama is a minimal Shell/Bash script designed to let you interact with a LLaMA-style model (e.g., running on an Ollama HTTP API) using simple curl commands. It’s essentially a command-line toolkit that automates detection of an Ollama server and exercises basic API operations.

Key Details

It’s a Bash script that:

Optionally discovers Ollama instances on a LAN by probing GET /api/version.

Validates the HTTP API (/api/version, /api/tags, /api/ps).

Performs a basic chat smoke test using POST /api/chat, letting you specify model & prompt.

Prints out a set of ready-to-use curl commands for common API calls.

Includes simple timing and prompt UI handling (e.g., “3-second doubletap input windows”).

Purpose / Use Case

Makes it easy to test and interact with locally hosted LLaMA-like models via a REST API without writing custom HTTP client code.

Provides a quick diagnostic and command mapping script for developers experimenting with Ollama model hosts.
