# Skill: /conversation-log

Save the current conversation as a structured Markdown log file.

## Invocation

```
/conversation-log
```

No arguments required. When called, extract all substantive topics from the current conversation and save them as log files.

## Behavior

1. **Identify topics** — Scan the current conversation for all substantive exchanges (development, business, consultation, etc.). If multiple distinct topics exist, handle each separately.

2. **Determine save path** — Use the format `chat-logs/YYYYMMDD_{topic-name}.md` where:
   - `YYYYMMDD` is today's date (e.g., `20260325`)
   - `{topic-name}` is a short, lowercase, hyphen-separated description (e.g., `aws-contract-renewal`)
   - If a **different** topic uses the same date prefix, append `_2`, `_3`, etc.

3. **Check for existing file** — If a file with the same date and topic name already exists, **append** new conversation content following the Append Mode procedure below (append to 会話の流れ, update 要約/決定事項/次のアクション). Then skip to step 6.

4. **Create the `chat-logs/` directory** if it does not exist.

5. **Write the log file** using the format below.

6. **Report** the saved file path: `ログを保存しました: chat-logs/YYYYMMDD_{topic-name}.md`

## Log File Format

```markdown
# 会話ログ: {トピック}

| 項目 | 内容 |
|------|------|
| 日時 | YYYY-MM-DD HH:MM |
| カテゴリ | {business/support/consultation/decision/development/other} |
| 関連先 | {会社名・サービス名・人名 etc.} |

## 要約
{3–5行の要約。何について話し、何が明らかになったかを簡潔に記述}

## 会話の流れ

**User**: {ユーザーの発言をそのまま転記。原文のニュアンスを保つ}

**Assistant**: {アシスタントの回答・提案・作成した内容を転記}

**User**: {ユーザーの返答・追加情報・判断を転記}

**Assistant**: {続くやり取りを全て転記}

...

## 主要な決定事項
- {決定または合意した内容}

## 送付・作成した文面
> {会話中に作成したメッセージ・メール・文書があれば全文引用。なければこのセクションを省略}

## 次のアクション
- [ ] {フォローアップが必要なTODO}
```

## Field Guidelines

| Field | How to fill |
|-------|-------------|
| トピック | Brief descriptive title of the conversation topic |
| 日時 | Use `YYYY-MM-DD HH:MM` format — include the time, not just the date |
| カテゴリ | One of: `business`, `support`, `consultation`, `decision`, `development`, `other` |
| 関連先 | Company name, service name, person name, or "不明" if unknown |
| 要約 | 3–5 lines; factual and concise — quick overview for scanning |
| 主要な決定事項 | Bullet list; omit if no decisions were made |
| 送付・作成した文面 | Block-quote any drafted messages/emails in full; omit section entirely if none |
| 次のアクション | Checkbox list of follow-up tasks; omit if none |

## 「会話の流れ」セクションのルール（厳守）

このセクションが本スキルの**最重要セクション**。以下のルールを厳守すること。

### 必須フォーマット

全てのやり取りを `**User**:` / `**Assistant**:` の交互形式で記録する。この形式以外は禁止。

### 禁止事項

- ❌ 番号付きリスト（`1. 〇〇した 2. 〇〇した`）で経緯をまとめる
- ❌ 「ユーザーが〇〇を質問し、アシスタントが〇〇と回答した」のような三人称での要約
- ❌ 「〇〇について話し合った」のような抽象化
- ❌ 発言の省略・圧縮（短い発言でも全件記録）

### 必須事項

- ✅ ユーザーの発言は**原文のニュアンスを保って転記**（口語・敬語・略語もそのまま）
- ✅ アシスタントの回答も**実際に言った内容を転記**（提案文・説明文など）
- ✅ ユーザーが貼り付けたメール・データ・スクリーンショットの説明は**そのまま含める**
- ✅ アシスタントが作成した文面（メール・回答文等）は**全文転記**
- ✅ 純粋な相槌（「ok」「了解」）のみ省略可。それ以外は全て記録

### コンテキスト圧縮への対処

セッション後半でコンテキスト圧縮により前半の会話が失われている場合:
- 記憶にある範囲で最善の再現を試みる
- 再現できない部分は `（※コンテキスト圧縮により原文不明）` と明記する
- **要約で埋めてはならない** — 不明なら不明と書く

### 正しい例

```markdown
**User**: キズナ・ばの保守契約の件なんだけど、先方から質問が3つ来てる。
レンタルサーバーってうちが借りてるさくらの2つだけ？って聞かれてるのと、
メールサーバーって何？って聞かれてる。あと支払い期限も。

**Assistant**: 過去の調査資料を確認しました。さくらの2契約（①アプリケーションプラス名義 ②キズナば名義）のみです。メールサーバーはこの2契約の片方に同居しています...

**User**: あーそれでいい。送っといて
```

### 悪い例（これをやってはいけない）

```markdown
1. 先方から3点の質問を受領（レンタルサーバーの範囲、メールサーバーとは何か、支払い期限）
2. 過去の調査資料を読み取り、サーバー構成の全容を把握
3. 非技術者向けに平易な文面で回答を作成
```

## Append Mode

When `/conversation-log` is invoked and a file with the same date and topic name already exists in `chat-logs/`:

1. **Read the existing file** to find the end of the `## 会話の流れ` section.
2. **Append** a separator and the new conversation content:

```markdown
---
<!-- Continued: HH:MM -->

**User**: {continued conversation...}

**Assistant**: {continued response...}
```

3. **Update** the following sections to reflect the latest state:
   - `## 要約` — Rewrite to cover the full conversation (original + appended)
   - `## 主要な決定事項` — Merge new decisions with existing ones
   - `## 送付・作成した文面` — Append any new drafted messages (keep existing ones intact)
   - `## 次のアクション` — Update with current action items (mark completed items as done)

4. **Preserve** the original metadata table (日時 stays as the first recording time).

This ensures a single file per topic per day, with the full conversation history intact.

## Multiple Topics

If the conversation contains more than one qualifying topic, create a separate file for each topic and report all saved paths at the end.

## Customizing the Save Directory

By default, logs are saved to `chat-logs/` relative to the current working directory. To change this, update the path in this skill file or specify the target directory when invoking.
