---
name: claudecodecost-2-copilot-aicredit
description: 入力・出力・キャッシュ等のトークン使用量をGitHub Copilotのモデル料金表に基づいてUSD料金およびAI creditsに変換・計算するスキル。
license: MIT
---

# GitHub Copilot トークン使用量・料金変換スキル

入力されたトークン使用量（入力トークン、キャッシュされた入力トークン、キャッシュ書き込みトークン、出力トークン）を、GitHub Copilotのモデル別価格表（[GitHub Copilot のモデルと価格設定](https://docs.github.com/ja/copilot/reference/copilot-billing/models-and-pricing)）に基づいて、USD金額および **AI credits** に変換・計算する手順と仕様です。
2026年8月29日時点の価格で計算します

---

## 1. 料金計算の基本仕様

1. **AI Credit 単位**
   - **1 GitHub AI credit = $0.01 USD** （1 USD = 100 AI credits）

2. **価格表の基準**
   - すべての価格は **100万トークン（1M tokens）あたり** のUSD料金です。

3. **計算式**
   $$\text{Total Cost (USD)} = \frac{\text{Input Tokens}}{1,000,000} \times P_{\text{input}} + \frac{\text{Cached Input Tokens}}{1,000,000} \times P_{\text{cached}} + \frac{\text{Cache Write Tokens}}{1,000,000} \times P_{\text{write}} + \frac{\text{Output Tokens}}{1,000,000} \times P_{\text{output}}$$

   $$\text{Total Cost (AI Credits)} = \text{Total Cost (USD)} \times 100$$

4. **Long Context（長いコンテキスト）レベルの自動判定**
   特定のモデルでは入力トークン数がしきい値を超えると **Long context レベル** の単価が適用されます。
   - GPT-5.4, GPT-5.5, GPT-5.6 Sol, GPT-5.6 Terra: 入力トークン > 272,000 (272K)
   - GPT-5.6 Luna, Gemini 3.1 Pro, Grok 4.5, Grok 4.6: 入力トークン > 200,000 (200K)

---

## 2. GitHub Copilot 価格表 (100万トークンあたり USD)

### OpenAI

| Model | レベル | しきい値 (入力トークン) | 入力 ($/1M) | キャッシュ入力 ($/1M) | キャッシュ書き込み ($/1M) | 出力 ($/1M) |
|---|---|---|---|---|---|---|
| GPT-5 mini | Default | N/A | $0.25 | $0.025 | N/A | $2.00 |
| GPT-5.3-Codex | Default | N/A | $1.75 | $0.175 | N/A | $14.00 |
| GPT-5.4 | Default | ≤ 272K | $2.50 | $0.25 | N/A | $15.00 |
| GPT-5.4 | Long context | > 272K | $5.00 | $0.50 | N/A | $22.50 |
| GPT-5.4 mini | Default | N/A | $0.75 | $0.075 | N/A | $4.50 |
| GPT-5.4 nano | Default | N/A | $0.20 | $0.02 | N/A | $1.25 |
| GPT-5.5 | Default | ≤ 272K | $5.00 | $0.50 | N/A | $30.00 |
| GPT-5.5 | Long context | > 272K | $10.00 | $1.00 | N/A | $45.00 |
| GPT-5.6 Luna | Default | ≤ 200K | $0.20 | $0.02 | $0.25 | $1.20 |
| GPT-5.6 Luna | Long context | > 200K | $0.40 | $0.04 | $0.50 | $1.80 |
| GPT-5.6 Sol | Default | ≤ 272K | $2.00 | $0.20 | $2.50 | $10.00 |
| GPT-5.6 Sol | Long context | > 272K | $4.00 | $0.40 | $5.00 | $15.00 |
| GPT-5.6 Terra | Default | ≤ 272K | $2.00 | $0.20 | $2.50 | $12.00 |
| GPT-5.6 Terra | Long context | > 272K | $4.00 | $0.40 | $5.00 | $18.00 |

### Anthropic

| Model | 入力 ($/1M) | キャッシュ入力 ($/1M) | キャッシュ書き込み ($/1M) | 出力 ($/1M) |
|---|---|---|---|---|
| Claude Haiku 4.5 | $1.00 | $0.10 | $1.25 | $5.00 |
| Claude Sonnet 4 | $3.00 | $0.30 | $3.75 | $15.00 |
| Claude Sonnet 4.5 | $3.00 | $0.30 | $3.75 | $15.00 |
| Claude Sonnet 4.6 | $3.00 | $0.30 | $3.75 | $15.00 |
| Claude Opus 4.5 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.6 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.7 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 4.8 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Opus 5 | $5.00 | $0.50 | $6.25 | $25.00 |
| Claude Sonnet 5 | $2.00 | $0.20 | $2.50 | $10.00 |
| Claude Opus 4.8 (fast mode) | $10.00 | $1.00 | $12.50 | $50.00 |
| Claude Fable 5 | $10.00 | $1.00 | $12.50 | $50.00 |

### Google

| Model | レベル | しきい値 (入力トークン) | 入力 ($/1M) | キャッシュ入力 ($/1M) | 出力 ($/1M) |
|---|---|---|---|---|---|
| Gemini 3.1 Pro | Default | ≤ 200K | $2.00 | $0.20 | $12.00 |
| Gemini 3.1 Pro | Long context | > 200K | $4.00 | $0.40 | $18.00 |
| Gemini 3.5 Flash | Default | N/A | $1.50 | $0.15 | $9.00 |
| Gemini 3.6 Flash | Default | N/A | $0.75 | $0.075 | $3.75 |
| Gemini 3.7 Flash | Default | N/A | $0.75 | $0.075 | $3.75 |

### 微調整 (GitHub Fine-tuned)

| Model | 入力 ($/1M) | キャッシュ入力 ($/1M) | 出力 ($/1M) |
|---|---|---|---|
| Raptor mini | $0.25 | $0.025 | $2.00 |

### Microsoft

| Model | 入力 ($/1M) | キャッシュ入力 ($/1M) | 出力 ($/1M) |
|---|---|---|---|
| MAI-Code-1-Flash | $0.75 | $0.075 | $4.50 |
| MAI-Code-1.1-Flash | $0.20 | $0.02 | $1.20 |

### xAI

| Model | レベル | しきい値 (入力トークン) | 入力 ($/1M) | キャッシュ入力 ($/1M) | 出力 ($/1M) |
|---|---|---|---|---|---|
| Grok 4.5 | Default | ≤ 200K | $2.00 | $0.50 | $6.00 |
| Grok 4.5 | Long context | > 200K | $4.00 | $1.00 | $12.00 |
| Grok 4.6 | Default | ≤ 200K | $2.00 | $0.50 | $6.00 |
| Grok 4.6 | Long context | > 200K | $4.00 | $1.00 | $12.00 |

### Moonshot AI

| Model | 入力 ($/1M) | キャッシュ入力 ($/1M) | 出力 ($/1M) |
|---|---|---|---|
| Kimi K2.7 Code | $0.95 | $0.19 | $4.00 |
| Kimi K3 | $3.00 | $0.30 | $15.00 |

---

## 3. 計算入力形式と実行手順

ユーザーまたはシステムから以下の形式で入力データを受け取り、使用量を計算します。

### 入力パラメータ
- **Model**: 使用したモデル名（例: `Claude Sonnet 4.6`, `GPT-5.4`, `Gemini 3.6 Flash`）
- **Input Tokens**: 通常の入力トークン数
- **Cached Input Tokens**: キャッシュされた入力トークン数（該当する場合）
- **Cache Write Tokens**: キャッシュ書き込みトークン数（該当する場合）
- **Output Tokens**: 出力トークン数

### 計算出力フォーマット例

```markdown
### トークン使用量 料金計算結果

- **対象モデル**: Claude Sonnet 4.6
- **使用トークン内訳**:
  - 入力トークン: 50,000
  - キャッシュ入力トークン: 200,000
  - キャッシュ書き込みトークン: 10,000
  - 出力トークン: 5,000
- **単価 (1Mトークンあたり)**:
  - 入力: $3.00 / キャッシュ入力: $0.30 / キャッシュ書き込み: $3.75 / 出力: $15.00
- **小計**:
  - 入力コスト: (50,000 / 1,000,000) * $3.00 = $0.1500
  - キャッシュ入力コスト: (200,000 / 1,000,000) * $0.30 = $0.0600
  - キャッシュ書き込みコスト: (10,000 / 1,000,000) * $3.75 = $0.0375
  - 出力コスト: (5,000 / 1,000,000) * $15.00 = $0.0750
- **合計概算料金**:
  - **USD**: **$0.3225 USD**
  - **AI Credits**: **32.25 AI Credits**
```

---

## 4. 特記事項

- **コード補完 (Code Completion)**
  コード補完および next edit suggestions は AI credits での課金対象外（全有料 Copilot プランで無制限）です。
- **プロモーション料金情報**
  - Gemini 3.6 Flash / 3.7 Flash: 2026年12月31日までプロモーション価格（入力 $0.75 / 1M, キャッシュ入力 $0.075 / 1M, 出力 $3.75 / 1M）。
  - GPT-5.6 Sol: 2026年9月3日までプロモーション価格（標準レートの50%引き）。
