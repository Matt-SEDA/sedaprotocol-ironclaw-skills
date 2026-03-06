# SEDA Oracle Program Builder (Agent Skill)

## Purpose
Guide a non-coder through building, deploying, and executing SEDA Oracle Programs using the SEDA **For Agents** docs as the only source of truth.

## Root Source of Truth (must use)
Start here and follow links from this section only:
https://docs.seda.xyz/home/for-agents/getting-started

## Activation
Use this skill when the user asks to:
- build an Oracle Program
- deploy an Oracle Program
- execute via SEDA Fast or SEDA Core
- configure a Data Proxy (private APIs / website-to-API)
- verify prover deployments or use EVM Hardhat integration

## Hard rules (must follow)
1) Do not guess commands, IDs, endpoints, or payload shapes.
2) Use only the SEDA For Agents pages (root above) + direct subpages they link to.
3) Never print secrets (Fast API key, mnemonic, private keys). If a command needs a secret, show `{{REDACTED}}`.
4) One step at a time: output the next action + exact copy/paste commands + checkpoint.
5) Always include a checkpoint and stop if it fails. Ask for the output snippet needed to diagnose.
6) Always ask for confirmation before any deploy/upload/post request action.

## Required safety behavior
- Treat wallet mnemonics as high-risk. Never ask the user to paste mnemonics into chat unless explicitly required and the user agrees.
- Prefer testnet flows first.
- Warn the user that **Fast execution still requires tokens to deploy a new Oracle Program** (deployment is on SEDA Network).

## Default workflow (decision tree)
When user says “build an Oracle Program”:

A) Ask for two choices (only if unclear):
   1) Delivery: Fast or Core
   2) Data source type: supported feed / public API / private API / website-only

B) Route based on answers:
   - Supported feed → proceed to OP build or execution
   - Public API → require reading HTTP timeouts page before implementing fetch
   - Private/website → require Data Proxy runbook before OP logic

C) Execution plan:
   - Build + local test OP
   - Wallet + tokens (required for deployment)
   - Deploy OP (Core deploy step) to get Oracle Program ID
   - Execute:
     - Fast execute (API key) OR
     - Core post Data Request

D) Optional:
   - If EVM consumption required: verify prover exists via prover deployments page, then Hardhat integration.

## Output format (mandatory)
Every response must include:

### Next action
(1 sentence)

### Copy/paste
(Commands or exact instructions)

### Checkpoint
(What success looks like)

### Paste back
(Exactly what the user should paste back: command output snippet, URL, program ID, request ID, error)

## Minimal prompts to ask the user
Ask for only what you need, one at a time:
- Which path: Fast or Core?
- Which data source type?
- If Fast: Fast API key (never echo it)
- If deploying: confirmation + wallet/tokens readiness
- If Core deploy: confirmation + whether mnemonic is already set locally (never request it by default)

## Canonical subpages (expected to exist)
These are the pages you should use when linked from the root:
- Data source: /home/for-agents/modules/10-data-access
- HTTP fetch timeouts: /home/for-agents/modules/12-http-fetch-timeouts
- Data Proxy: /home/for-agents/modules/25-data-proxy
- Build + test OP: /home/for-agents/modules/20-build-op
- Wallet + tokens: /home/for-agents/modules/30-wallet-and-tokens
- Execute Fast: /home/for-agents/modules/40-execute-fast
- Execute Core: /home/for-agents/modules/50-execute-core
- Prover deployments: /home/for-agents/modules/60-prover-deployments
- EVM Hardhat: /home/for-agents/modules/60-relay-evm-hardhat

If any page is missing or inaccessible, stop and ask the user for the correct URL.
