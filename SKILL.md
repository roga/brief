---
name: brief
description: Run a small spec-driven workflow with plan, make, check, and close. Use when a user wants to define a code change before implementation, build an approved proposal, verify the result, or archive completed work.
---

# Brief

Write down the change before writing code.

## Commands

Read the first argument as the command:

- `plan`: Write a proposal and wait for approval.
- `make`: Implement an approved proposal.
- `check`: Check the implementation against the proposal.
- `close`: Archive a completed proposal.

If the command is missing or unknown, show these four commands and ask the user to choose one. Do not change files until the command is clear.

## General rules

- Never write implementation code before a proposal is created with `plan` and approved by the user.
- Work on one proposal at a time.
- Do not add features that the proposal does not request.
- Do not over-design the solution.
- Reuse existing code when it fits.
- Follow the project's coding style and conventions.
- If anything is unclear, contradictory, incorrect, or needs a user decision, stop and ask a specific question. Do not guess.
- Ask before any dangerous operation. Explain what could happen.

## Plan

Use this mode only when the command is `plan`.

1. Read the user's request. If no request is given, ask for one and stop.
2. Inspect the project only as needed to understand existing behavior, file names, style, and likely scope. Do not change implementation files.
3. If the request is unclear or contradictory, ask focused questions and stop. Create the proposal only after the important details are clear.
4. Choose exactly one category:
   - `feature`: Add new behavior.
   - `fix`: Correct broken behavior.
   - `refactor`: Change code structure without changing behavior.
   - `other`: Work that does not fit the first three categories.
5. Create a short English name. Use lowercase words joined by hyphens, such as `add-todo` or `fix-login`.
6. Create `brief/` if it does not exist. Use the current local date in `YYYYMMDD` format. The proposal path is:

   `brief/YYYYMMDD-{category}-{short-name}.md`

7. Write the proposal with this format:

```markdown
## Overview

A short, clear description of the requested change and the problem it solves.

## Scope

- `path/to/file`: What may change in this file.
- `path/to/new-file`: New file and its purpose.

## Requirements

- [ ] R1: One behavior to add or change.
- [ ] R2: Another behavior to add or change.

## Acceptance Criteria

- [ ] AC1: Scenario: Describe an observable result in plain language.
- [ ] AC2: Scenario: Describe another observable result in plain language.
```

8. Keep each requirement small enough to implement and mark separately. Do not write more than 30 requirements. If the change needs more than 30, do not create the proposal; ask the user to reduce or split the request.
9. Write enough acceptance criteria to check the requested behavior. They do not need a one-to-one mapping to requirements. Use concrete scenarios that describe what should happen.
10. Read the saved proposal and show its full content to the user.
11. End with: `Do you want to change the proposal? If it is approved, use brief make to start implementation.`
12. Stop and wait. Do not write implementation code until the user runs `make`.

## Make

Use this mode only when the command is `make`. The `make` command means the user approved the proposal.

1. Find the proposal:
   - If the user gives a file name or path, use that proposal.
   - Otherwise, use the proposal active in the current conversation.
   - If there is no active proposal, look for Markdown files directly under `brief/`. Ignore `brief/archive/`.
   - If exactly one proposal is available, use it.
   - If none or more than one are available, stop and ask the user to identify the proposal. Do not guess.
2. Read the whole proposal before changing implementation files.
3. Confirm that it has `Requirements` and `Acceptance Criteria`. If the proposal is incomplete, unclear, or contradictory, explain the problem and stop.
4. Work through unchecked requirements from top to bottom. Complete only one requirement at a time.
5. Before each requirement:
   - Read that requirement and the acceptance criteria again.
   - Inspect the relevant project files.
   - Look for existing code, patterns, helpers, and components that can be reused.
6. Implement only what the current requirement needs. Follow the existing coding style and conventions.
7. Check the result against the requirement and relevant acceptance criteria. If it is not complete, keep working on that requirement. If the proposal is wrong or a decision is needed, stop and tell the user exactly why.
8. Only after the requirement is complete, change its marker in the proposal from `- [ ] R#` to `- [V] R#`.
9. Read the proposal again and confirm that the marker was saved as `- [V]`.
10. Report `R# complete` with a short description of the result, then continue with the next unchecked requirement unless user input is needed.
11. Do not mark acceptance criteria during `make`.
12. When every requirement is marked `- [V]`, report that all requirements are complete and ask the user to run `brief check`.

## Check

Use this mode only when the command is `check`.

1. Find the proposal by using the same selection rules as `make`.
2. Read the whole proposal and confirm that every requirement is marked `- [V]`. If any requirement is unchecked, list it and stop. Ask the user to finish it with `brief make`.
3. Work through acceptance criteria from top to bottom. Check only one criterion at a time.
4. Before each check, choose evidence that directly proves the scenario. Use the safest suitable method, such as an automated test, a focused command, code inspection, or observable runtime behavior. Do not use a narrow check to claim a broader result.
5. Ask the user when access, credentials, an external service, or a manual action is needed. Before a dangerous operation, explain the operation and its possible consequences, then wait for approval.
6. If the criterion passes:
   - Change its marker from `- [ ] AC#` to `- [V] AC#`.
   - Add a short indented `Evidence:` line below it with the command, observation, or inspected behavior that proves the result.
   - Read the proposal again and confirm that the marker and evidence were saved.
   - Report `AC# passed` with a short summary, then continue to the next unchecked criterion.
7. If the criterion fails or cannot be proven, do not mark it. Report the expected result, actual result, and useful evidence. Stop and ask the user how to proceed. Do not fix implementation code during `check`.
8. When every acceptance criterion is marked `- [V]`, report that all checks passed and ask the user to run `brief close`.

## Close

Use this mode only when the command is `close`.

1. Find the proposal by using the same selection rules as `make`.
2. Read the whole proposal. Confirm that every requirement and every acceptance criterion is marked `- [V]`.
3. If any item is unchecked, list it and stop. Do not archive incomplete work.
4. Create `brief/archive/` if it does not exist.
5. Move the proposal file into `brief/archive/` without changing its file name. Do not overwrite an existing archive file. If the target already exists, stop and ask the user how to resolve it.
6. Confirm that the archived file exists at its new path.
7. Report that the proposal is closed. Add one short paragraph that summarizes the request, the implementation, and the verified result so the user can understand what was completed later.
