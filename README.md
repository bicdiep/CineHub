# CineHub

A responsive web system for cinema management and online ticket booking, with integrated Generative AI features.

> Course project for **Introduction to Software Engineering** (CQ2024/4), University of Science, VNU-HCM.

## Overview

CineHub supports the full flow from film submission and showtime scheduling to online ticket booking, with no waiting in line:

- **Film producers / distributors:** register an account, submit film information, request showtimes and view revenue.
- **Cinema manager (Admin):** review requests, assign screening rooms, build the schedule with automatic conflict checking, and view statistics.
- **Customers:** browse showtimes, choose seats, pay (simulated) and receive an e-ticket with a QR code.

## Key Features

- Role-based access for three roles: Customer, Distributor, Admin.
- Movie browsing, search and filters.
- Showtime scheduling with **automatic conflict detection** (end time = start time + movie duration + 15 minutes for room cleaning).
- **10-minute temporary seat hold** with automatic release of expired holds, preventing a seat from being sold twice.
- Simulated payment and **QR code e-tickets**.
- Ticket history and ticket cancellation.
- Revenue screen for distributors and statistics for Admin.
- **AI features:** spoiler-free movie summaries and personalized movie recommendations. The booking flow keeps working when the AI service is unavailable.

## Tech Stack

| Component | Technology |
|---|---|
| Front-end | `<to be decided after Sprint 2>` |
| Back-end | `<to be decided after Sprint 2>` |
| Database | `<to be decided after Sprint 2>` |
| AI service | `<Generative AI API, called from the server side>` |
| Task management | JIRA |

## Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/<account-name>/CineHub.git
cd CineHub

# 2. Create the environment file from the template and fill in the values
cp .env.example .env

# 3. Install dependencies and run
<install-and-run-commands>
```

> Never commit the `.env` file or any other secrets to the repository.

## Roadmap

The project has 5 sprints of 2 weeks each (05/10/2026 – 13/12/2026).

| Phase | Sprint | Dates | Release tag |
|---|---|---|---|
| Inception | Sprint 1 | 05/10 – 18/10 | – |
| Elaboration | Sprint 2 | 19/10 – 01/11 | `v0.1-prototype` |
| Construction | Sprint 3 | 02/11 – 15/11 | `v1.0-build1` |
| | Sprint 4 | 16/11 – 29/11 | `v2.0-build2` |
| | Sprint 5 | 30/11 – 13/12 | `v3.0-build3` |

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for the rules on branch naming, commit messages, Pull Requests and code review. All branch names and commit messages are written in **English**.

## Team

| Member | Student ID | Role |
|---|---|---|
| Nguyen Thanh Ha Uyen | 24120243 | Team lead, project manager; Developer |
| Duong Thi Bich Diep | 24120247 | Configuration and build manager; Developer |
| Ngo Thanh Khang | 24120066 | Requirements analyst, documentation lead; Developer |
| Doan Ham Triem | 24120473 | Test lead; Developer|
| Nguyen Quoc Dat | 24120282 | architecture and AI integration tech lead; Developer|

All members are developers working on both front-end and back-end.

## License

This project is for educational purposes. `<Add a license if needed>`