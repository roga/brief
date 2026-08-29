## Overview

更新 README 的安裝與使用說明，以 `~/.agents/skills` 作為跨 AI Agent 共用的主要安裝路徑，並依截至 2026-08-30 的相容性狀況，說明 Claude Code 仍需透過 `~/.claude/skills` 的額外連結才能使用同一份 skill。同時將 Codex 與 Claude Code 的個別使用段落整合為通用的 AI Agents 使用說明與範例。

## Scope

- `README.md`: 更新手動安裝指令、具日期的路徑相容性說明，以及合併後的 AI Agents 使用範例。

## Requirements

- [V] R1: 將 `Option 3: Manual installation` 的主要安裝位置改為 `~/.agents/skills/brief`，不再為每個 AI Agent 複製一份 skill。
- [V] R2: 在手動安裝流程中，為 Claude Code 建立指向 `~/.agents/skills/brief` 的 `~/.claude/skills/brief` 符號連結，並維持既有的衝突保護意圖，不覆寫同名 skill。
- [V] R3: 加入截至 2026-08-30 的相容性註記，說明 `~/.agents/skills` 是跨 Agent 共用慣例，而 Claude Code 目前仍原生使用 `~/.claude/skills`，因此需要額外連結。
- [V] R4: 將 `Use with Codex` 與 `Use with Claude Code` 合併為 `Use with AI Agents`，並提供 `$brief` 的完整工作流程範例，不再提供 Claude Code 的 `/brief` 範例。

## Acceptance Criteria

- [ ] AC1: Scenario: 依照手動安裝指令執行時，Brief 只會被複製到 `~/.agents/skills/brief`，Claude Code 則透過 `~/.claude/skills/brief` 符號連結存取同一份 skill，且既有同名路徑不會被靜默覆寫。
- [ ] AC2: Scenario: 閱讀手動安裝說明時，可以看見標示為截至 2026-08-30 的跨 Agent 路徑與 Claude Code 相容性資訊。
- [ ] AC3: Scenario: 閱讀使用說明時，只會看到一個 `Use with AI Agents` 段落，其中包含 `$brief plan/make/check/close` 的可直接參考範例，且不包含 Claude Code 的 `/brief` 範例。
