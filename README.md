# Prompt Engineering Workbench

> A research-driven platform integrating **17 advanced prompt engineering frameworks** across 5 categories — from geometric control systems and meta-cognitive architectures to quality assurance, cross-domain translation, and creative research tools.

[![Next.js](https://img.shields.io/badge/Next.js-15-black?logo=next.js)](https://nextjs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.2-blue?logo=typescript)](https://www.typescriptlang.org)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3.3-38bdf8?logo=tailwind-css)](https://tailwindcss.com)
[![Prisma](https://img.shields.io/badge/Prisma-6.7-2D3748?logo=prisma)](https://www.prisma.io)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

---

## What this is

The Prompt Engineering Workbench began as a structured research effort to analyze, normalize, and integrate advanced AI prompting methodologies. It has since grown into a full-stack web application that makes those frameworks interactive and accessible — with dedicated tools for each methodology, a framework combiner, a prompt library, a CxEP pipeline simulator, and practice exercises.

The repository houses two distinct but connected layers:

- **Research layer** — foundational analysis documents that defined the framework baseline
- **Application layer** — a Next.js web app implementing those frameworks as interactive tools

---

## Framework overview

The workbench integrates 17 frameworks organized into 5 categories.

### Core Foundation (5 frameworks)

The original five frameworks that define the workbench's architectural baseline.

| Framework | Description |
|-----------|-------------|
| **PALS** (Promptcraft-Aware Linguistic Stack) | 6-layer multi-dimensional prompt control: Structural, Meaning, Cognitive, Functional, Trust & Epistemics, and Cross-Modal Semiotics |
| **Context Engineering 2.0** (CxEP) | Context-to-Execution Pipeline — systematic, auditable AI-assisted workflow design |
| **AI Prompts Systemic Analysis** | Failure cascade simulation, cognitive friction auditing, and governance trilemma analysis |
| **Role Prompting Research** | Persona engineering, two-stage immersion, synergistic prompting, and multi-agent orchestration |
| **LensGPT Deep Prompting** | Epistemic scaffolding, reflexive analysis, bias detection, and recursive refinement |

### Geometric Control Systems (4 frameworks)

Polygon-based generative syntax for cross-modal prompt design.

| Framework | Description |
|-----------|-------------|
| **Polygonal Grammar** | Geometric primitives as generative syntax across 11 design domains |
| **Multimodal Design** | Geometric-to-sensory translation (auditory, tactile, olfactory outputs) |
| **Narrative Geometry** | Polygon-based temporal narrative scaffolding |
| **Polygonal Semiotics** | Geometric forms as symbolic grammar and latent control elements |

### Meta-Cognitive Enhancement (3 frameworks)

Self-improving and temporally-aware prompt architectures.

| Framework | Description |
|-----------|-------------|
| **Meta-Recursive Engine** | Self-improving prompt architectures with recursive quality enhancement |
| **Temporal Palimpsest** | Tracking AI metaphor and cognition evolution across training epochs (TEI, SDC, MFS metrics) |
| **Advanced Architecture** | Forensic analysis and reconstruction of ToT, Zero-Shot, and Prompt Chaining architectures |

### Quality Assurance Systems (3 frameworks)

Real-time monitoring and integrity management for prompt systems.

| Framework | Description |
|-----------|-------------|
| **Symbolic Drift Detection** | Six-type symbolic drift detection, monitoring, and correction |
| **Contradiction Emergence** | Recursive contradiction detection and mitigation in AI systems |
| **Failure Optimization** | Diagnostic and reconstructive framework for prompting architecture failures |

### Cross-Domain & Creative Research (5 frameworks)

Domain bridging, evaluation, and creative tooling.

| Framework | Description |
|-----------|-------------|
| **Synthesized Composition** | Cross-domain compositional grammar and modular phrase mapping |
| **Advanced Techniques** | Multi-technique integration (CoT, ToT, few-shot, self-consistency, decomposition) |
| **Prompt Poetry** | Poetic language structures for emotionally rich visual storytelling |
| **Deep Research Analytics** | MTAS, SSI, and LCM-based evaluation with semiotic interface design |
| **Cross-Domain Translation** | Universal translation matrix across design, code, narrative, and visual domains |

---

## Application routes

All routes are implemented in `app/app/`. The application uses the Next.js App Router.

```
/                           — Dashboard: overview of all 17 frameworks and quick navigation
/pals-builder               — PALS 6-layer prompt builder
/cxep-simulator             — Context-to-Execution Pipeline simulator
/systemic                   — Systemic analysis tools (failure cascade, cognitive friction)
/role                       — Role prompting persona builder and immersion workflows
/lens                       — LensGPT reflexive inquiry interface
/geometric/polygonal-grammar — Polygonal grammar composer
/geometric/*                — Multimodal design, narrative geometry, semiotics tools
/meta/recursive-engine      — Meta-recursive prompt enhancement engine
/meta/*                     — Temporal palimpsest analyzer, architecture tools
/quality/symbolic-drift     — Symbolic drift detection monitor
/quality/*                  — Contradiction emergence, failure optimization
/cross/synthesized-composition — Cross-domain composition and translation
/creative/prompt-poetry     — Prompt poetry and visual storytelling studio
/creative/*                 — Deep research analytics, advanced evaluation
/integration/combiner       — Framework combiner: synergistic multi-framework mixing
/compare                    — Side-by-side 5-way framework comparison
/library                    — Prompt library: annotated collection across all frameworks
/exercises                  — Practice exercises for all 17 frameworks
```

---

## Tech stack

| Layer | Technology |
|-------|------------|
| Framework | [Next.js 15](https://nextjs.org) (App Router) |
| Language | TypeScript 5.2 |
| Styling | Tailwind CSS 3.3 + `tailwindcss-animate` |
| UI Components | [Radix UI](https://radix-ui.com) (full suite) + [shadcn/ui](https://ui.shadcn.com) |
| State management | [Zustand](https://zustand-demo.pmnd.rs) + [Jotai](https://jotai.org) |
| Server state | [TanStack Query](https://tanstack.com/query) + [SWR](https://swr.vercel.app) |
| Database ORM | [Prisma](https://prisma.io) 6.7 |
| Auth | [NextAuth.js](https://next-auth.js.org) 4 with Prisma adapter |
| Forms | React Hook Form + Zod + Yup |
| Charts | Recharts + Chart.js + Plotly.js |
| Animation | Framer Motion 10 |
| Icons | [Lucide React](https://lucide.dev) |
| Package manager | Yarn (Berry) |

---

## Repository structure

```
prompt_engineering_workbench/
├── .github/                          # GitHub configuration
├── Uploads/                          # Source research documents and raw inputs
├── app/                              # Next.js application
│   ├── app/                          # App Router pages and routes
│   │   ├── api/                      # API route handlers
│   │   ├── pals-builder/             # PALS framework tool
│   │   ├── cxep-simulator/           # CxEP pipeline simulator
│   │   ├── systemic/                 # Systemic analysis tools
│   │   ├── role/                     # Role prompting tools
│   │   ├── lens/                     # LensGPT tools
│   │   ├── geometric/                # Geometric control systems
│   │   ├── meta/                     # Meta-cognitive tools
│   │   ├── quality/                  # Quality assurance monitors
│   │   ├── cross/                    # Cross-domain translation
│   │   ├── creative/                 # Creative and research tools
│   │   ├── integration/              # Framework combiner
│   │   ├── compare/                  # Framework comparison
│   │   ├── library/                  # Prompt library
│   │   ├── exercises/                # Practice exercises
│   │   ├── layout.tsx                # Root layout
│   │   └── page.tsx                  # Dashboard
│   ├── components/                   # Shared UI components
│   ├── hooks/                        # Custom React hooks
│   ├── lib/                          # Utilities and shared logic
│   ├── prisma/                       # Prisma schema and migrations
│   ├── scripts/                      # Seed and utility scripts
│   ├── package.json
│   ├── tailwind.config.ts
│   └── tsconfig.json
├── prompt_engineering_analysis.md    # Research: PALS + Context Engineering 2.0
├── additional_frameworks_analysis.md # Research: Systemic Analysis + Role + LensGPT
├── advanced_frameworks_synthesis.md  # Research: 12-framework synthesis and roadmap
└── CONTRIBUTING.md                   # Contribution rules and Workbench Analysis Agent spec
```

---

## Getting started

### Prerequisites

- Node.js 18+
- Yarn (Berry) — configured via `app/.yarnrc.yml`
- A supported database (Prisma default: SQLite for development, configurable for Postgres)

### Installation

```bash
# Clone the repository
git clone https://github.com/projectedanx/prompt_engineering_workbench.git
cd prompt_engineering_workbench/app

# Install dependencies
yarn install

# Configure environment
cp .env .env.local
# Edit .env.local with your database URL and auth secrets

# Set up the database
npx prisma generate
npx prisma db push

# (Optional) Seed the database
npx prisma db seed

# Start the development server
yarn dev
```

The app will be available at `http://localhost:3000`.

### Environment variables

See `app/.env` for the required variable names. At minimum you will need:

- `DATABASE_URL` — your Prisma database connection string
- `NEXTAUTH_URL` — the base URL of the application
- `NEXTAUTH_SECRET` — a random secret for NextAuth session signing

---

## Research documents

The three analysis documents in the root are the conceptual foundation of the workbench. They were produced in July 2025 and define the framework catalog that the application implements.

| Document | Content |
|----------|---------|
| [`prompt_engineering_analysis.md`](./prompt_engineering_analysis.md) | Deep analysis of PALS and Context Engineering 2.0, including layer-to-effect atlas, YAML schema, and implementation roadmap |
| [`additional_frameworks_analysis.md`](./additional_frameworks_analysis.md) | Analysis of Systemic Analysis Framework, Role Prompting, and LensGPT; integration architecture and service design |
| [`advanced_frameworks_synthesis.md`](./advanced_frameworks_synthesis.md) | Synthesis of 12 additional frameworks; normalized catalog, integration mapping, and 18-month implementation roadmap |

---

## Contributing

All contributions follow the specification in [`CONTRIBUTING.md`](./CONTRIBUTING.md).

The core idea: every new research document or framework proposal should be analyzed using the **Workbench Analysis Agent** prompt (defined in `CONTRIBUTING.md`) before opening a PR. This ensures every addition is coherently mapped against the existing framework baseline and produces a traceable, modular integration proposal.

**Quick contribution flow:**

1. Place source documents in `Uploads/`
2. Run the Workbench Analysis Agent against them
3. Save the analysis output to `analyses/<slug>_analysis.md`
4. Open a PR with both artifacts and check all boxes in the PR checklist

For batches of 5+ documents, use the clustering workflow described in `CONTRIBUTING.md`.

---

## Design principles

- **Modular** — each framework is independently composable and testable
- **Traceable** — every architectural decision maps back to a research artifact
- **Systematic** — contributions use consistent extraction and integration logic
- **Honest about uncertainty** — speculative ideas are labeled explicitly
- **Research-first, implementation-aware** — theory is always translated into buildable structure

---

## Project status

- [x] Research corpus established (17 frameworks across 3 analysis documents)
- [x] Framework baseline defined and formalized
- [x] Next.js application scaffolded with full route structure
- [x] Contribution rules formalized (`CONTRIBUTING.md`)
- [ ] `analyses/` and `frameworks/` directories populated
- [ ] App documentation for individual route implementations
- [ ] Deployment configuration
- [ ] CI/CD pipeline

---

*Research origin: July 2025 — Abacus AI*  
*Application: Next.js 15 / TypeScript / Prisma*  
*Maintained by [@projectedanx](https://github.com/projectedanx)*
