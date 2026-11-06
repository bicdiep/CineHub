# CineHub Source Code Management Guidelines

Branch – Commit – Pull Request – Code Review – Integration – Tags

> All branch names, commit messages and Pull Request titles must be written in **English**.
> These guidelines follow the CineHub Software Development Plan (SDP), especially Section 4.3.4 (Configuration Management). If a detail in this document conflicts with the approved SDP, the SDP takes precedence and this document must be updated.

## 1. Purpose and Scope

This document defines how the CineHub team manages source code, tests, project documents, branches, Pull Requests, integrations and delivery milestones throughout Sprint 1 to Sprint 5 (05/10/2026 – 12/12/2026).

The objectives are to:

- Keep `main` integrated and buildable.
- Allow team members to work in parallel without overwriting each other's changes.
- Link every work item and change to JIRA for traceability.
- Require review by at least one other team member before a change enters `main`.
- Keep source code, test scripts, data files and related design documents together in project baselines.
- Preserve submitted documents and milestone tags until the end of the course.

These rules apply to the entire repository: front-end, back-end, tests, data files, documentation and project configuration.

## 2. Branching and Integration

### 2.1. Branching Model

The team follows **GitHub Flow** with one permanent integration branch, `main`.

1. `main` is the shared integration branch. It must remain buildable.
2. Each work item is developed on its own short-lived branch created from the latest `main`.
3. Completed work is submitted to `main` through a Pull Request (PR).
4. The PR is reviewed by at least one other team member.
5. After the merge conditions are met, the **Configuration and Build Manager merges the PR**.

The team does not create long-lived sprint branches (such as `sprint-1` or `sprint-2`) just to represent a sprint or hold a milestone. Sprint and delivery snapshots are represented by Git tags.

| Branch | Used for | Created from | Target | Lifetime |
|---|---|---|---|---|
| `main` | Stable, integrated project source | – | – | Whole project |
| `feature/SCRUM-...` | Application code changes, including new features, bug fixes and refactoring | Latest `main` | `main` | Short-lived; normally within one sprint |
| `docs/SCRUM-...` | Documents, reports, diagrams and documentation-only changes | Latest `main` | `main` | Short-lived |
| `tests/SCRUM-...` | Test scripts, test cases and test-related changes | Latest `main` | `main` | Short-lived |

A branch should contain one JIRA work item. Do not combine unrelated work items in one branch. If a task genuinely requires coordinated code, documentation and test changes, the team may keep the related files together in one branch and PR, using the branch prefix that best describes the main purpose of that JIRA task.

### 2.2. Branch Naming Convention

Use this format:

```text
<type>/SCRUM-<number>-<short-name>
```

The allowed prefixes are:

- `feature/` for application code work: new functionality, bug fixes and code refactoring.
- `docs/` for documentation-only work.
- `tests/` for test-only work.

`SCRUM-<number>` must be the actual JIRA issue key. `<short-name>` must be a short, descriptive phrase in lowercase English, with words separated by hyphens. Do not use spaces, accents or personal names.

Examples (the issue numbers are illustrative; always use the actual JIRA key):

```text
feature/SCRUM-30-seat-hold
feature/SCRUM-42-select-seats-for-showtime
feature/SCRUM-51-fix-seat-hold-timeout
docs/SCRUM-45-use-case-diagram
docs/SCRUM-60-update-getting-started-guide
tests/SCRUM-62-seat-hold-concurrency-tests
```

Invalid examples:

```text
test
my-branch
Feature/Login
fix/SCRUM-51-seat-hold-timeout
feature/google-login
docs/update-readme
```

The examples above are invalid because they omit the JIRA key, use an unsupported prefix, use inconsistent capitalization, or do not follow the required naming pattern.

### 2.3. Creating and Updating a Task Branch

The developer assigned to a work item creates and manages that task branch. The Configuration and Build Manager coordinates the repository workflow; this role does not mean creating a separate branch for every developer.

