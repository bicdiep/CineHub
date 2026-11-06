# CineHub

A responsive web system for cinema management and online ticket booking, with integrated Generative AI features.

> Course project for **Introduction to Software Engineering** (CQ2024/4), University of Science, VNU-HCM.

## Overview

CineHub supports the cinema workflow from film submission and showtime scheduling to online ticket booking.

- **Customers:** browse showtimes, choose seats, make simulated payments and receive e-tickets with QR codes.
- **Film Distributors:** register an account, submit film information, request showtimes and view revenue.
- **Administrator (Cinema Management):** review requests, assign screening rooms, build the schedule with automatic conflict checking and view statistics.

## Key Features

- Role-based access for Customer, Distributor and Admin.
- Movie browsing, search and filters.
- Showtime scheduling with automatic conflict detection. The end time is the start time plus the movie duration plus the cleaning time (15 minutes by default).
- Temporary seat holding for 10 minutes, with automatic release of expired holds to prevent a seat from being sold twice.
- Simulated payment and QR code e-tickets.
- Ticket history and ticket cancellation.
- Revenue screen for distributors and statistics for Admin.
- AI features such as spoiler-free movie summaries and personalized movie recommendations. The booking flow is designed to remain usable when the AI service is unavailable.

## Tech Stack

| Component | Technology |
| :--- | :--- |
| Front-end | React 19 + TypeScript + Vite |
| Back-end | Node.js 24 LTS + Express 5 + TypeScript |
| Database | PostgreSQL 18 + node-postgres + SQL migrations |
| AI service | Google Gemini API (server-side via AIProvider adapter) |
| Task management | JIRA |
| Source control and collaboration | Git and GitHub |

## Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/bicdiep/CineHub.git
cd CineHub

# 2. Create the environment file from the template and fill in the values
cp .env.example .env

# 3. Install dependencies and run
<install-and-run-commands>
```

> Never commit the `.env` file or any other secrets to the repository. Keep the template in `.env.example` and store real credentials locally or in the approved secret-management service.

## Project Schedule and Milestones

The project has five sprints (05/10/2026 – 12/12/2026). The phases follow the course schedule: Inception = Sprint 1, Elaboration = Sprints 2–3, and Construction = Sprints 4–5.

| Phase | Sprint | Dates | Milestone (Git tag) |
|---|---|---|---|
| Inception | Sprint 1 | 05/10 – 18/10 | – |
| Elaboration | Sprint 2 | 19/10 – 01/11 | `prototype-s2` |
| Elaboration | Sprint 3 | 02/11 – 13/11 | `elaboration-baseline` |
| Construction | Sprint 4 | 14/11 – 27/11 | `build-1` |
| Construction | Sprint 5 | 28/11 – 12/12 | `build-2` |

The milestone tags identify project snapshots:

- `prototype-s2`: internal prototype at the end of Sprint 2.
- `elaboration-baseline`: the approved architecture and project baseline at the end of Sprint 3.
- `build-1`: the beta build and its end-to-end demonstration at the end of Sprint 4.
- `build-2`: the final build and release at the end of Sprint 5.

Tags are annotated Git tags prepared by the Configuration and Build Manager. Under the normal workflow, each tag points to one integrated, buildable commit on `main`. The detailed scope, tagging procedure and any approved exception are documented in [CONTRIBUTING.md](CONTRIBUTING.md). A tag is a snapshot of the whole project at a milestone, not a separate branch for one feature or one team member.

## Project Management and Documentation

- **JIRA** is used to manage the backlog, work items, bugs and change requests. Problems and change requests are submitted as JIRA issues, reviewed at the weekly team meeting, and given one of these dispositions: **Approved**, **Rejected** or **Deferred**.
- The team keeps project documents in the shared team folder. Documents are exported to this repository at each submission so that the submitted source code and its related documentation can be reviewed together.
- Documents follow the naming convention `CineHub-<Type>-v<major>.<minor>` (for example, `CineHub-SDP-v1.2`). Every document change must be recorded in its Revision History.
- The Git remote and shared team folder are the project backups. Every team member keeps a local Git clone. Tags and submitted documents must not be deleted before the end of the course.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before making changes. It explains:

- GitHub Flow, branch naming and branch lifecycle.
- Commit message conventions and Pull Request requirements.
- Code review, approval and merge responsibilities.
- JIRA issue handling and document management.
- Milestone tags, baselines, backup and retention rules.

The `main` branch is protected and must remain buildable. All changes enter `main` through a Pull Request linked to its JIRA key and reviewed by at least one other team member. The Configuration and Build Manager performs the merge after the required conditions are satisfied.

## Team

| Member | Student ID | Role |
|---|---|---|
| Nguyen Thanh Ha Uyen | 24120243 | Team lead, project manager; Developer |
| Duong Thi Bich Diep | 24120247 | Configuration and build manager; Developer |
| Ngo Thanh Khang | 24120066 | Requirements analyst, documentation manager; Developer |
| Doan Ham Triem | 24120473 | Test team leader; Developer |
| Nguyen Quoc Dat | 24120282 | Architecture and AI integration technical leader; Developer |

All members contribute as developers across the project.

## License

This project is for educational purposes. `<Add a license if needed>`
