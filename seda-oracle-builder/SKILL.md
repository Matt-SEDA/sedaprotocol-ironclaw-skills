---
name: seda-oracle-builder
version: "0.2.0"
description: "Build, deploy (upload), and execute SEDA Oracle Programs using SEDA For-Agents docs with strict checkpoints and no guessing."
author: "SEDA"
license: "MIT"
tags:
  - seda
  - oracle-programs
  - agents
  - mcp
  - fast
  - core
  - data-proxy
---

# SEDA Oracle Program Builder (Agent Skill)

## Purpose
Guide a non-coder through building, deploying (uploading), and executing SEDA Oracle Programs using SEDA **For Agents** docs as the only source of truth.

## Canonical navigation (must follow)
Start and navigate only from these pages:
- Getting Started: https://docs.seda.xyz/home/for-agents/getting-started
- Agent Modules (index): https://docs.seda.xyz/home/for-agents/agent-modules
- Machine index (llms.txt): https://docs.seda.xyz/home/for-agents/llms-txt

Example module pages (do not hardcode others; follow Agent Modules links):
- Execute on SEDA Fast: https://docs.seda.xyz/home/for-agents/agent-modules/execute-on-seda-fast
- Deploy Oracle Program (Upload to SEDA Network): https://docs.seda.xyz/home/for-agents/agent-modules/deploy-oracle-program-upload-to-seda-network

## Hard rules (must follow)
1) Do not guess commands, IDs, endpoints, payload shapes, or URLs.
2) Do not construct URLs and do not fetch any `modules/*.md` file paths.
3) Use only the content reachable from the Canonical navigation pages above and the links they contain.
4) Never print secrets (Fast API key, mnemonic, private keys). If a command requires a secret, show `{{REDACTED}}`.
5) One step at a time: output the next action + copy/paste + checkpoint + what to paste back.
6) Always include a checkpoint and stop if it fails; request the exact output needed to diagnose.
7) Always ask for confirmation before any deploy/upload/post request action.
8) Never say “deploy to SEDA Fast.” Fast is execution only.
9) When referencing a module, always use the canonical GitBook URL from Agent Modules or llms.txt, and treat Source file: paths as labels only.

## Terminology (must use)
- Deploy / Upload = publish Oracle Program WASM to the **SEDA Network** using wallet + tokens (testnet or mainnet). Output: **Oracle Program ID**.
- Execute (Fast) = call Fast `/execute` with API key + Oracle Program ID + execInputs.
- Execute (Core) = post an onchain Data Request and await result.
- Data Proxy = a separate service to hold credentials and expose private/proprietary APIs to SEDA.

## Required safety behavior
- Treat mnemonics as high-risk. Never ask the user to paste a mnemonic into chat by default.
- Prefer testnet first.
- Warn the user: Fast execution can happen without tokens only when executing an existing OP; deploying a new OP still requires wallet+tokens.

---

## Mandatory Step 0: Environment Gate (choose execution mode)
Before any build/deploy/execution steps, determine which mode is possible.

### Gate questions (ask user first)
- “Do you have an MCP server connected (e.g., `seda-mcp`) that can run commands on your machine?”
- “Are you running commands on your local Mac, or inside a cloud shell?”

### Mode selection rules
Choose exactly one mode:

**Mode A: MCP Mode (preferred)**
Use if an MCP server is available that exposes build/deploy tools (env_check, clone/update, build, test, deploy, post-dr, fast execute).
- In this mode, do not output install instructions for bun/rust.
- Call MCP tools for execution and return tool outputs.
- Keep secrets out of chat; use local env/secret storage on the MCP host.

**Mode B: Local Mode (user runs commands)**
Use if the user will run commands on their Mac and paste outputs back.
- Output exact copy/paste commands from For-Agents pages only.
- Never suggest installing dependencies unless the For-Agents page explicitly requires it.

**Mode C: Cloud Shell Mode (avoid installs)**
Use only if the environment already has required toolchain (git, bun, rustc/cargo).
- Do not attempt `apt-get` or package installs unless explicitly allowed by the user and confirmed possible.
- If toolchain is missing, switch to Mode A or B.

### Environment check action
If in MCP Mode: call `env_check`.
If in Local/Cloud Mode: ask the user to run and paste:

```bash
which git bun node npm rustc cargo || true
git --version || true
bun --version || true
node --version || true
npm --version || true
rustc --version || true
cargo --version || true
