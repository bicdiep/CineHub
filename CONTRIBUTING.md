# CineHub Source Code Management Guidelines

Branch – Commit – Pull Request – Code Review

> All branch names, commit messages and Pull Request titles are written in **English**.

## 1. Purpose and Scope

This document defines how the CineHub team uses Git and GitHub throughout the project (Sprint 1 to Sprint 5).

The goals of these guidelines are to:

- Keep the `main` branch always runnable, so it can be demoed at any time.
- Let all members work in parallel without overwriting each other's work.
- Make every change traceable: who changed what, why, and which sprint task it belongs to.
- Ensure every piece of code is read by at least one other person before it enters the product.

These guidelines apply to the entire repository, including front-end, back-end, documentation and configuration files.

## 2. Branching

### 2.1. Branching Model

The team uses **GitHub Flow**: there is a single main branch, `main`. All work is done on branches created from `main` and merged back into `main` through a Pull Request. This model is simple and suits a small team with 2-week sprints.

| Branch | Purpose | Created from | Merged into | Lifetime |
|---|---|---|---|---|
| `main` | Stable, always runnable code. Delivery milestones (Prototype, Build 1, 2, 3) are marked with tags on this branch | – | – | Whole project |
| `feature/...` | Develop a new feature | `main` | `main` | A few days, at most 1 sprint |
| `fix/...` | Fix a bug | `main` | `main` | Short |
| `docs/...` | Update documents, reports, diagrams | `main` | `main` | Short |
| `refactor/...` | Restructure code without changing behavior | `main` | `main` | Short |
| `test/...` | Add or update tests | `main` | `main` | Short |

### 2.2. Branch Naming Convention

Syntax: `<type>/<key-task>-<short-description>`

- `<type>`: one of `feature`, `fix`, `docs`, `refactor`, `test`.
- `<key-task>`: the JIRA task key, for example `SCRUM-12`.
- `<short-description>`: lowercase English words joined by hyphens, no accents, kept short.

Valid examples:

```
feature/SCRUM-12-google-login
feature/SCRUM-25-select-seats-for-showtime
fix/SCRUM-31-seat-hold-timeout
fix/SCRUM-40-qr-code-cannot-be-scanned
docs/SCRUM-05-use-case-diagram
```

Invalid examples: `test`, `my-branch`, `Feature/Login`, `fix-bug`.

### 2.3. Working with Branches

- **Nobody commits or pushes directly to `main`.** Branch protection is enabled on GitHub. Every change must go through a Pull Request.
- Each branch serves **one piece of work** (one task or one bug). Do not mix unrelated features in the same branch.
- A branch should not live longer than 1 sprint, to avoid drifting far from `main`. Before starting work each day, update your branch with `main` to reduce conflicts:

```bash
git checkout main
git pull origin main
git checkout feature/SCRUM-12-google-login
git merge main
```

- A merged branch must be **deleted right after** the Pull Request is closed.
- At the end of each sprint, after all features of the sprint are merged, the team lead tags `main` with the version matching the delivery milestone: `v0.1-prototype`, `v1.0-build1`, `v2.0-build2`, `v3.0-build3`.

## 3. Commits

### 3.1. General Principles

- Each commit is a small, complete and meaningful change. Do not put unrelated changes in one commit.
- The code at every commit must compile/run, with no syntax errors left behind.
- Do not commit generated files, libraries (`node_modules/`, `build/`, `dist/`), personal IDE configuration, and above all **secrets** (passwords, AI service API keys, database connection strings). List such files in `.gitignore`; keep secrets in a `.env` file and share only a `.env.example` template.
- Commit often (at least once after every working session) to avoid losing work and to make reviews easier to follow.

### 3.2. Commit Message Format

The team follows the **Conventional Commits** standard:

```
<type>(<scope>): <short description>

[optional detailed body]
```

Commit types:

| Type | Meaning |
|---|---|
| `feat` | Add a new feature |
| `fix` | Fix a bug |
| `docs` | Documentation changes |
| `style` | Code formatting (whitespace, semicolons, ...), no logic change |
| `refactor` | Restructure code without adding features or fixing bugs |
| `test` | Add or update tests |
| `chore` | Configuration, dependency updates, supporting work |

The `<scope>` is the affected module, for example: `auth`, `movie`, `booking`, `schedule`, `payment`, `ai`, `ui`.

Rules for the short description:

- Written in English, no more than 72 characters.
- Starts with a verb describing what was done (for example `add`, `update`, `delete`, `change`).
- Does not end with a period.

### 3.3. Examples

Valid commits:

```
feat(booking): add 10-minute seat hold
fix(payment): fix incorrect ticket code displayed in QR code
docs(report): update system context diagram
test(auth): add tests for incorrect password login
```

Invalid commits:

```
Update
fix bug
Abc
fix random bugs, add home page, change button color
```

## 4. Pull Requests