Create a branch from the latest `main`:

```bash
git checkout main
git pull origin main
git checkout -b feature/SCRUM-30-seat-hold
```

Replace the example branch name with the correct prefix and actual JIRA key for the work item.

Before opening or updating a PR, bring the latest `main` into the task branch:

```bash
git checkout main
git pull origin main
git checkout feature/SCRUM-30-seat-hold
git merge main
```

Resolve any conflicts, run the application and relevant tests, then push the branch:

```bash
git push -u origin feature/SCRUM-30-seat-hold
```

Rules:

- Do not commit or push directly to `main`.
- Keep each branch focused on one work item.
- Keep branches short-lived and normally finish them within the sprint in which the work is planned.
- Delete a task branch after its PR has been merged. Do not delete `main` or milestone tags.
- Do not create a sprint branch only to create or preserve a milestone tag.

### 2.4. Keeping `main` Buildable

`main` is buildable when a team member can clone it, follow the README's Getting Started instructions, install the required dependencies, build and start the application, and run the available automated tests successfully.

The following rules apply:

- Changes enter `main` only through an approved PR.
- The `protect-main` repository ruleset is recorded as configured to require Pull Requests and at least one approval, and to block force pushes and deletion of `main`.
- Before requesting review, the PR author updates the branch with the latest `main`, runs the application and relevant tests, and completes the PR self-check.
- When CI is configured, the relevant build-and-test checks should be made required checks in the `main` ruleset. A PR must not be merged when a required check fails.
- Do not commit secrets such as `.env` files, API keys, passwords or database connection strings. Keep real credentials outside the repository and provide only a safe `.env.example` template.
- If a merge breaks `main`, notify the team and the Configuration and Build Manager immediately. The Configuration and Build Manager coordinates a revert through a reviewed PR. The original author then fixes the problem on a new task branch and submits a new PR.
- A broken `main` must not be used for a delivery milestone tag.

## 3. Commits

### 3.1. General Principles

- Each commit should represent one small, meaningful and coherent change.
- Do not combine unrelated changes in one commit.
- Keep the code in a working state; do not leave syntax errors or unfinished temporary debugging code.
- Do not commit generated output, dependency folders (such as `node_modules/`, `build/` or `dist/`), personal IDE settings or secrets. Add appropriate generated and local-only files to `.gitignore`.
- Commit regularly so work is traceable and recoverable.

### 3.2. Commit Message Format

The team uses **Conventional Commits**:

```text
<type>(<scope>): <short-description>
```

Common commit types:

| Type | Meaning |
|---|---|
| `feat` | Add functionality |
| `fix` | Fix a bug |
| `docs` | Change documentation |
| `style` | Formatting-only change with no behavior change |
| `refactor` | Restructure code without changing behavior |
| `test` | Add or update tests |
| `chore` | Configuration, dependencies or maintenance work |

The scope identifies the affected module, for example: `auth`, `movie`, `booking`, `schedule`, `payment`, `ai`, `ui` or `config`.

The short description must be in English, concise, start with a verb and not end with a period.

Examples:

```text
feat(booking): add 10-minute seat hold
fix(payment): correct ticket code in QR code
docs(report): update system context diagram
test(auth): add incorrect password login tests
chore(config): update development dependencies
```

Avoid vague messages such as `Update`, `fix bug` or `changes`.

## 4. Pull Requests

### 4.1. Creating a Pull Request

Open a PR when the work item is ready for review and the author has completed the self-check. A Draft PR may be opened earlier to request feedback, but it must clearly state what is incomplete.

Every PR must:

- Target `main`.
- Be linked to its JIRA key.
- Use a title written in English and include the JIRA key.
- Describe what changed, why it changed and how the reviewer can test it.
- Have at least one reviewer who is not the PR author.
- Include screenshots or other evidence when they help explain a UI or behavior change.

Suggested title format:

