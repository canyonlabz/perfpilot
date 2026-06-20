# ✈️ PerfPilot

> **An open-source AI co-pilot for the Performance Testing Lifecycle — from test creation to execution, monitoring, analysis, reporting, and delivery.**

> ⚠️ **This project is under active development.**
> Repository structure, APIs, setup instructions, Docker files, and documentation may change frequently while the platform is being assembled.

---

## 🚀 Overview

**PerfPilot** is an AI-assisted performance testing platform that brings together:

* 🤖 **Multi-agent orchestration** for performance testing workflows
* 🛩️ **PerfPilot Hub**, a unified MCP gateway for performance testing tools
* 🧪 **JMeter script generation and execution**
* 🔥 **BlazeMeter test execution and result collection**
* 📊 **Datadog metrics, logs, and APM correlation**
* 🧠 **PerfMemory**, a persistent AI memory layer for debugging and lessons learned
* 📄 **Performance analysis and reporting**
* 💬 **Collaboration integrations** such as Confluence, MS Teams, and SharePoint
* 🖥️ **A browser-based CopilotKit / React UI** for human-in-the-loop workflows

PerfPilot extends the original [`mcp-perf-suite`](https://github.com/canyonlabz/mcp-perf-suite) project into a broader platform that combines MCP tools, AI agents, an A2A server, an AG-UI backend, and a web frontend under one repository.

---

## 🎯 Vision

Performance testing often requires a human to move between many disconnected tools:

1. Capture or design a user flow
2. Generate a JMeter script
3. Debug correlation and test data issues
4. Execute a load test
5. Monitor infrastructure and application metrics
6. Analyze bottlenecks
7. Write a report
8. Publish results
9. Notify stakeholders

**PerfPilot** is designed to turn that fragmented process into an agent-assisted workflow.

The goal is not to remove the human from the process. The goal is to give performance engineers an AI co-pilot that can handle the repetitive work while keeping humans in control of consequential decisions such as launching tests, approving reports, and publishing results.

---

## 🧭 Repository Structure

```text
perfpilot/
├── agent-framework/       # AG2 agents, A2A server, AG-UI backend, CopilotKit frontend
│   ├── frontend/          # CopilotKit / React / Next.js web UI
│   └── backend/           # Python AG-UI server and A2A server
├── mcp-perf-suite/        # Gateway + all MCP servers
│   ├── gateway-mcp/       # PerfPilot Hub — unified MCP gateway via FastMCP
│   ├── blazemeter-mcp/    # BlazeMeter API tools
│   ├── confluence-mcp/    # Confluence publishing tools
│   ├── datadog-mcp/       # Datadog metrics, logs, and APM tools
│   ├── jmeter-mcp/        # JMeter script generation and execution tools
│   ├── perfanalysis-mcp/  # Performance analysis and correlation tools
│   ├── perfmemory-mcp/    # AI memory backed by PostgreSQL, pgvector, and Apache AGE
│   ├── perfreport-mcp/    # Report generation tools
│   ├── artifacts/         # Test artifacts, reports, JTLs, logs, and generated files
│   └── streamlit-ui/      # Web UI for viewing performance test results
├── docker/                # Compose files, Dockerfiles, and config templates
└── docs/                  # Public documentation
```

---

## 🧠 Core Concepts

### 🛩️ PerfPilot Hub

**PerfPilot Hub** is the central MCP gateway. Instead of connecting an AI agent to many separate MCP servers, PerfPilot Hub exposes the performance testing toolchain through one MCP endpoint.

It routes requests to specialized MCP servers such as JMeter, BlazeMeter, Datadog, PerfAnalysis, PerfReport, Confluence, PerfMemory, MS Teams, and SharePoint.

### 🤖 PerfPilot Agents

**PerfPilot Agents** is the multi-agent layer. It is designed around an orchestrator agent and specialist agents for each major phase of the Performance Testing Lifecycle.

Planned and/or evolving agents include:

| Agent                  | Purpose                                                         |
| ---------------------- | --------------------------------------------------------------- |
| 🎯 Orchestrator Agent  | Coordinates the full workflow and delegates work to specialists |
| 📝 Script Agent        | Generates or adapts performance test scripts                    |
| 🚀 Execution Agent     | Starts and monitors performance test execution                  |
| 📊 Monitoring Agent    | Pulls infrastructure, application, logs, and APM data           |
| 🔍 Analysis Agent      | Correlates test results with monitoring data                    |
| 📄 Reporting Agent     | Drafts performance test reports                                 |
| 📣 Notifications Agent | Sends summaries, links, and status updates to stakeholders      |

### 🖥️ PerfPilot Web UI

The Web UI is planned as a browser-based chat interface for interacting with the PerfPilot orchestrator and specialist agents.

It is built around:

* Next.js
* React
* CopilotKit
* AG-UI
* A Python backend
* Persistent conversation and workflow state

---

## 🏗️ High-Level Architecture

```text
┌──────────────────────────────────────────────────────────────────┐
│                          Human Users                             │
│                                                                  │
│  Browser UI / Cursor / Claude / Other AI Agent Frameworks        │
└───────────────────────────────┬──────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                       PerfPilot Agent Layer                      │
│                                                                  │
│  Orchestrator Agent                                              │
│     ├── Script Agent                                             │
│     ├── Execution Agent                                          │
│     ├── Monitoring Agent                                         │
│     ├── Analysis Agent                                           │
│     ├── Reporting Agent                                          │
│     └── Notifications Agent                                      │
│                                                                  │
│  Surfaces:                                                       │
│     - A2A server for agent-to-agent workflows                    │
│     - AG-UI backend for browser-based human interaction          │
└───────────────────────────────┬──────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                         PerfPilot Hub                            │
│                                                                  │
│  Unified MCP gateway exposing performance testing tools          │
└───────────────────────────────┬──────────────────────────────────┘
                                │
        ┌───────────────────────┼────────────────────────┐
        ▼                       ▼                        ▼
┌───────────────┐       ┌────────────────┐        ┌─────────────────┐
│ JMeter MCP    │       │ BlazeMeter MCP │        │ Datadog MCP     │
└───────────────┘       └────────────────┘        └─────────────────┘
        ▼                       ▼                        ▼
┌───────────────┐       ┌────────────────┐        ┌─────────────────┐
│ PerfAnalysis  │       │ PerfReport     │        │ PerfMemory      │
│ MCP           │       │ MCP            │        │ MCP             │
└───────────────┘       └────────────────┘        └─────────────────┘
        ▼                       ▼                        ▼
┌───────────────┐       ┌────────────────┐        ┌─────────────────┐
│ Confluence    │       │ MS Teams       │        │ SharePoint      │
│ MCP           │       │ MCP            │        │ MCP             │
└───────────────┘       └────────────────┘        └─────────────────┘
```

---

## 🔄 Example Workflow

A future end-to-end PerfPilot workflow may look like this:

1. A user submits a request through the Web UI, Cursor, Claude, or an upstream AI framework.
2. The Orchestrator Agent creates a performance testing plan.
3. The Script Agent generates or updates a JMeter script.
4. The human reviews and approves the generated script.
5. The Execution Agent starts a test through BlazeMeter or another load testing backend.
6. The Monitoring Agent collects metrics, logs, and traces during the test.
7. The Analysis Agent correlates load test results with application and infrastructure telemetry.
8. The Reporting Agent drafts an executive-friendly report.
9. The human reviews, revises, and approves the report.
10. The report is published to Confluence and/or archived to SharePoint.
11. Stakeholders are notified through MS Teams or another notification channel.

---

## 🧰 Technology Stack

| Layer                        | Technology                       |
| ---------------------------- | -------------------------------- |
| Agent framework              | AG2                              |
| Agent-to-agent communication | A2A                              |
| Tool protocol                | MCP                              |
| MCP gateway                  | FastMCP                          |
| Backend APIs                 | Python / FastAPI                 |
| Browser UI                   | Next.js, React, CopilotKit       |
| Database                     | PostgreSQL                       |
| Vector memory                | pgvector                         |
| Knowledge graph memory       | Apache AGE                       |
| Load testing                 | JMeter, BlazeMeter               |
| Observability                | Datadog                          |
| Reporting / collaboration    | Confluence, SharePoint, MS Teams |
| Local orchestration          | Docker Compose                   |

---

## 📦 Project Status

PerfPilot is currently being assembled from the original `mcp-perf-suite` project and the experimental agent-framework branch.

| Area                 | Status                                               |
| -------------------- | ---------------------------------------------------- |
| MCP Perf Suite       | Existing project being moved under the new root repo |
| PerfPilot Hub        | Existing MCP gateway concept from `gateway-mcp`      |
| Agent Framework      | Active development                                   |
| A2A Server           | Active development                                   |
| AG-UI Backend        | Active development                                   |
| CopilotKit Web UI    | Active development                                   |
| Docker Compose       | Planned / evolving                                   |
| Public documentation | In progress                                          |

---

## ▶️ Getting Started

> The repository is currently in early setup. Full installation instructions will be added as the folder structure stabilizes.

For now, the expected local development flow will be:

```bash
# Clone the repository
git clone https://github.com/canyonlabz/perfpilot.git
cd perfpilot
```

Then follow the setup instructions inside each major module:

| Module          | Path               | Purpose                                                            |
| --------------- | ------------------ | ------------------------------------------------------------------ |
| Agent Framework | `agent-framework/` | Multi-agent orchestration, A2A server, AG-UI backend, and frontend |
| MCP Perf Suite  | `mcp-perf-suite/`  | MCP gateway and specialized performance testing MCP servers        |
| Docker          | `docker/`          | Local containers, databases, and service orchestration             |
| Docs            | `docs/`            | Public documentation and architecture notes                        |

---

## 🗺️ Roadmap

Planned areas of work include:

* [ ] Move `mcp-perf-suite` into the new `perfpilot` repository
* [ ] Move the experimental `agent-framework` branch into the new root structure
* [ ] Normalize environment configuration across agents, MCPs, and Docker
* [ ] Add root-level Docker Compose orchestration
* [ ] Add one-command local startup for database, MCP gateway, agents, and UI
* [ ] Expand specialist agents beyond the first working vertical slices
* [ ] Add human-in-the-loop approval cards in the Web UI
* [ ] Add persistent multi-thread conversation history
* [ ] Add task progress streaming and test-run result views
* [ ] Add public architecture documentation
* [ ] Add contribution guidelines

---

## 🤝 Contributing

Contributions, ideas, and feedback are welcome.

This project is still early and moving quickly, so please expect breaking changes while the architecture stabilizes.

---

## 📜 License

This project is licensed under the MIT License.

See the `LICENSE` file for details.

---

Created with ❤️ for performance engineers, quality engineers, SREs, and AI-assisted testing workflows.