### 4.1. When to Create a Pull Request

Create a Pull Request (PR) when the work on the branch is finished and self-checked. To get early feedback, you may open the PR as a **Draft** and state clearly which parts are not done yet.

### 4.2. Pull Request Requirements

- **Title** follows the same format as commits, followed by the task key. Example: `feat(booking): add seat selection by showtime [SCRUM-25]`.
- **Small size:** each PR should have fewer than roughly 400 changed lines. Larger changes must be split into several smaller PRs.
- **Complete description** using the template in section 4.3.
- **Self-check before submitting:** the app runs, tests pass, the branch is up to date with `main`, and there are no conflicts.
- **Reviewer:** assign at least 1 other team member.
- **Task link:** write `Closes #<issue number>` so the issue closes automatically when the PR is merged.

### 4.3. Pull Request Template

The following template is stored at `.github/pull_request_template.md` so GitHub fills it in automatically when a PR is created:

```markdown
## Changes
- Briefly describe what was done.

## Reason
- Which requirement or bug does this change address? (Closes #...)

## How to Test
1. Steps for the reviewer to try it out.
2. Expected result.

## Screenshots (if the UI changed)

## Self-check
- [ ] The app runs on my machine
- [ ] I ran the tests and all of them pass
- [ ] I updated my branch with main and there are no conflicts
- [ ] There are no secrets in the source code
- [ ] I updated the documentation (if needed)
```

### 4.4. Merge Conditions

A PR can be merged only when **all** of the following are met:

- At least 1 approval from someone other than the author.
- All comments requesting changes have been addressed and marked **Resolved**.
- Automated checks (if GitHub Actions is configured) pass.
- No conflicts with the target branch.

Merge method: use **Squash and merge** so the history of `main` stays clean, with one commit per feature. The merger is the PR author, after approval.

## 5. Code Review

### 5.1. Goals

Code review aims to catch bugs early, keep the codebase consistent, and help members understand each other's work. A review comments on the code, not on the person who wrote it.

### 5.2. Deadlines

- The assigned reviewer must respond to the PR within **24 hours** (excluding holidays and exam periods announced in advance).
- The author must respond to review comments within **24 hours**.
- If a deadline is missed, the author reminds the reviewer in the team chat or asks the team lead to assign another reviewer.

### 5.3. Review Checklist

| Criterion | Question to answer |
|---|---|
| Meets requirements | Does the change meet the description of the task / user story? |
| Correctness | Are there logic errors or unhandled edge cases (empty data, two people holding the same seat, expired payment, ...)? |
| Readability | Are variable and function names clear? Are functions too long or duplicated? |
| Consistency | Does the code follow the project's naming and formatting conventions? |
| Security | Are API keys or passwords exposed? Is user input validated? Are AI service calls made on the server side? |
| Testing | Does the new feature come with tests? Do the existing tests still pass? |
| Scope | Does the PR contain changes unrelated to the task? |

### 5.4. Writing Comments

Comments must be specific, give a reason, and suggest a fix when possible. The team uses prefixes to distinguish severity:

| Prefix | Meaning | Must fix before merge? |
|---|---|---|
| `[Required]` | A bug, security issue or violation of the guidelines | Yes |
| `[Suggestion]` | A better way to write it, but the current code is not wrong | No, the author decides |
| `[Question]` | The reviewer does not understand and needs an explanation | No, but the author must answer |

Do write: `[Required] The holdSeat() function does not check whether the seat is already held by someone else, so two users could book the same seat. Check the seat status inside the same transaction.`

Do not write: `This is wrong. This code is terrible.`

### 5.5. Review Outcome

The reviewer chooses one of three outcomes on GitHub:

- **Approve:** agree to merge.
- **Request changes:** at least one `[Required]` comment exists; the author fixes it and requests a new review.
- **Comment:** only `[Suggestion]` or `[Question]` comments.

## 6. Overall Workflow

From receiving a task to including the feature in a delivery build:

1. Pick up a task on the sprint board and move it to **In Progress**.
2. Create a new branch from `main` following the naming rules in section 2.2.
3. Write code and commit often following section 3.
4. Update the branch with `main`, self-check, then push to GitHub.
5. Create a Pull Request into `main` using the template in section 4.3, assign a reviewer, and move the task to **In Review**.
6. The reviewer checks the PR following section 5; the author fixes issues until it is Approved.
7. The author merges the PR with **Squash and merge**, deletes the branch, and moves the task to **Done**.
8. At the end of the sprint, the team lead tags the version on `main` and submits the delivery build.

## 7. Responsibilities

| Role | Responsibilities |
|---|---|
| Team lead | Configure branch protection on `main`, tag versions at the end of each sprint, remind members when guidelines are violated |
| Developers | Follow the branch, commit and PR guidelines; review assigned PRs on time |
| Whole team | Review and adjust these guidelines during the Sprint Retrospective if they are not working well |