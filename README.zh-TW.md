# Brief

Brief 是一個適用於 Codex 與 Claude Code 的輕量規格驅動開發 Skill。

它的核心原則很簡單：

> 寫程式之前，先把變更寫清楚。

工作流程包含四個指令：

```text
plan -> make -> check -> close
```

## 指令

### `plan`

在實作前定義變更。Brief 會建立一份提案檔：

```text
brief/YYYYMMDD-{category}-{short-name}.md
```

提案包含：

- 概述（Overview）
- 範圍（Scope）
- 需求（Requirements）
- 驗收條件（Acceptance Criteria）

Brief 會顯示提案並等待核准。在執行 `plan` 時，它不會撰寫實作程式碼。

### `make`

實作已核准的提案。Brief 每次處理一項需求，盡可能重用現有程式碼，並將每項已完成的需求標記為 `- [V]`。

### `check`

使用直接證據檢查每項驗收條件。Brief 只會在驗收條件通過後，才將其標記為 `- [V]`。

### `close`

確認所有需求與驗收條件皆已完成，接著將提案移至：

```text
brief/archive/
```

## 安裝方式

### 選項 1：透過 npx 安裝

```sh
npx skills add roga/brief
```

### 選項 2：請 AI Agent 安裝 Brief

你可以將儲存庫網址提供給 Codex 或 Claude Code，並請它安裝 Brief。例如：

```text
請從 https://github.com/roga/brief 這個 Git 儲存庫安裝 Brief Skill。
請依照儲存庫中的「手動安裝」章節進行，讓 Codex 與 Claude Code 都能使用它。
安裝時請勿覆寫任何現有的 Skill；如果遇到衝突，請先詢問我。
```

### 選項 3：手動安裝

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

- 注意：請確認尚未安裝名為 `brief` 的 Skill。
- 相容性說明（於 2026-08-30 驗證）：Agent Skills 規格並未規定安裝路徑，
  但 `~/.agents/skills` 是讓相容 AI Agent 共用 Skill 時廣泛採用的慣例。
  Claude Code 目前會從 `~/.claude/skills` 尋找個人 Skill，因此上述手動安裝方式
  會建立一個指向共用副本的符號連結。請參閱
  [Agent Skills 實作指南](https://agentskills.io/client-implementation/adding-skills-support)
  與 [Claude Code Skills 文件](https://code.claude.com/docs/en/slash-commands)。

## 搭配 AI Agent 使用

```text
$brief plan 新增待辦清單
$brief make
$brief check
$brief close
```

如果同時存在多份尚未結案的提案，請在指令中加上提案檔名或路徑。

## 原則

- 實作前先規劃。
- 一次只處理一份提案。
- 不加入提案未要求的功能。
- 不過度設計。
- 重用現有程式碼。
- 遵循專案的程式碼風格與慣例。
- 遇到不清楚或矛盾之處時，停止並詢問使用者。

## 致謝

Brief 的概念源自 [kaochenlong](https://gist.github.com/kaochenlong) 所撰寫的[SimpleSDD.txt](https://gist.github.com/kaochenlong/27ade9a6218244c2584777fa276d1214)，特此致謝。
