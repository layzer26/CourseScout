# CourseScout

> Scout every South African golf course before you play.

![Status](https://img.shields.io/badge/status-in%20development-orange)
![Phase](https://img.shields.io/badge/phase-0%20foundations-blue)
![License](https://img.shields.io/badge/license-all%20rights%20reserved-lightgrey)

CourseScout is a web platform that lets golfers explore South African golf
courses hole by hole in 3D, see yardages, carries and elevation, and follow a
suggested route for each hole before they tee off.

The same course data is exposed through a versioned **Route API**, so other golf
apps can show planned routes for SA courses without mapping them themselves.

---

## Table of contents

1. [Features](#features)
2. [Roadmap](#roadmap)
3. [Tech stack](#tech-stack)
4. [Architecture](#architecture)
5. [Getting started](#getting-started)
6. [Project structure](#project-structure)
7. [Development workflow](#development-workflow)
8. [Environments](#environments)
9. [Documentation](#documentation)
10. [License](#license)

---

## Features

### Planned for v1.0 (MVP)

- [ ] Course directory with search and a map of South African courses
- [ ] 3D hole viewer with terrain and satellite imagery
- [ ] Tee box selector with per-tee distances
- [ ] Yardages: distance to green, carries to hazards, elevation change
- [ ] Suggested route per hole (curated)
- [ ] Metres / yards toggle
- [ ] Link out to each course's own booking page

### Later

- Public Route API v1 with API keys and OpenAPI documentation
- Rule-based route suggestions personalised to a golfer's club distances
- Partner integrations with golf apps and golf clubs (by agreement)
- Intelligent suggestions behind the same API contract

---

## Roadmap

| Phase | Goal | Status |
|---|---|---|
| 0 | Foundations: repo, CI, environments, architecture decisions | In progress |
| 1 | MVP website: mapped Gauteng courses, 3D hole view, curated routes | Planned |
| 2 | Public Route API v1 | Planned |
| 3 | Rule-based engine and first partners | Planned |
| 4 | Intelligent suggestions | Later |
| 5 | National coverage | Later |

Each completed phase is published as a [GitHub Release](../../releases).

---

## Tech stack

| Layer | Technology |
|---|---|
| Framework | Next.js (App Router), TypeScript |
| Styling | Tailwind CSS |
| Maps / 3D | To be decided (see ADR on map provider) |
| Geometry | Turf.js |
| Database | PostgreSQL + PostGIS (Supabase) |
| Hosting | Vercel |
| CI | GitHub Actions |
| Testing | Vitest, Playwright |

Every significant technology choice is recorded as an
[Architecture Decision Record](docs/adr).

---

## Architecture

The platform is **API-first**: the website is the first consumer of the same
Route API that partner apps will use. Route suggestions come from a
`SuggestionProvider` interface, so the engine behind it (curated, rule-based or
intelligent) can change without breaking any consumer.

```
 Website      Partner apps
     \            /
      Route API v1  (versioned, keyed, rate limited)
           |
   SuggestionProvider
     /     |      \
 Curated  Rules  Intelligent
```

Full detail: [docs/architecture.md](docs/architecture.md) (coming in Phase 0).

---

## Getting started

> Setup instructions are written as each part of the stack is added.

### Prerequisites

| Tool | Version |
|---|---|
| Node.js | LTS (exact version recorded here once installed) |
| Git | Latest |
| Docker Desktop | Latest (for the local database) |

### Run locally

```bash
git clone https://github.com/layzer26/CourseScout.git
cd CourseScout
# Further steps added in CSA-3 (app scaffold) and CSA-8 (local database)
```

### Environment variables

Copy `.env.example` to `.env.local` and fill in the values.
Never commit `.env.local`. (File added when the first variable is needed.)

---

## Project structure

```
/app            Pages and API route handlers
/components     UI components
/lib            Business logic (yardage maths, suggestion providers)
/supabase       Database migrations and seed data
/docs
  /adr          Architecture Decision Records
.github         CI workflows, PR and issue templates
```

---

## Development workflow

Work is tracked in Jira (project key `CSA`) and planned in 1-week sprints.

| Item | Convention | Example |
|---|---|---|
| Branch | `feature/CSA-<n>-short-name`, `fix/...`, `chore/...` | `feature/CSA-14-yardage-engine` |
| Commit (on a branch) | [Conventional Commits](https://www.conventionalcommits.org) + ticket key | `feat(yardage): add carry to hazard [CSA-14]` |
| Pull request title | Conventional Commit format + ticket key (becomes the squash commit on `main`) | `feat(yardage): carries and elevation engine [CSA-14]` |
| Merge | Squash merge into `main` after CI passes | |
| Release | Semantic versioning tag + release notes | `v0.1.0` |

`main` is always deployable and protected: changes arrive only through pull
requests with passing checks.

### Definition of Done

- Acceptance criteria on the ticket are met
- Lint, type check, tests and build pass in CI
- Works on a 375 px wide mobile screen
- Docs updated (README, ADR or API spec where relevant)

---

## Environments

| Environment | URL | Deploys from |
|---|---|---|
| Local | `http://localhost:3000` | Your machine |
| Preview | Unique URL per pull request | Every PR |
| Production | To be announced | `main` |

---

## Documentation

| Document | Purpose |
|---|---|
| [docs/adr](docs/adr) | Why the system is built the way it is |
| `openapi.yaml` | Route API contract (Phase 1) |
| [CHANGELOG.md](CHANGELOG.md) | What changed in each release (from v0.1.0) |

---

## Data and attribution

Course geometry is mapped by the CourseScout team. Where OpenStreetMap data
is used it is credited as "© OpenStreetMap contributors" under the ODbL.
Golf club names are used descriptively; no affiliation is implied.

---

## License

Copyright (c) 2026 Liam Naidoo. All rights reserved.
See [LICENSE](LICENSE). This repository is public for portfolio purposes; the
code and data may not be copied, modified or distributed without written
permission.

## Contact

Liam Naidoo: [GitHub](https://github.com/layzer26) ·
[LinkedIn](https://www.linkedin.com/in/<your-profile>)
