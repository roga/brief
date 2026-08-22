# Brief

Brief is a small spec-driven development skill for Codex and Claude Code.

Its main rule is simple:

> Write down the change before writing code.

The workflow has four commands:

```text
plan -> make -> check -> close
```

## Commands

### `plan`

Define the change before implementation. Brief creates one proposal file at:

```text
brief/YYYYMMDD-{category}-{short-name}.md
```

The proposal contains:

- Overview
- Scope
- Requirements
- Acceptance Criteria

Brief shows the proposal and waits for approval. It does not write implementation code during `plan`.

### `make`

Implement an approved proposal. Brief works on one requirement at a time, reuses existing code when possible, and marks each completed requirement as `- [V]`.

### `check`

Check every acceptance criterion with direct evidence. Brief marks a criterion as `- [V]` only after it passes.

### `close`

Confirm that all requirements and acceptance criteria are complete, then move the proposal to:

```text
brief/archive/
```

## Install

### Ask an AI agent to install Brief

If your AI coding agent can access GitHub and your local project files, you can give it the repository URL and ask it to install Brief for you. For example:

```text
Install the Brief skill from https://github.com/roga/brief for this project.
Make it available to both Codex and Claude Code by following the installation
instructions in the repository. Do not overwrite existing skill links; ask me
if you find a conflict.
```

The agent should clone the repository and link it into the skill directories described below. You can also perform the same steps manually.

### Manual installation

Clone this repository, then link the same skill folder into Codex and Claude Code. Replace `/absolute/path/to/brief` with the path to this repository.

### Project installation

From the project that will use Brief:

```sh
mkdir -p .agents/skills .claude/skills
ln -s /absolute/path/to/brief .agents/skills/brief
ln -s /absolute/path/to/brief .claude/skills/brief
```

Codex reads `.agents/skills/brief/SKILL.md`. Claude Code reads `.claude/skills/brief/SKILL.md`. Both links use the same `SKILL.md`.

## Use with Codex

```text
$brief plan add a todo list
$brief make
$brief check
$brief close
```

## Use with Claude Code

```text
/brief plan add a todo list
/brief make
/brief check
/brief close
```

If more than one open proposal exists, add the proposal file name or path to the command.

## Principles

- Plan before implementation.
- Work on one proposal at a time.
- Do not add unrequested features.
- Do not over-design.
- Reuse existing code.
- Follow the project's coding style and conventions.
- Stop and ask when something is unclear or contradictory.

## Acknowledgements

The concept behind Brief was inspired by [kaochenlong](https://gist.github.com/kaochenlong)'s [SimpleSDD.txt](https://gist.github.com/kaochenlong/27ade9a6218244c2584777fa276d1214).
