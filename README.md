# Keraunos

### A Deterministic AI Agent System

**Architected & Engineered by Gregory Kennedy — Senior ML/AI Engineer**

---

> **Verified AI deliverables — guaranteed, not hoped for.**
> A deterministic, **blueprint-first** agent system — modeled on the Stripe Minions pattern that
> safeguards code supporting **>$1 trillion** in annual payment volume — that produces **audit-ready
> technical training courses** and **jurisdiction-verified legal documents** on infrastructure you
> control, at a **fixed** annual cost instead of runaway per-token bills. In a market where ~95% of
> enterprise AI pilots never ship, Keraunos ships **provably correct** output or escalates to a human.

---

*Κεραυνός — the thunderbolt of Zeus. A tool of absolute precision, striking exactly where it was aimed, every time, without exception.*

---

## The problem: most enterprise AI never ships — and the bill arrives anyway

The gap between AI demos and AI in production has become the defining risk in enterprise software:

- **~95% of enterprise generative-AI pilots deliver no measurable P&L return** — MIT Project NANDA, *The GenAI Divide: State of AI in Business 2025* (150 executive interviews, 350-employee survey, 300 public deployments).
- **More than 80% of AI projects fail — roughly twice the failure rate of conventional IT projects** — RAND Corporation.
- **42% of companies abandoned most of their AI initiatives in 2025, up sharply from 17% a year earlier** — S&P Global Market Intelligence, *Voice of the Enterprise*; nearly half of AI proofs-of-concept are scrapped before they ever reach production.

And the meter never stops. **Inference — not training — is now 80–90% of AI spend.** Agentic workflows cost **5–25× more than chat**, firing 10–20 model calls per task; enterprise AI spend has risen **~320% in two years** even as per-token prices fell. Teams are paying more for tokens than the work is worth — and *still* can't trust the result, because LLMs are **non-deterministic by nature** and produce plausible-but-wrong output that no regulated buyer can ship.