```text
<type>(<scope>): <short-description> [SCRUM-<number>]
```

Examples:

```text
feat(booking): add seat selection by showtime [SCRUM-42]
docs(report): update use case diagram [SCRUM-45]
test(booking): add seat hold concurrency tests [SCRUM-62]
```

Keep PRs focused and reasonably small. As a guideline, aim for fewer than approximately 400 changed lines when practical. If a change is larger, split it when doing so does not make the work harder to understand or integrate.

### 4.2. Pull Request Description

Use the following structure (the repository may store it as `.github/pull_request_template.md`):

```markdown
## Changes
- Describe what was changed.

## Reason
- JIRA key: SCRUM-...
- Requirement, task or bug addressed.

## How to Test
1. Steps for the reviewer.
2. Expected result.

## Evidence
- Screenshots, test output or other evidence (if applicable).

## Self-check
- [ ] The PR targets main and is linked to the correct JIRA issue.
- [ ] The branch name and PR title follow the naming rules.
- [ ] I updated my branch with the latest main.
- [ ] The application runs and relevant tests pass.
- [ ] There are no unresolved merge conflicts.
- [ ] No secrets are included.
- [ ] Related documentation is updated, if needed.
```

### 4.3. Review and Merge Conditions

A PR can be merged only when all of the following conditions are satisfied:

- At least one other team member has reviewed and approved it. The author cannot approve their own PR.
- All requested changes have been addressed and review conversations are resolved.
- Required CI/build/test checks pass, when those checks have been configured.
- The PR has no merge conflicts with `main`.
- The PR is linked to the correct JIRA work item and contains enough information for the team to understand and verify the change.

**The Configuration and Build Manager is responsible for merging approved PRs into `main`.** The PR author is responsible for implementing the change, responding to review comments, resolving code issues and keeping the branch up to date. The author must not merge their own PR.

Use **Squash and merge** as the team's standard merge method so each PR is represented by one integrated commit on `main`, unless the team explicitly agrees on another method for a specific reason.

After the merge, the task branch can be deleted and the JIRA work item can be updated according to the team's workflow.

## 5. Code Review

### 5.1. Purpose

Code review helps detect defects, check whether a change meets its requirement, maintain consistency and share technical knowledge. Reviewers should comment on the change, not on the person who wrote it.

### 5.2. Review Checklist

| Criterion | Questions |
|---|---|
| Requirements | Does the change satisfy the linked JIRA task or requirement? |
| Correctness | Are there logic errors or unhandled cases? |
| Readability | Are names clear? Is the code understandable and maintainable? |
| Consistency | Does the change follow the project's conventions and existing structure? |
| Security | Are credentials protected? Is user input validated? Are sensitive operations handled safely? |
| Testing | Are relevant tests included or updated? Do existing tests still pass? |
| Scope | Does the PR contain only changes related to its work item? |
| Documentation | Are related documents updated when necessary? |

### 5.3. Review Comments and Outcomes

Make review comments specific. Explain the reason and suggest a correction when possible.

| Prefix / Outcome | Meaning | Required action |
|---|---|---|
| `[Required]` | Defect, security concern or violation that must be fixed | Author fixes it before merge |
| `[Suggestion]` | Possible improvement; current implementation may still be acceptable | Author considers and responds |
| `[Question]` | Reviewer needs clarification | Author explains the decision or behavior |

The reviewer records the review outcome using GitHub:

- **Approve:** the change is acceptable for merge.
- **Request changes:** required issues must be fixed before approval.
- **Comment:** feedback or questions are provided without blocking approval by themselves.

## 6. JIRA Issues and Change Requests

JIRA is the system of record for the project backlog, work items, problems, bugs and change requests.

