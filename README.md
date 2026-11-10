# Ovia Inc — A2UI (fork of a2ui-project/a2ui)

<img src="docs/public/assets/a2ui_gallery_examples.png" alt="Ovia Inc — gallery of A2UI rendered cards from the upstream project assets, showing a variety of agent-generated interface compositions" width="100%"/>

We maintain this fork of [a2ui-project/a2ui](https://github.com/a2ui-project/a2ui), the open-source Agent-to-User Interface project. Upstream credit: the a2ui-project team and contributors. Licence: Apache-2.0 (see LICENSE), preserved as received.

**Our main site:** https://oviainc.github.io/ai-services/

**Upstream project site:** https://a2ui.org/

## What this is

A2UI gives agents a way to present rich, interactive interfaces instead of plain text alone. An agent sends a declarative JSON description of the interface intent — a flat list of components with data binding — and the client application renders that description with trusted native components from a pre-approved catalog. The upstream description sums the result up as safe like data, but expressive like code: no arbitrary generated code runs in the client, and the same payload can render across frameworks.

The repository carries the specification and the surrounding implementation surface: renderers and client work for web and mobile frameworks — upstream lists React, Angular, Lit, Flutter, SwiftUI and Jetpack Compose — agent SDKs, catalogs, blueprints, conformance material, documentation, and worked examples, including a restaurant finder quickstart and tools for composing and stepping through A2UI payloads. At the fork point, upstream described the production release as v0.9.1 in the stable v0.9 protocol family, with the v1.0 specification as a release candidate and v0.8 as legacy, and the project as an early-stage public preview with an evolving specification.

## Story

Upstream opened the project under the a2ui-project organization on September 24, 2025, to solve a concrete gap in agent work: models handle text and code well, while rich presentation — forms, cards, dashboards, approvals — is harder, especially when an agent is remote or crosses a trust boundary. The design answers that gap with four commitments set out in the upstream README: security first through a declarative format and a trusted component catalog, an LLM-friendly flat component model that can update incrementally and render progressively, framework-agnostic portability from a single payload, and an open registry that lets developers map server-side types to custom client components with sandboxing under developer control.

We forked the project into the Oviainc collection on September 17, 2026, at the September 16, 2026 upstream snapshot. The fork sits in our collection in the coding-agent tooling group, beside the terminal, context, and database tooling forks we evaluate side by side. Where those projects run an agent or feed an agent, this one covers the last step in the chain: how an agent shows work to a person and asks a person for the next input, as a real interface rather than a wall of text.

What we evaluate here is the protocol and the rendering model — how a declarative payload stays expressive without becoming executable code, how a component catalog and registry keep trust with the client developer, how incremental updates and data binding behave in a live conversation, and how one payload carries across web, desktop, and mobile renderers. Transport fit also matters: upstream describes compatibility with agent-to-agent and agent-user protocols, and generation by any model that can produce JSON.

Where the project fits our work:

- **AI agents** — the core fit. Agent services need a presentation layer: approvals, forms, status, and results a person can act on. A2UI is a leading open candidate for that layer.
- **Context engineering** — an interface payload is context made visible: the components, values, and choices presented shape what a person reviews and what returns to the agent. Catalog design and incremental updates are context decisions.
- **Business intelligence and data** — dashboards and data collection are named upstream use cases: an agent generates an approval view or a visualization from a query, and a bespoke form gathers the exact inputs a workflow needs. Both map directly to BI delivery.
- **Virtual production** — production review, asset selection, and sign-off are interface-heavy, often remote, and cross tools on different frameworks. A portable, declarative interface format that renders natively on each client fits that shape of work.

## Modifications by Ovia Inc

Documentation only. Upstream code and content are unchanged from the fork point, except for documentation notes added in November 2026: a modification reference on November 4, 2026, replaced by this expanded treatment (Ovia section, story, and picture) on November 10, 2026. Where a later commit in this repository says otherwise, that commit governs.

## Sync status

This fork carries documentation commits of our own on top of the September 16, 2026 fork-point snapshot (upstream commit 0086493, "clarify spec"), and upstream is 664 commits ahead at the time of this pass, with the production line and the v1.0 specification work still moving. No sync has been applied on this pass: with a specification still stabilizing and renderer work moving quickly, a sync is a deliberate step, taken after a review of what changed in the protocol and the renderers, rather than an automatic merge — and the fork's value as an evaluation reference depends on knowing exactly which snapshot is under review. The upstream README below is the fork-point version, preserved as received.

## Related repositories

- [ai-services](https://github.com/Oviainc/ai-services) — our flagship site: AI agents, machine learning in production, and AI products for business
- [antigravity-cli-ovia-inc](https://github.com/Oviainc/antigravity-cli-ovia-inc) — the terminal surface of a vendor agent harness, from the same tooling group
- [Graft-ovia-inc](https://github.com/Oviainc/Graft-ovia-inc) — a local context graph for coding agents, from the same tooling group
- [agents-cli-ovia-inc-studio-labs](https://github.com/Oviainc/agents-cli-ovia-inc-studio-labs) — the CLI and skills for building, evaluating, and deploying agents, evaluated alongside this fork
- [mcp-toolbox-ovia-inc](https://github.com/Oviainc/mcp-toolbox-ovia-inc) — a toolbox that connects agents to databases, from the same tooling group

## Working with us

Start from our site: https://oviainc.github.io/ai-services/ — the contact form there is the fastest way to reach us.

## Licence

Apache-2.0. See LICENSE in this repository. The licence stands as received, and upstream credit remains with the upstream contributors.

---

Ovia Inc — Artificial Intelligence & Context Engineering. Based in Canada, working wherever the work is.

---

# A2UI: Agent-to-User Interface

A2UI is an open-source project, complete with a format
optimized for representing updatable agent-generated
UIs and an initial set of renderers, that allows agents
to generate or populate rich user interfaces.

<img src="docs/public/assets/a2ui_gallery_examples.png" alt="Gallery of A2UI components" height="400">

_A gallery of A2UI rendered cards, showing a variety of UI compositions that A2UI can achieve._

## ⚠️ Status: Early stage public preview

> **Note:** A2UI's current production release is **v0.9.1**, a patch release in
> the stable v0.9 protocol family. The v1.0 specification is a release
> candidate, while v0.8 is legacy. The specification and implementations are
> functional but are still evolving. We are opening the project to foster
> collaboration, gather feedback, and solicit contributions (e.g., on client
> renderers). Expect changes.

## Summary

Generative AI excels at creating text and code, but agents can struggle to
present rich, interactive interfaces to users, especially when those agents
are remote or running across trust boundaries.

**A2UI** is an open standard and set of libraries that allows agents to
"speak UI." Agents send a declarative JSON format describing the _intent_ of
the UI. The client application then renders this using its own native
component library (Flutter, Angular, Lit, etc.).

This approach ensures that agent-generated UIs are
**safe like data, but expressive like code**.

## High-level philosophy

A2UI was designed to address the specific challenges of interoperable,
cross-platform, generative or template-based UI responses from agents.

The project's core philosophies:

- **Security first**: Running arbitrary code generated by an LLM may present a
  security risk. A2UI is a declarative data format, not executable
  code. Your client application maintains a "catalog" of trusted, pre-approved
  UI components (e.g., Card, Button, TextField), and the agent can only request
  to render components from that catalog.
- **LLM-friendly and incrementally updatable**: The UI is represented as a flat
  list of components with ID references which is easy for LLMs to generate
  incrementally, allowing for progressive rendering and a responsive user
  experience. An agent can efficiently make incremental changes to the UI based
  on new user requests as the conversation progresses.
- **Framework-agnostic and portable**: A2UI separates the UI structure from
  the UI implementation. The agent sends a description of the component tree
  and its associated data model. Your client application is responsible for
  mapping these abstract descriptions to its native widgets—be it web components,
  Flutter widgets, React components, SwiftUI views or something else entirely.
  The same A2UI JSON payload from an agent can be rendered on multiple different
  clients built on top of different frameworks.
- **Flexibility**: A2UI also features an open registry pattern that allows
  developers to map server-side types to custom client implementations, from
  native mobile widgets to React components. By registering a "Smart Wrapper,"
  you can connect any existing UI component—including secure iframe containers
  for legacy content—to A2UI's data binding and event system. Crucially, this
  places security firmly in the developer's hands, enabling them to enforce
  strict sandboxing policies and "trust ladders" directly within their custom
  component logic rather than relying solely on the core system.

## Use cases

Some of the use cases include:

- **Dynamic Data Collection:** An agent generates a bespoke form (date pickers,
  sliders, inputs) based on the specific context of a conversation (e.g.,
  booking a specialized reservation).
- **Remote Sub-Agents:** An orchestrator agent delegates a task to a
  remote specialized agent (e.g., a travel booking agent) which returns a
  UI payload to be rendered inside the main chat window.
- **Adaptive Workflows:** Enterprise agents that generate approval
  dashboards or data visualizations on the fly based on the user's query.

## Architecture

The A2UI flow disconnects the generation of UI from the execution of UI:

1. **Generation:** An Agent (using Gemini or another LLM) generates or uses
   a pre-generated `A2UI Response`, a JSON payload describing the composition
   of UI components and their properties.
2. **Transport:** This message is sent to the client application
   (via A2A, AG-UI, etc.).
3. **Resolution:** The Client's **A2UI Renderer** parses the JSON.
4. **Rendering:** The Renderer maps the abstract components
   (e.g., `type: 'text-field'`) to the concrete implementation in the client's codebase.

## Dependencies

A2UI is designed to be a lightweight format, but it fits into a larger ecosystem:

- **Transports:** Compatible with **A2A Protocol** and **AG-UI**.
- **LLMs:** Can be generated by any model capable of generating JSON output.
- **Host Frameworks:** Requires a host application built in a supported framework
  (currently: Web or Flutter).

## Getting started

Pick the path that matches where you want to start:

| Path                                                                                                      | What you get                                                                                                                            | Time   |
| --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| 🍜 **[Quickstart Restaurant Finder Demo](https://a2ui.org/quickstart/)**                                  | Full-stack A2UI running locally with a Gemini powered ADK agent and Lit renderer. Learn A2UI end-to-end and customize to your use case. | ~5 min |
| ⚛️ **[Use A2UI with Any Agent Framework & Harness](docs/public/guides/a2ui-with-any-agent-framework.md)** | Scaffold an AG-UI app or harness for your framework of choice, then enable A2UI rendering over AG-UI.                                   | ~5 min |
| 🎨 **[A2UI Composer](https://a2ui-project.github.io/composer/)**                                          | Generate A2UI JSON from a visual editor and paste it into any agent prompt — no install required.                                       | ~1 min |
| 🎬 **[A2UI Theater](https://a2ui-composer.ag-ui.com/theater)**                                            | Step through pre-built A2UI streaming scenarios across Lit, React, and Angular renderers — no install required.                         | ~1 min |

### Restaurant Finder demo — Summary

Prerequisites: Node.js 18+ (with [Corepack](https://nodejs.org/api/corepack.html) enabled), [uv](https://docs.astral.sh/uv/), and a [Gemini API key](https://aistudio.google.com/apikey).

```bash
git clone https://github.com/a2ui-project/a2ui.git
cd a2ui
export GEMINI_API_KEY="your_gemini_api_key"

# Enable Corepack (macOS Homebrew users: see tip below)
corepack enable

yarn install
cd samples/client/lit
yarn demo:restaurant
```

> [!TIP]
> **macOS Homebrew Users:** If you previously installed standalone package managers, unlink conflicts before installing Corepack so Corepack can manage versions per-project:
>
> ```bash
> brew unlink yarn pnpm
> brew install corepack
> corepack enable
> ```

These commands install dependencies across workspaces, build the renderers, start the Python agent, and open the client at `http://localhost:5173`. For step-by-step instructions, alternative demos, and troubleshooting see the **[full Quickstart](docs/public/quickstart.md)**.

### Use A2UI with Any Agent Framework & Harness — Summary

```bash
npx create-ag-ui-app@latest
```

Use the AG-UI CLI with your framework or harness of choice (Google Chat, ADK, LangGraph, CrewAI, Mastra, Strands, Slack, Teams, etc.), then follow the **[AG-UI guide](docs/public/guides/a2ui-with-any-agent-framework.md)** to enable A2UI rendering. Some scaffold paths use [CopilotKit's A2UI runtime](https://docs.copilotkit.ai/generative-ui/a2ui) with Next.js under the hood, but the setup surface is AG-UI-first.

### Other renderers

For Flutter, check out the [GenUI SDK](https://github.com/flutter/genui), which uses A2UI under the hood. See [docs/public/reference/renderers.md](docs/public/reference/renderers.md) for the full list of client implementations.

## Roadmap

We hope to work with the community on the following:

- **Spec stabilization:** Moving towards a v1.0 specification.
- **More renderers:** Adding official support for React, Jetpack Compose, iOS (SwiftUI), and more.
- **Additional transports:** Support for REST and more.
- **Additional Agent frameworks:** Genkit, LangGraph, and more.

## Contribute

A2UI is an **Apache 2.0** licensed project. We believe the future of UI is agentic,
and we want to work with you to help build it.

See [CONTRIBUTING.md](CONTRIBUTING.md) for details on how to get started.