> *Sources: [MIT report via Fortune, Aug 2025](https://fortune.com/2025/08/18/mit-report-95-percent-generative-ai-pilots-at-companies-failing-cfo/); RAND Corporation; S&P Global *Voice of the Enterprise* 2025; [AI inference cost economics, 2026](https://www.techaheadcorp.com/blog/inference-cost-explosion/).*

## The insight Keraunos is built on: blueprint-first, model-second

The rare teams that run AI in production **at scale** don't trust the model — they wrap it in deterministic control. **Stripe's "Minions"** are the reference implementation: a *blueprint* alternates fixed, code-enforced steps (lint, compile, tests, template compliance) with bounded agentic steps, and the code those agents manage now supports **over $1 trillion in annual payment volume** ([InfoQ, 2026](https://www.infoq.com/news/2026/03/stripe-autonomous-coding-agents/); Stripe processed **$1.9 trillion** total in 2025). The lesson is unambiguous: **determinism — not a bigger model — is what makes AI shippable** in a high-stakes, regulated domain.

Gregory Kennedy spent years evaluating frameworks, models, and agent systems in the field. **Stripe's Minions blueprint pattern was the best-of-breed**, and Keraunos is a purpose-built adaptation of it — *"Stripe Minions for regulated knowledge work."*

## What Keraunos does

Keraunos is a **deterministic** AI agent system focused on two high-stakes, regulation-sensitive deliverables: **technical training courses** (slide decks, code labs, assessments, instructor notes) and **jurisdiction-verified legal documents**. The same deterministic pipeline extends cleanly to security-audit reports, regulatory compliance, and infrastructure-as-code.

Every output passes through **10 deterministic pipeline stages**. Seven of those stages are quality gates enforced by code — the AI cannot bypass, skip, or reorder them. When the AI fails to fix an issue after exactly two attempts, it **stops and asks a human**.

The system runs entirely on **your own infrastructure — or on a secure, sandboxed cloud infrastructure**. **No client data leaves your network.** It is built on **client-use-case-specific, custom fine-tuned, SOTA multi-modal open-source models** — not third-party cloud APIs.

### Why it's different

LLMs are **non-deterministic by nature.** The only reliable way to mitigate that risk is old-school **deterministic code and gates** — including **SHA-256-verified, screenshot-proof visual checks** — that verify every output against **client- and project-specific objective criteria before a human ever sees it.** Keraunos does not generate output and hope it is correct; it **proves** each deliverable against an explicit spec, or escalates to a human.

Because Keraunos runs self-hosted, fine-tuned open-source models instead of metered cloud APIs, it **sidesteps the inference-cost trap** that sinks API-first projects. Annual infrastructure and maintenance is estimated at **$40,000–$60,000** for our entry-level, small-to-medium-business, client-specific custom-fine-tuned automated setup, and **$100,000–$200,000** for our enterprise-grade, client-specific custom-fine-tuned automated setup — a **predictable, fixed cost in place of runaway per-token spend**, with data sovereignty included.

### Market

Keraunos leads with two beachhead verticals — **enterprise technical training** and **jurisdiction-verified legal drafting** — then expands along the *same* deterministic pipeline into security auditing, regulatory compliance, and infrastructure-as-code. Corporate e-learning / technical training and legal-tech / document automation are each multi-billion-dollar markets growing double digits; combined with the adjacent compliance and IaC verticals the pattern addresses, the reachable market **exceeds $15 billion**. The wedge is durable because in every one of these markets the buyer is regulated, cannot tolerate non-deterministic output, and cannot send data to a third-party cloud — **exactly the constraints Keraunos is engineered for.** A one-page business summary lives in [`docs/business/LEAN-CANVAS.md`](docs/business/LEAN-CANVAS.md).

### For engineers

Keraunos is accessed through the **Hermes WebUI** (browser) — the **sole** user interface — running on top of the **hermes-agent** backend TUI (the runtime/harness). Engineers/users run both **locally** on their own machines today; at pre-deployment to production they run on the VPS and the WebUI reaches the hermes-agent over standard HTTPS. (**Hermes One is retired — there is no fallback client.**) The agent harness is the critical orchestration layer that determines how reliably an AI system executes multi-step workflows. The Hermes Agent runtime provides native MCP integration, deterministic pipeline execution, multi-model support, and the Blueprint pattern that enforces quality gates the AI cannot bypass. This is not incidental — the harness is the product. The LLM is a component inside the harness, not the other way around.

Architecturally, Keraunos is a Stripe Minions-inspired pipeline built on the Hermes Agent runtime, with a self-hosted VPS backend providing Ollama model serving, a 64-tool MCP ecosystem, TursoDB/libSQL with native F32_BLOB vector search for RAG, and Valkey for caching. All orchestrated by Dokploy on a single Hostinger VPS. See `docs/hermes/SOUL.md` for the system's identity and ethical operating principles.

## Architecture

```
User's machine (Hermes WebUI)   VPS (Hostinger KVM 8, 32 GB RAM)
                                        Ubuntu 24.04 + Dokploy v0.29.8
┌──────────────────────┐              ┌──────────────────────────────────┐
│                      │              │  Dokploy (Docker Swarm + Traefik)│
│   Hermes WebUI       │              │                                  │
│   (primary client)   │              │  ┌────────────────────────────┐  │
│                      │    HTTPS     │  │  Traefik                   │  │
│   ● Chat             │─────────────>│  │  SSL termination           │  │
│   ● Skills           │              │  │  Let's Encrypt auto-certs  │  │
│   ● Extensions (MCP) │              │  └─────────┬──────────────────┘  │
│   ● Apps             │              │            │                     │
│   ● Scheduler        │              │  ┌─────────▼──────────────────┐  │
│   ● .hermeshints     │              │  │  Model Server: Ollama      │  │
│                      │              │  │  ~20 GB for models         │  │
│                      │              │  │  Custom fine-tuned + base  │  │
│                      │              │  │  ollama.yourdomain.cloud   │  │
│                      │              │  └────────────────────────────┘  │
│                      │    HTTPS     │                                  │
│                      │─────────────>│  ┌────────────────────────────┐  │
│                      │              │  │  Hermes Agent API          │  │
│                      │              │  │  (port 8642)               │  │
│                      │              │  │  Centralized agent loop    │  │
│                      │              │  │  hermes.yourdomain.cloud   │  │
│                      │              │  └────────────────────────────┘  │
│                      │    HTTPS     │                                  │
│                      │─────────────>│  ┌────────────────────────────┐  │
│                      │              │  │  Keraunos MCP Server       │  │
│                      │              │  │  64 tools live             │  │
│                      │              │  │  FastAPI + FastMCP          │  │
│                      │              │  │  mcp.yourdomain.cloud      │  │
│                      │              │  └─────────┬──────────────────┘  │
└──────────────────────┘              │            │                     │
                                      │            │ dokploy-network     │
                                      │            ▼                     │
No SSH required.                      │  ┌────────────────────────────┐  │
No VPN required.                      │  │  TursoDB / libSQL          │  │
Standard HTTPS only.                  │  │  Primary DB backend        │  │
                                      │  │  Native F32_BLOB vectors   │  │
                                      │  │  ~500 MB RAM               │  │
                                      │  └────────────────────────────┘  │
                                      │                                  │
                                      │  ┌────────────────────────────┐  │
                                      │  │  Valkey (Cache Layer)      │  │
                                      │  │  Session + rate-limit      │  │
                                      │  │  ~256 MB RAM               │  │
                                      │  └────────────────────────────┘  │
                                      │                                  │
                                      │  All services on dokploy-network │
                                      └──────────────────────────────────┘
```

### How the connections work

- **Ollama** exposes an HTTP API on port 11434. Traefik (managed automatically by Dokploy) terminates SSL and routes `ollama.yourdomain.cloud` to that port. The client's inference provider (Hermes WebUI) connects to this URL, while the `keraunos-mcp` container connects internally via `http://ollama:11434`. (Locally, the client connects to `http://localhost:11434`.)
- **Keraunos MCP Server** exposes an SSE endpoint on port 8001. Traefik routes `mcp.yourdomain.cloud` to it. The client connects via Extensions (MCP).
- **Hermes Agent API** exposes port 8642. Traefik routes `hermes.yourdomain.cloud` to it. The Hermes WebUI (the sole interface) connects to it. **The legacy OpenWebUI route has been decommissioned** — the root domain no longer routes to port 8080.
- **TursoDB / libSQL** is the active database backend (internal only, never exposed to the internet). The MCP server connects via `http://turso-db:8080` on the internal Docker network.
- **Valkey** provides in-memory caching for query results, sessions, and rate limiting, connected internally via `valkey:6379`.

> [!IMPORTANT]
> **Dokploy Routing Requirement:** In Dokploy, you **must** manually register these domains in the **Domains** tab. Without this step, Traefik will not route public traffic. See the full domain configuration in `KERAUNOS-SETUP-INSTALL-GUIDE-2026.md`.
>
> **NOTE:** The legacy root-domain route (`yourdomain.cloud` → port 8080) has been **permanently removed** (previously used by the decommissioned OpenWebUI service). Do not re-add it. The Hermes WebUI is the sole interface (Hermes One is retired).

### Infrastructure stack

| Layer | Technology | Purpose |
| :---- | :---- | :---- |
| Client | Hermes WebUI (sole UI, on the hermes-agent backend TUI) | Engineer/standard user interface: chat, agent tasks, extensions |
| PaaS | Dokploy v0.29.8 on Ubuntu 24.04 | Manages Docker Swarm, Traefik, SSL, deployments |
| Orchestration | Docker Swarm (managed by Dokploy) | Container scheduling, health checks, rolling updates |
| Agent runtime | Hermes Agent API (port 8642) | Centralized agent loop, MCP integration, skills |
| Database | TursoDB / libSQL | Primary backend with native F32_BLOB vector search (~500 MB RAM) |
| Cache | Valkey 8.x | Query result caching, sessions, rate limiting (~256 MB RAM) |
| Model Server | Ollama | Model serving with ~20 GB available for models |

### Resource budget (Hostinger KVM 8, 32 GB RAM)

| Service | RAM | Notes |
| :---- | :---- | :---- |
| Ollama models | ~20 GB | 3B model ≈ 10 GB · 8B model ≈ 20 GB |
| Keraunos MCP server | ~2 GB | FastAPI + FastMCP (64 tools) |
| TursoDB / libSQL | ~0.5 GB | Primary database backend |
| Valkey cache | ~0.5 GB | Query caching, sessions, rate limiting |
| Phoenix observability | ~1 GB | OpenTelemetry traces |
| System + Dokploy | ~1 GB | OS + PaaS overhead |
| **Headroom** | **~7 GB** | Safe buffer for burst workloads |

_Total ≈ 32 GB._

---

## Model server

Keraunos uses **Ollama** as the sole model-serving backend, deployed
via the `ollama` service in `deploy/docker-compose.yml` and accessed
internally by the Keraunos MCP server at `http://ollama:11434`.

### Ollama (production)

- **Image:** `ollama/ollama:latest` on port 11434
- **API:** Ollama native HTTP (`/api/tags`, `/api/generate`, etc.)
- **Model format: Ollama Pro registry models (`ollama pull`) and custom
  fine-tuned models via Ollama Pro pull + alias (`ollama pull` + `ollama cp`)
  Modelfile — no GGUF conversion needed)
- **Multi-model:** Yes — multiple models loaded simultaneously
- **Hermes WebUI:** Settings > Models > Ollama provider >
  `https://ollama.DOMAIN` (or `http://localhost:11434` locally)
- **Model management:** `ollama pull`, `ollama list`, `ollama create`,
  `ollama rm` — all via Docker exec
- **Custom models:** Ollama Pro pull + alias (see
  `docs/finetuning/Ministral-3-Deployment-Guide.md`)
- **Maturity:** Widely adopted, large community, well-documented

---

## MCP tool ecosystem

The Keraunos MCP server provides 64 tools that the hermes-agent invokes during pipeline execution. All tools live in `deploy/mcp-server/src/server.py`.

### 64 MCP tools

| Category | Count | Example Tools |
|----------|-------|---------------|
| Code Analysis | 8 | `analyze_codebase`, `trace_dependencies`, `extract_api_surface`, `calculate_complexity`, `diff_analysis`, `dead_code_finder`, `type_coverage`, `import_graph` |
| Project Management | 8 | `create_workspace`, `list_workspaces`, `save_artifact`, `load_artifact`, `pipeline_status`, `pipeline_history`, `compare_runs`, `export_project` |
| Security | 8 | `scan_dependencies`, `scan_secrets`, `scan_sast`, `check_permissions`, `validate_input_handling`, `check_tls_config`, `generate_security_report`, `suggest_remediations` |
| Testing | 7 | `run_tests`, `analyze_coverage`, `generate_unit_tests`, `generate_property_tests`, `generate_test_fixtures`, `mutation_testing`, `benchmark_performance` |
| Documentation | 6 | `generate_api_docs`, `generate_readme`, `generate_architecture_diagram`, `generate_changelog`, `explain_code`, `generate_inline_docs` |
| Infrastructure | 6 | `check_resources`, `pull_ollama_model`, `query_database`, `backup_database`, `check_service_health`, `list_ollama_models` |
| Quality | 6 | `run_linter`, `run_formatter`, `check_naming_conventions`, `check_error_handling`, `search_codebase`, `search_docs` |
| Knowledge / RAG | 4 | `index_repository`, `search_past_runs`, `find_similar_code`, `validate_dependencies` |
| Cost / Analytics | 5 | `estimate_cost`, `get_run_cost`, `get_monthly_spend`, `suggest_model_swap`, `find_function`, `detect_patterns` |
| Integration | 5 | `github_create_pr`, `github_list_issues`, `github_get_file`, `webhook_notify`, `deploy_to_dokploy` |

**Total: 64 tools.** See `AGENTS.md` for architectural context.

---

## Hermes WebUI — Primary Interface (Browser-Based)

Keraunos uses [`nesquena/hermes-webui`](https://github.com/nesquena/hermes-webui) as the **primary interface** for engineers and users. It provides full parity with the CLI experience in a browser — chat, sessions, workspace file browsing, model selection, skills, cron jobs, and voice input.

### Interface hierarchy

| Priority | Interface | URL | Voice input (STT) |
|----------|-----------|-----|---------------------|
| **Primary (today)** | Hermes WebUI (local) | `http://localhost:8787` | Browser Web Speech API — works with ANY chat model |
| **Pre-deploy** | Hermes WebUI (VPS) | `https://hermes-webui.${DOMAIN}` | Browser Web Speech API — same |
| CLI | hermes-agent backend TUI | Terminal | Optional local STT (`stt.provider: local`); Groq not used |

hermes-webui is the **sole** UI on top of the hermes-agent backend TUI. (Hermes One is retired — there is no fallback client.)

### Why the WebUI voice input is fully decoupled from the chat model

The WebUI's built-in, **free browser Web Speech API is our STT**, fully decoupled from the chat model — so voice input works with any chat model (Ollama Cloud, local Ollama, etc.) with no `/audio/transcriptions` 404 and no model switch. **Groq / Groq STT is not used anywhere in Keraunos** (team decision, 2026-06-21).

### Local WebUI — Quick Start

**Prerequisites:** Hermes Agent installed (`hermes --version`) + Python 3 + Git

```bash
# 1. Clone the WebUI to your home directory
git clone https://github.com/nesquena/hermes-webui.git ~/hermes-webui

# 2. Launch it
cd ~/hermes-webui && python3 bootstrap.py
# → Creates venv, starts server on http://localhost:8787, opens browser

# 3. Complete the onboarding wizard (set a password — any password is fine for local use)

# 4. Start chatting — uses your configured model from ~/.hermes/config.yaml

# 5. Click the mic button for voice input — browser transcribes locally (free, no 404)
```

**Browser compatibility:** Chrome, Edge, Safari (full voice mode). Firefox (TTS only, no STT).

### Local WebUI — Start/Stop Scripts

The project includes cross-platform scripts in `scripts/` for starting, stopping, and checking the WebUI status:

#### macOS / Linux

```bash
./scripts/start-webui.sh           # Start in background (daemon mode, opens browser)
./scripts/start-webui.sh --fg      # Start in foreground (Ctrl-C to stop)
./scripts/start-webui.sh --no-browser  # Start without opening browser
./scripts/stop-webui.sh            # Stop the WebUI
./scripts/status-webui.sh          # Check status
./scripts/status-webui.sh --logs   # Tail last 50 log lines
```

#### Windows

```cmd
scripts\start-webui.bat             REM Start in background (opens browser)
scripts\start-webui.bat --fg        REM Start in foreground (Ctrl-C to stop)
scripts\start-webui.bat --no-browser REM Start without opening browser
scripts\stop-webui.bat              REM Stop the WebUI
scripts\status-webui.bat            REM Check status
scripts\status-webui.bat --logs     REM Tail last 50 log lines
```

**What the scripts do:**
- `start-webui`: Checks prerequisites (Hermes Agent + WebUI clone), starts the server on `127.0.0.1:8787` (loopback only), waits for health check, opens browser
- `stop-webui`: Stops the server via `ctl.sh`, PID file, or port lookup (three fallback methods)
- `status-webui`: Shows PID, health check result, uptime, and available commands

**Environment variable overrides:**
- `HERMES_WEBUI_DIR` — WebUI clone location (default: `~/hermes-webui`)
- `HERMES_WEBUI_PORT` — port (default: `8787`)
- `HERMES_WEBUI_HOST` — bind address (default: `127.0.0.1` for security)

### VPS WebUI — Browser Access from Any Device

The VPS deployment includes a separate WebUI instance accessible at `https://hermes-webui.${DOMAIN}`. It runs in a Docker container on the VPS and shares the `dokploy-network` with the main stack.

**Deployment guide:** [`docs/june-2026-final-updates/Manual Setup for Hermes Web-UI via Dokploy & Traefik.md`](docs/june-2026-final-updates/Manual%20Setup%20for%20Hermes%20Web-UI%20via%20Dokploy%20%26%20Traefik.md)

**Files:** `deploy/hermes-webui/docker-compose.yml`, `deploy/hermes-webui/.env.example`, `deploy/scripts/deploy-hermes-webui.sh`.

**Key hardening:** Dokploy GUI domain registration (not raw Traefik labels), read-only bind mount of `/opt/hermes` per WebUI v0.51.84, `HERMES_WEBUI_TRUST_FORWARDED_FOR=1` for Traefik, `WANTED_UID/GID=1000` to match the project's `hermes-agent`.

**Impact on existing services:** **Zero.** The VPS WebUI is a separate Compose project. The existing `hermes-agent` flow is untouched.

**Zero interference between local and VPS WebUI:** The local WebUI (`localhost:8787`) and VPS WebUI (`hermes-webui.${DOMAIN}`) run on different machines, use different processes, and share zero state. They can both run simultaneously.

### Headroom Context Compression (Optional)

[Headroom](https://github.com/chopratejas/headroom) compresses tool outputs, logs, and RAG chunks before they reach the LLM — 60–95% fewer tokens with the same answers. It runs locally as a proxy alongside the WebUI and is **selective**: pipeline validation stages (Quality Check, Security Scan) receive full uncompressed data.

**Install:**
```bash
uv tool install "headroom-ai[proxy]" --no-build
```

**Start (with pipeline validation tools excluded from compression):**
```bash
./scripts/start-headroom.sh    # Starts proxy on port 8788
hermes config set model.base_url "http://127.0.0.1:8788/v1"
hermes config set compression.enabled false
./scripts/stop-webui.sh && ./scripts/start-webui.sh
```

**Stop (restore direct Ollama Cloud):**
```bash
./scripts/stop-headroom.sh
hermes config set model.base_url "https://ollama.com/v1"
hermes config set compression.enabled true
./scripts/stop-webui.sh && ./scripts/start-webui.sh
```

**Excluded tools (not compressed):** Pipeline validation tools (`run_linter`, `scan_sast`, `scan_dependencies`, `scan_secrets`, `check_permissions`, etc.) are excluded via `HEADROOM_EXCLUDE_TOOLS` to preserve exact data for deterministic quality gates.

**VPS impact:** None. Headroom runs on the local Mac only. The VPS Hermes Agent is completely unaffected.

### Hermes One — retired

Hermes One Desktop is **retired**. There is no fallback client. hermes-webui is the sole UI, running
on top of the hermes-agent backend TUI (locally today, on the VPS at pre-deploy). The local-first
features formerly built for Hermes One now live in `deploy/local/` (local hermes-agent + hermes-webui;
embedded libSQL, diskcache, local MCP). See `deploy/local/README.md` for the isolation + parity contract.

### Local hermes-agent (CLI/TUI) — offline & air-gapped operation

Engineers can also install the **Hermes Agent runtime on their own machine** (the same `hermes` CLI/TUI the VPS runs). It sits below the WebUI and Desktop app as a third fallback tier, useful when:

- **The VPS or network is down** — keep working; the agent runs entirely on your machine.
- **Air-gapped / maximum data sovereignty** — point it at a *local* Ollama holding a local copy of `ministral-courseware`; nothing leaves the machine.
- **Power use** — full terminal control, scripting, and background `delegate_task` from the shell.

**How it stays in sync with the VPS**

1. **Shared governed profile (config, skills, SOUL, MCP connections) via git.** Install/refresh the `keraunos-technical-course` profile distribution:
   ```bash
   hermes profile install github.com/techcwbldr26/keraunos-technical-course
   hermes profile update keraunos-technical-course   # pull the latest
   ```
   The distribution ships SOUL + config + skills + cron + MCP connections. **Credentials, memories, and sessions stay per-machine** (never synced) — so the local agent matches the team's governed setup without sharing private state.
2. **Point it at the same VPS backend** so it uses identical models/tools/data. In `~/.hermes/config.yaml`: Ollama provider → `https://ollama.${DOMAIN}`, keraunos-mcp SSE extension → `https://mcp.${DOMAIN}/sse` (X-API-Key), DB → the VPS Turso. Workspace artifacts then live on the VPS (`/workspaces` + Turso); files the agent edits in a remote sandbox are synced back to your machine on session teardown.
3. **Match the version of record.** Pin hermes-agent to `VERSIONS.md` (0.17.0) and run `scripts/version-check.sh` — a local agent on a different version can behave differently (see `FEATURE-F-005-version-pinning.md`).

> **Governance:** for **client/course work** the local agent must run the governed `keraunos-technical-course` profile (local `ministral-courseware`, guards ON) — never a cloud model. Air-gapped/offline use keeps all data on-network by definition.

---

## Custom fine-tuned models

Keraunos uses custom fine-tuned open-source models hosted on Ollama Pro. The current production model is `ministral-courseware`, aliased from `philnetlan/ministral-3-3b-q8_256k` (GGUF Q8_0, 3B params, ~3.7 GB), optimized for technical courseware extraction.

### Production Model: `ministral-courseware`

| Property | Value |
|----------|-------|
| Ollama Pro source | `philnetlan/ministral-3-3b-q8_256k` |
| Size | ~3.7 GB (Q8_0) |
| Params | 3B |
| Ollama name | `ministral-courseware` (alias) |

### Deployment

```bash
# Pull from Ollama Pro and alias to ministral-courseware on the VPS
VPS_HOST="root@187.124.144.141" ./deploy/scripts/import-ministral-model.sh

# Or manually:
ssh root@187.124.144.141
OLLAMA_CTR=$(docker ps --filter "name=ollama" -q | head -1)
docker exec $OLLAMA_CTR ollama pull philnetlan/ministral-3-3b-q8_256k
docker exec $OLLAMA_CTR ollama cp philnetlan/ministral-3-3b-q8_256k ministral-courseware
```

After import, the model appears in hermes-webui at **Settings > Models**.

---

## 📊 Observability

- **Arize Phoenix Dashboard:** https://phoenix.technicalcoursewarebuilder.cloud
  - Traces every MCP tool call and LLM inference via OpenTelemetry OTLP
  - OTLP endpoint: `http://phoenix:6006/v1/traces` (OTLP/HTTP, not gRPC)
  - Version: Arize Phoenix v17.5.0
  - See `docs/phoenix/PHOENIX-INTEGRATION-GUIDE.md`

## Getting the code (clone — do NOT download the ZIP)

> 👋 **Non-technical / first-timer? Read this first.** GitHub's **"Download ZIP"**
> gives you a folder named `keraunos-main` that is **not** a git repository (it has
> no hidden `.git`), so `git status` will fail with
> `fatal: not a git repository`. You must **clone** instead. Keraunos is a
> **private** repo, so cloning asks for a GitHub username + a **Personal Access
> Token (PAT)** — not your password. Create a PAT at GitHub → **Settings →
> Developer settings → Personal access tokens** (scope: `repo`).

**1. Check git is installed** (all platforms):
```bash
git --version
```
If it says "command not found":
- **macOS:** `xcode-select --install` (or `brew install git`)
- **Windows:** install **Git for Windows** from https://git-scm.com/download/win (gives you "Git Bash")
- **Linux:** `sudo apt install git` (Debian/Ubuntu) or `sudo dnf install git` (Fedora)

**2. Clone into a folder you like:**

- **macOS / Linux** (Terminal):
  ```bash
  cd ~/Desktop
  git clone https://github.com/techcwbldr26/keraunos.git
  cd keraunos
  git status        # should print: On branch main
  ```
- **Windows** (Git Bash or PowerShell):
  ```powershell
  cd $HOME\Desktop
  git clone https://github.com/techcwbldr26/keraunos.git
  cd keraunos
  git status        # should print: On branch main
  ```

When prompted, enter your GitHub **username** and **PAT**. (Alternatives:
`gh auth login` with the GitHub CLI, or an SSH key →
`git clone git@github.com:techcwbldr26/keraunos.git`.)

**3. Open the cloned folder in VS Code:** File → **Open Folder** → select the new
`keraunos` folder (NOT the old `keraunos-main` ZIP folder — you can delete that).
In VS Code, **Terminal → New Terminal** opens inside the repo, so `git status`
works there too.

> The deployment runbook (`docs/june-2026-final-updates/02-FINAL-DEPLOYMENT-STEP-BY-STEP-PLAN.md`)
> starts from here: **Phase A on your machine**, then **SSH to the VPS** for the rest.

## Quick start: VPS deployment

### Prerequisites

- Hostinger KVM 8 VPS (or any VPS with 32 GB or more RAM)
- Ubuntu 24.04 with Dokploy installed (one-click template)
- A domain with DNS managed in Hostinger hPanel

### Step 1: DNS records

In Hostinger hPanel, add A records pointing to your VPS IP:

| Type | Name | Content |
| :---- | :---- | :---- |
| A | @ | your VPS IP |
| A | hermes | your VPS IP |
| A | hermes-webui | your VPS IP |
| A | ollama | your VPS IP |
| A | mcp | your VPS IP |
| A | phoenix | your VPS IP |
| A | dokploy | your VPS IP |

### Step 2: Dokploy setup

Access the Dokploy dashboard at `http://your-vps-ip:3000`. Create an admin account. In Settings, enter your domain and enable HTTPS with Let's Encrypt.

### Step 3: Upload and deploy

Upload the `deploy/` directory and run the init script:

```bash
scp -r deploy/ root@your-vps-ip:/root/keraunos/
ssh root@your-vps-ip
find /root/keraunos/deploy -type d -exec chmod 755 {} \;
find /root/keraunos/deploy -type f -exec chmod 644 {} \;
chmod +x /root/keraunos/deploy/scripts/*.sh
./deploy/scripts/init-secrets.sh --auto
```

### Step 4: Deploy the stack

1. In the Dokploy Dashboard, create a new **Stack** project (NOT Compose for production)
2. Paste the contents of `deploy/docker-compose.yml`
3. Set non-secret environment variables (DOMAIN, DATABASE_BACKEND=turso, etc.)
4. **Do NOT set secret variables** in the Environment tab — use Docker Swarm secrets
5. Configure domains in the Domains tab
6. Click Deploy

Services: `turso-db`, `ollama`, `keraunos-mcp`, `hermes-agent`, `phoenix`, `valkey`, `model-bootstrap`

### Step 5: Verify

```bash
curl https://ollama.yourdomain.cloud/api/tags
curl https://mcp.yourdomain.cloud/health
curl https://hermes.yourdomain.cloud/health
./deploy/scripts/health-check.sh
```

### Step 6: Connect hermes-webui

**Local (today):** run `deploy/local/install-local.sh`, start the local MCP (`keraunos-local-mcp`),
then launch hermes-webui on `127.0.0.1:8787` (see `deploy/local/README.md`).

**VPS (pre-deploy):** open `https://hermes-webui.yourdomain.cloud`; it auto-detects the hermes-agent on
the dokploy-network. To point a local hermes-agent at the VPS MCP, set `MCP_TARGET=vps` and use
`https://mcp.yourdomain.cloud/sse` with the `X-API-Key` (see `deploy/local/hermes-config.local.yaml`).

> **The legacy OpenWebUI service has been decommissioned.** Users access Keraunos via the Hermes WebUI — the sole interface (Hermes One is retired).

---

## Quick start: local development

### Prerequisites

- Node.js 24+ / Python 3.13+ / Rust 1.95.0+ / hermes-agent installed (see `deploy/local/`)

### Run locally

```bash
# Ollama (optional, for offline dev)
curl -fsSL https://ollama.com/install.sh | sh
ollama pull philnetlan/ministral-3-3b-q8_256k

# MCP server
cd deploy/mcp-server
pip install -r requirements.txt
uvicorn src.server:app --host 0.0.0.0 --port 8001
```

Add as a hermes-agent MCP extension: SSE, `http://localhost:8001/sse`.

---

## Environment Scripts (UV Virtual Environment)

Keraunos includes `start.sh` and `stop.sh` scripts that automatically create and manage an **Astral UV** virtual environment with all dependencies pre-installed. This isolates Python packages and prevents the system-wide Python conflicts and warnings you would otherwise encounter during model management tasks.

### Prerequisites

- [Astral UV](https://docs.astral.sh/uv/getting-started/installation/) installed (`brew install uv` on macOS)

### Start the environment

| OS | Command |
|:---|:--------|
| **macOS / Linux** | `./start.sh` |
| **Windows (Git Bash or WSL)** | `bash start.sh` |

The script will:
1. Verify UV is installed
2. Create `.venv/` if it does not exist
3. Install `huggingface-hub`, `requests`, `pyyaml`, `tqdm`
4. Source your `.env` file
5. Print the exact `source` command to activate the virtual environment

**Example output:**
```
$ ./start.sh
[INFO] Checking for Astral UV...
[OK] UV found: uv 0.5.15
[INFO] Creating UV virtual environment at /Users/.../Keraunos/.venv...
[OK] Virtual environment created.
[INFO] Installing Keraunos dependencies via UV...
[OK] Dependencies installed.

╔══════════════════════════════════════════════════════════════════╗
║           KERAUNOS AI ENVIRONMENT READY                          ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  To ACTIVATE the virtual environment, run:                         ║
║      source .venv/bin/activate                                   ║
║                                                                  ║
║  To DEACTIVATE, run:                                             ║
║      deactivate                                                  ║
║                                                                  ║
║  Or use the provided stop.sh script:                             ║
║      ./stop.sh                                                   ║
║                                                                  ║
║  Or use the provided stop.sh script:                             ║
║      ./stop.sh                                                   ║
║                                                                  ║
╚══════════════════════════════════════════════════════════════════╝
```

### Activate manually (after running `start.sh`)

| OS | Command |
|:---|:--------|
| **macOS / Linux (bash/zsh)** | `source .venv/bin/activate` |
| **macOS / Linux (fish)** | `source .venv/bin/activate.fish` |
| **Windows (PowerShell)** | `.venv\Scripts\Activate.ps1` |
| **Windows (CMD)** | `.venv\Scripts\activate.bat` |

### Stop and clean up

| OS | Command | Effect |
|:---|:--------|:-------|
| **macOS / Linux** | `./stop.sh` | Deactivates venv, cleans temp files |
| **macOS / Linux** | `./stop.sh --clean` | **Also removes `.venv/` entirely** |
| **Windows (Git Bash or WSL)** | `bash stop.sh` (add `--clean` to remove `.venv/`) | Same as above |

> **Why UV instead of `python -m venv`?** UV is 10–100× faster than pip, produces reproducible lockfiles, and handles dependency resolution without the conflicts common in older tools. It is now the standard package manager in the Python ecosystem.

---

## Configuration

### Environment variables

Set **non-secret** variables in the Dokploy Environment tab. Secret variables are managed via Docker Swarm secrets (`deploy/scripts/init-secrets.sh`).

| Variable | Required | Set in Dokploy? | Description |
| :---- | :---- | :---- | :---- |
| `DOMAIN` | Yes | ✅ Yes | Your domain (e.g., yourdomain.cloud) |
| `DATABASE_BACKEND` | Yes | ✅ Yes | Set to `turso` (active production backend) |
| `TURSO_DATABASE_URL` | Yes | ✅ Yes | TursoDB endpoint (e.g., `http://turso-db:8080`) |
| `OLLAMA_STARTUP_MODELS` | No | ✅ Yes | Comma-separated list of models to pull on startup |
| `LOG_LEVEL` | No | ✅ Yes | Logging level (`INFO` or `DEBUG`) |
| `TURSO_AUTH_KEY` | Yes | ❌ No | Use `turso_auth_key` Swarm secret |
| `MCP_API_KEY` | Yes | ❌ No | Use `mcp_api_key` Swarm secret |
| `GITHUB_TOKEN` | No | ❌ No | Use `github_token` Swarm secret |

### Critical settings

- `OLLAMA_ORIGINS=*` must be set in the Ollama service environment (in docker-compose.yml) for browser (hermes-webui) CORS access
- Traefik labels must be under `deploy.labels` (Docker Swarm mode), not top-level `labels`

### hermes-webui / hermes-agent settings

| Setting | Value |
| :---- | :---- |
| Models > Provider | Ollama |
| Models > Host | `https://ollama.yourdomain.cloud` |
| Extensions > keraunos-mcp | SSE, `https://mcp.yourdomain.cloud/sse` |

### Skills (slash commands)

| Command | Skill | Description |
| :---- | :---- | :---- |
| `/course` | course-builder | Technical training course generation |
| `/blueprint` | blueprint-build | Code generation pipeline |
| `/legal` | legal-document | Legal document drafting |
| `/lint-fix` | lint-fix | Bounded lint and fix (max 2 rounds) |
| `/security-audit` | security-audit | Security scanning |
| `/deploy-check` | deploy-check | VPS health verification |

Skills are SKILL.md files in `deploy/hermes/skills/` loaded on demand by the hermes-agent. Context files (`AGENTS.md`, `.hermeshints`) provide project-wide configuration that shapes every conversation.

> **Skill-guard on (Rev 17, 2026-06-15).** The 6 skills above are hand-curated and protected. Hermes Agent must NOT auto-create skills — whether connected to Keraunos or not. Two upstream config gates are ON in `deploy/hermes/hermes-config.yaml`: `skills.guard_agent_created: true` (content scanner) and `skills.write_approval: true` (every `skill_manage` write is staged under `~/.hermes/pending/skills/` for human review via `/skills pending`, `/skills diff <id>`, `/skills approve <id>`, `/skills reject <id>`). The same gate is wired for memory writes (`memory.write_approval: true`). See `docs/june-2026-final-updates/01-AGENT-SKILL-GUARD-TASK-LIST.md`.

> **Bundled-skills blank-slate (Rev 19, 2026-06-17).** The hermes-agent runtime runs a curated, minimal bundled-skills profile. The default ~90 bundled skills shipped with Hermes Agent v0.17.0 were reduced to 18 relevant skills (github workflows, software development, hermes-agent, huggingface-hub, autonomous agent delegation). A `.no-bundled-skills` marker prevents `hermes update` from re-seeding deleted skills. `skills.auto_sync_bundled: false` blocks newly-added upstream skills from auto-installing. `curator.prune_builtins: false` exempts bundled skills from curator auto-archival. The 11 project-specific skills (10 in `skills/`, 1 in `deploy/hermes/skills/`) are repo-managed, hand-authored, and completely unaffected by the opt-out. See `docs/june-2026-final-updates/01-SKILLS-BLANK-SLATE-TASK-LIST.md`.
>
> **Note on curator pinning:** `hermes curator pin` only works for **agent-created** skills, not bundled skills. The correct mechanism to protect bundled skills from curator archival is `curator.prune_builtins: false`, which exempts all bundled skills from the curator's auto-archive lifecycle. Attempting to pin a bundled skill will return: `"is bundled or hub-installed — cannot pin (only agent-created skills participate in curation)"`.

---

## Key design decisions

### Why the Hermes Agent runtime

The agent harness is the most consequential architectural decision in any AI agent system. It determines execution reliability, tool integration quality, and failure handling. The Hermes Agent runtime provides native MCP integration, deterministic pipeline execution, multi-model support, and the Blueprint pattern that enforces quality gates the AI cannot bypass. No other harness offers this combination.

Users connect through the browser **Hermes WebUI** — the sole UI — running on top of the hermes-agent backend TUI (locally today, on the VPS at pre-deploy), with chat, skills, MCP extensions, sandboxed apps, and a scheduler. Keeping the heavy VPS UI off engineers' machines when running against the VPS frees RAM for larger Ollama models. (Hermes One is retired.)

The Hermes Agent's `skill_manage` tool is gated by the **Hermes Agent Skill Guard** in `deploy/hermes/hermes-config.yaml` — agent-driven skill and memory writes are staged for human review, never auto-landed.

### Why Dokploy instead of raw Docker

Dokploy provides a web dashboard for managing Docker Swarm, Traefik routing, SSL certificates, environment variables, and deployments. It removes the need to SSH into the VPS for routine operations. Dokploy manages Traefik automatically — no separate Traefik container to maintain.

### Why Docker Swarm mode

Dokploy uses Docker Swarm for container orchestration. Traefik labels must be placed under `deploy.labels` (not top-level `labels`), and services require `placement.constraints: [node.role == manager]`. This is the most common source of deployment errors.

### Why self-hosted

Client data sovereignty is a non-negotiable requirement for legal, financial, and healthcare applications. No client data leaves the VPS. The system can operate fully air-gapped using local Ollama models with zero internet connectivity.

---

## Project structure

```
keraunos/
├── AGENTS.md                          # Root context file (architecture, rules)
├── README.md                          # This document
├── env.example                        # Environment variables template
├── .hermeshints                       # Hermes project rules
│
├── deploy/                            # Production deployment
│   ├── docker-compose.yml             # Main Dokploy Swarm compose
│   ├── .env.example                   # Environment template
│   ├── hermes/                        # Hermes agent configuration
│   ├── mcp-server/                    # FastAPI + FastMCP server (64 tools)
│   └── scripts/                       # deploy.sh, health-check.sh, init-secrets.sh
│
├── deterministic-engine/              # Rust-based deterministic pipeline
├── non-deterministic-engine/          # TypeScript LLM reasoning wrapper
│
├── skills/                            # Domain knowledge modules (10 skills)
│
├── graphify-out/                      # Knowledge graph (v0.8.44)
│   ├── graph.html                     # Interactive visualization (3,780 nodes)
│   ├── graph.json                     # Raw graph data (GraphRAG-ready)
│   ├── GRAPH_REPORT.md                # Audit report with god nodes + surprising connections
│   └── manifest.json                  # File manifest for incremental updates
│
└── docs/                              # Documentation, audits, research
    ├── deploy/                        # Deployment guides
    ├── turso/                         # TursoDB reference docs
    └── use-cases/                     # Vertical use case documentation
```

---

## The pipeline: 10 stages

Every Keraunos workflow follows the same pattern regardless of the domain. Seven stages are deterministic (enforced by code). Three stages use bounded LLM reasoning with strict validation on input and output.

```
 1          2             3            4            5
 Context    Planning      Validation   Scaffolding  Generation
 assembly   (LLM)        (code)       (code)       (LLM)
    ■           ◆             ■            ■            ◆
                              │
 10         9             8            7            6
 Packaging  Testing       Security     Fix loop     Quality
 (code)     (code)        (code)       (LLM, max 2) check (code)
    ■           ■             ■            ◆            ■

 ■ = Deterministic (code, AI cannot bypass)
 ◆ = Non-deterministic (bounded LLM, validated output)
```

The fix loop at stage 7 is the critical safety mechanism. The AI gets exactly two attempts to resolve issues found by the quality check or security scan. After two failures, the pipeline reports `ESCALATED` status and routes the problem to a human. The AI never silently ships broken output.

---

## Roadmap

### Delivered

- 10-stage deterministic pipeline with max 2 fix rounds
- 64 production MCP tools in FastAPI + FastMCP server
- 6 Keraunos pipeline skills with portable SKILL.md definitions
- 7 additional domain knowledge skills with 12 eval test cases
- Bundled-skills blank-slate management (18 of ~90 bundled skills kept, 75 removed, opt-out marker + auto_sync_bundled:false + prune_builtins:false)
- Dokploy Docker Swarm deployment
- Custom fine-tuned model (`ministral-courseware`) deployed via Ollama Pro pull + alias
- TursoDB/libSQL primary backend with native F32_BLOB vector search + Valkey caching
- Database abstraction layer (`db.py`) — Turso/libSQL only
- KERAUNOS-SETUP-INSTALL-GUIDE-2026.md (comprehensive deployment guide)
- AGENTS.md project context with full architecture documentation

### In progress

- Standalone demo app for funder presentations
- Git auto-commit at each pipeline stage

### Planned

- Parallel execution (one container per component)
- Context pre-hydration via TursoDB DiskANN vector search
- Adaptive model selection (auto-route based on task complexity)
- IDE extensions / MCP tool marketplace

---

## Knowledge Graph (graphify v0.8.44)

Keraunos uses [graphify](https://github.com/safishamsi/graphify) — an AI-powered knowledge graph pipeline — to map the codebase into a navigable structure of entities, relationships, and clustered communities. The graph is rebuilt on code changes via `graphify update .` and serves as a structural map for LLM-based agent navigation.

### Quick Facts (code graph rebuilt 2026-06-19)

| Metric | Value |
|--------|-------|
| Nodes | 3,780 |
| Edges | 4,229 |
| Communities | 293 |
| Extraction quality | 99% EXTRACTED, 1% INFERRED |
| Files indexed | 218 files, ~370,500 words |
| Install | `uv tool install graphifyy` (v0.8.44) |

> Counts above are from the 2026-06-19 code-only rebuild (AST, no LLM cost).
> Community **names** still date from the 2026-06-15 Gemini labeling run; a
> semantic re-label (`graphify label . --backend gemini`, needs an LLM key) is
> pending to refresh names and prune references to renamed doc paths.

### God Nodes (core abstractions)
1. `TursoMemoryStore` — 46 edges
2. `Configuration` — 45 edges
3. `AsyncWriter` — 22 edges
4. `compilerOptions` — 22 edges
5. `Keraunos` — 19 edges
6. `SqliteVecDB` — 17 edges

### Surprising Connections
- `WriteTask` → `TursoMemoryStore` (turso-memory-plugin, INFERRED)
- `TestIntegrationOllamaEmbedding` → `TursoMemoryStore` (test → prod, INFERRED)
- `TestFullPluginLifecycle` → `TursoMemoryStore` (smoke test → store, INFERRED)
- deploy `turso_memory/writer.py` ↔ desktop `turso-memory-plugin/store.py` (cross-tree reuse)

**Update the graph:** `graphify update .` (code-only, no LLM cost)  
**Full rebuild:** `/graphify .` via the graphify skill (`skills/graphify/SKILL.md`)

---

## License

Apache 2.0

---

**Gregory Kennedy ML/AI Engineer** | Keraunos Deterministic AI Agent System | May 2026
*My Sources: [Archix](https://arxiv.org/) Research Papers Numerous (to many to count), Stripe Dev Blog and Team Minions: Stripe’s one-shot, end-to-end coding agents: Part 1 - https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents Part 2- https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents-part-2, Linkedins Erran Berger (VP of Product Engineering) https://youtu.be/ErDS9TIQoWU?si=58DcaqFzbJvRZW9g, Anthropic Dev Blog, Vercel Dev Blog, OpenHands Dev Blog, Block Goose, +++*

Profound Gratitude to all of the OG women and men mathematicians, engineers, scientists and researchers from The 9th-century Persian mathematician Muhammad ibn Mūsā al-Khwārizmī who is widely credited as the inventor of the algorithm (The word "algorithm" is a direct, albeit slightly altered, derivative of "Al-Khwarizmi"), to Ada Lovelace (19th Century/1843): Credited with creating the first algorithm designed to be processed by a machine, and the numerous others who paved the way for me, and all of us.  Gregory 