- Submit a problem, bug or change request as a JIRA issue so it can be tracked.
- Link the related branch and PR to the JIRA key.
- Review submitted problems and change requests in the weekly team meeting.
- Record the team's disposition using one of the SDP outcomes: **Approved**, **Rejected** or **Deferred** (SDP Section 4.3.1).
- Implement an approved change through the normal branch, PR and review workflow. Do not silently introduce an unapproved scope change.

The team should keep the JIRA issue status and the actual implementation status consistent.

## 7. Documents, Baselines and Milestone Tags

### 7.1. Document Management

Project documents are maintained in the shared team folder and exported to the Git repository at each submission. Documents related to source code, including design documents, should be included in the repository and in the relevant baseline.

Use this document naming convention:

```text
CineHub-<Type>-v<major>.<minor>
```

Example:

```text
CineHub-SDP-v1.2
```

Every document must describe each update in its **Revision History**. Documentation-only changes use a `docs/SCRUM-<number>-<short-name>` branch and are submitted through a PR like other changes.

### 7.2. Baseline Contents

Baselines include:

- All source code.
- Test scripts.
- Data files required by the project.
- Documentation related to the source code, such as design and architecture documents.

The final baseline of each iteration also includes all customer deliverables, including executables. The exact contents must match the relevant milestone and submission requirements in the SDP.

### 7.3. Project Milestones

The project uses these fixed milestone tags:

| Tag | Planned date | Milestone purpose | SDP reference |
|---|---|---|---|
| `prototype-s2` | End of Sprint 2 – 01/11/2026 | Internal prototype demonstrating the planned prototype scope; formal testing is not required at this stage | CH-25 |
| `elaboration-baseline` | End of Sprint 3 – 13/11/2026 | Elaboration architecture baseline, including project skeleton, CI, role-based login, seat-holding service and detailed design documents | 3.21 |
| `build-1` | End of Sprint 4 – 27/11/2026 | Build 1 beta, including the planned end-to-end testing and demonstration | 4.9 |
| `build-2` | End of Sprint 5 – 12/12/2026 | Build 2 final release | 5.16 |

These are project-wide snapshots. A milestone tag represents one commit containing the integrated project state required for that milestone; it is not a tag for an individual developer's branch or a single feature in isolation.

The detailed acceptance scope for each milestone must be checked against the corresponding SDP requirements and iteration plan. The prototype milestone's planned scope includes role-based login, Seat Map and Atomic Seat Holding. Other work, such as Call AI, may be included as additional scope but does not replace required items. A Seat Map is the visual seat-selection interface; Atomic Seat Holding is the concurrency-safe mechanism that prevents two users from holding the same seat at the same time.

### 7.4. Tagging Rules and Responsibility

- Use the exact milestone tag names listed in Section 7.3.
- Create **annotated tags** with a message describing the milestone and the state being marked.
- The **Configuration and Build Manager** verifies the required scope, confirms that the target commit is appropriate and buildable, creates the tag and announces it to the team.
- Under the normal workflow, all work required for the milestone must first be integrated into `main`. The tag points to the resulting integrated commit on `main`.
- Before `build-1` and `build-2` are tagged, the Test Team Leader confirms that the required build tests have been completed according to the test plan.
- Do not delete, move, overwrite or force-push a milestone tag before the end of the course.
- If a tag was created on the wrong commit, do not move or overwrite it. Record the reason in JIRA and create a corrected tag with a suffix (for example, `build-1-r2`), then inform the team.
- Configure a GitHub tag ruleset to restrict updates and deletions of the milestone tags. Until the ruleset has actually been created and verified, every member must follow the protection rules manually. Do not claim that GitHub enforcement is active before verifying the setting.

### 7.5. Tagging Procedure

For a normal milestone tag on `main`, the Configuration and Build Manager first confirms that the required PRs have been merged, the milestone scope is present and `main` is buildable. Then:

