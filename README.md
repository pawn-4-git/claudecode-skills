# claudecode-skills

Claude Code などの AI コーディングエージェント向けに最適化されたエージェントスキル（Agent Skills）を管理・配布するリポジトリです。

---

## 📦 収録スキル一覧

| スキル名 | 説明 | ライセンス |
|---|---|---|
| [`claudecodecost-2-copilot-aicredit`](skills/claudecodecost-2-copilot-aicredit/SKILL.md) | トークン使用量をGitHub Copilotのモデル別料金表に基づいてUSD金額およびAI creditsに計算・変換するスキル | MIT |

---

## 🛠️ インストール方法

GitHub CLI の `gh skill` コマンドを使用して、Claude Code に簡単にインストールできます。

### 1. ユーザー全体（グローバル）にインストール（推奨）
どのプロジェクトで Claude Code を実行してもスキルが有効になります。

```bash
gh skill install pawn-4-git/claudecode-skills claudecodecost-2-copilot-aicredit --agent claude-code --scope user
```

### 2. 特定のプロジェクトのみにインストール
現在の Git リポジトリ内（`.agents/skills/`）にのみインストールします。

```bash
gh skill install pawn-4-git/claudecode-skills claudecodecost-2-copilot-aicredit --agent claude-code --scope project
```

### 3. ローカル開発・テスト用インストール
リポジトリをクローンまたはローカルで変更後、直接インストールする場合：

```bash
gh skill install . claudecodecost-2-copilot-aicredit --from-local --agent claude-code --scope user
```

---

## 🔍 スキル詳細

### 1. `claudecodecost-2-copilot-aicredit`

- **概要**:
  LLM のトークン使用量（入力、キャッシュされた入力、キャッシュ書き込み、出力）を入力とし、[GitHub Copilot のモデルと価格設定](https://docs.github.com/ja/copilot/reference/copilot-billing/models-and-pricing) に基づいて概算コスト（USD）と **GitHub AI credits**（1 AI credit = $0.01 USD）を算定します。

- **対応モデル例**:
  - **OpenAI**: GPT-5.6 (Luna / Sol / Terra), GPT-5.5, GPT-5.4, GPT-5.3-Codex, GPT-5 mini/nano
  - **Anthropic**: Claude Sonnet 4 / 4.5 / 4.6 / 5, Claude Opus 4.5〜5, Claude Haiku 4.5
  - **Google**: Gemini 3.1 Pro, Gemini 3.5 / 3.6 / 3.7 Flash
  - **その他**: xAI Grok 4.5 / 4.6, Microsoft MAI-Code-1.1, Moonshot Kimi K2.7/K3, Raptor mini

- **機能特徴**:
  - Long Context（長いコンテキスト > 272K または > 200K）の自動しきい値判定
  - キャッシュ書き込み単価の個別加算
  - コード補完（Code Completion）の無制限（課金対象外）仕様への対応

---

## 🔄 スキルの更新と管理

- **インストール済みスキルの更新**:
  ```bash
  gh skill update --all
  ```

- **インストール済みスキルの一覧表示**:
  ```bash
  gh skill list
  ```

- **リポジトリへの公開・検証（メンテナ用）**:
  ```bash
  gh skill publish --dry-run
  gh skill publish
  ```

---

## 📄 ライセンス

[MIT License](LICENSE)
