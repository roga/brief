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

## Installation

### Option 1: Install via npx

```sh
npx skills add roga/brief
```

### Option 2: Ask an AI agent to install Brief

You can give Codex or Claude Code the repository URL and ask it to install Brief. For example:

```text
Install the Brief Skill from the git repository at https://github.com/roga/brief.
Follow the "Manual installation" section in the repository so that both Codex
and Claude Code can use it. Do not overwrite any existing Skills during the
installation; ask me first if you encounter any conflicts.
```

### Option 3: Manual installation

```sh
#!/usr/bin/env sh

set -e

tmp="$(mktemp -d)"
trap 'rm -rf "$tmp"' 0

git clone --depth 1 https://github.com/roga/brief.git "$tmp/skill"
rm -rf "$tmp/skill/.git"

if [ -e ~/.agents/skills/brief ] || [ -L ~/.agents/skills/brief ]; then
  echo "A Skill named brief already exists in ~/.agents/skills." >&2
  exit 1
fi

if [ -e ~/.claude/skills/brief ] || [ -L ~/.claude/skills/brief ]; then
  echo "A Skill named brief already exists in ~/.claude/skills." >&2
  exit 1
fi

mkdir -p ~/.agents/skills ~/.claude/skills

cp -R "$tmp/skill" ~/.agents/skills/brief
ln -s ~/.agents/skills/brief ~/.claude/skills/brief
```

- Note: Make sure you do not already have a Skill named `brief` installed.
- Compatibility note (verified 2026-08-30): The Agent Skills specification does
  not mandate installation paths, but `~/.agents/skills` is a widely adopted
  convention for sharing Skills across compatible AI agents. Claude Code
  currently discovers personal Skills from `~/.claude/skills`, so the manual
  installation above creates a symbolic link to the shared copy. See the
  [Agent Skills implementation guide](https://agentskills.io/client-implementation/adding-skills-support)
  and [Claude Code Skills documentation](https://code.claude.com/docs/en/slash-commands).

## Use with AI Agents

```text
$brief plan add a todo list
$brief make
$brief check
$brief close
```

<img src="https://github.com/user-attachments/assets/7eb03d6f-6a24-476d-a093-8c6c868f6e50" alt="screenshot">

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
