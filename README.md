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

You can give Codex or Claude Code the repository URL and ask it to install Brief. For example:

```text
Install the Brief Skill from the git repository at https://github.com/roga/brief.
Follow the "Manual installation" section in the repository so that both Codex
and Claude Code can use it. Do not overwrite any existing Skills during the
installation; ask me first if you encounter any conflicts.
```

### Manual installation

```sh
#!/usr/bin/env sh

set -e

tmp="$(mktemp -d)"
trap 'rm -rf "$tmp"' 0

git clone --depth 1 https://github.com/roga/brief.git "$tmp/skill"
rm -rf "$tmp/skill/.git"

mkdir -p ~/.codex/skills ~/.claude/skills

cp -R "$tmp/skill" ~/.codex/skills/brief
cp -R "$tmp/skill" ~/.claude/skills/brief
```

- Note: Make sure you do not already have a Skill named `brief` installed.

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