```bash
git checkout main
git pull origin main

# Review the commit that will be tagged
git log --oneline --decorate -5

# Create an annotated tag (use the correct milestone name and message)
git tag -a elaboration-baseline -m "CineHub Elaboration Baseline (Sprint 3)"

# Push the tag to GitHub
git push origin elaboration-baseline

# Verify the tag and the commit it points to
git show elaboration-baseline
git ls-remote --tags origin
```

Replace the example tag with the milestone that is actually ready. Do not run these commands before the milestone has been approved and verified. Do not recreate a tag that already exists.

**Limited prototype exception:** if the approved project plan requires the prototype tag before the prototype has been merged into `main`, the tag may point to one branch commit containing the complete required prototype scope, provided the team lead and Configuration and Build Manager confirm the exception against the SDP and document the decision. If the required work is spread across multiple branches, integrate it into one agreed commit first; one Git tag cannot point to multiple commits. The prototype work must subsequently be integrated into `main` according to the project plan. This exception does not create a permanent sprint branch or change the normal GitHub Flow.

## 8. Backup and Retention

The project uses the following backup arrangement:

- The remote Git repository on GitHub is a backup of the source repository.
- The shared team folder is the backup location for project documents.
- Every team member keeps a local clone of the Git repository.
- Do not delete milestone tags or submitted documents before the end of the course.
- Do not rely on a single local working copy as the only copy of project work.

If a backup or synchronization problem is discovered, inform the Configuration and Build Manager and team lead so the team can verify the available copies before making changes.

## 9. Overall Workflow

For each work item:

1. Select or receive the work item in JIRA.
2. Create a short-lived branch from the latest `main`, using the correct prefix and JIRA key.
3. Implement the work and commit changes using the commit message convention.
4. Update the branch with the latest `main`, run the application and relevant tests, and complete the self-check.
5. Push the branch and open a PR targeting `main`. Link the PR to the JIRA issue and assign at least one other team member as reviewer.
6. The reviewer checks the change and records an outcome. The author addresses required comments and requests another review when needed.
7. Once all merge conditions are satisfied, the Configuration and Build Manager merges the PR. The task branch is then deleted and the JIRA issue is updated.
8. For a problem or change request, follow the JIRA review and disposition process in Section 6.
9. At each submission, export the required project documents from the shared team folder to the repository.
10. At a milestone, the Configuration and Build Manager verifies the milestone scope and build state, coordinates any required testing, creates the annotated tag and prepares the build with the team.

## 10. Roles and Responsibilities

| Role | Responsibilities |
|---|---|
| Team lead / Project Manager | Plan and coordinate sprints; help resolve team-level disagreements; approve milestone-specific exceptions with the Configuration and Build Manager. |
| Configuration and Build Manager | Maintain the `main` ruleset; configure and verify tag protection; coordinate repository integration; merge approved PRs; resolve or coordinate merge conflicts; monitor branch, PR and tag compliance; create milestone tags; coordinate build preparation and repository recovery. |
| Requirements Analyst / Documentation Manager | Coordinate requirements and project documents; keep document versions and Revision Histories consistent; ensure required documents are exported to the repository at submissions. |
| Test Team Leader | Coordinate test activities; review test-related changes; confirm completion of required build tests before `build-1` and `build-2`. |
| Architecture and AI Integration Technical Lead | Guide architecture and AI integration decisions; support technical consistency and integration. |
| Developers (all team members) | Create and manage task branches; implement assigned work; write or update tests and documentation as needed; self-check; respond to review comments; review assigned PRs. |
| Whole team | Keep `main` buildable; maintain local clones; communicate risks and conflicts early; preserve tags and submitted documents until the end of the course. |

## 11. Repository Configuration Checklist

The following checklist records repository configuration status. Change an item only after the corresponding GitHub setting or workflow has actually been configured and verified.

- [x] `main` ruleset: require Pull Requests, require at least one approval, block force pushes and restrict deletion.
- [ ] CI workflow: configure build-and-test checks and make the appropriate checks required when ready.
- [x] Tag ruleset: target the milestone tags and restrict updates and deletions.