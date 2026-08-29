## Overview

更正 Brief 技能的語言規則，明確規定產出的 Markdown 檔案應使用 AI Agent 與使用者當下交談的語言；若無法判斷應使用哪種語言，則預設使用英文。

## Scope

- `SKILL.md`: 更正產出 Markdown 檔案時所使用語言的規則。

## Requirements

- [V] R1: 將語言規則改為：產出的 Markdown 檔案使用 AI Agent 與使用者交談的語言撰寫。
- [V] R2: 明確規定無法判斷交談語言時，產出的 Markdown 檔案預設使用英文。

## Acceptance Criteria

- [ ] AC1: Scenario: 檢查 `SKILL.md` 的語言規則時，內容明確要求產出的 Markdown 檔案使用 AI Agent 與使用者交談的語言。
- [ ] AC2: Scenario: 無法判斷應使用哪種語言時，`SKILL.md` 明確指定英文為產出 Markdown 檔案的預設語言。
