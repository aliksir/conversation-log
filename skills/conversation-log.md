# Skill: /conversation-log

Save the current conversation as a structured Markdown log file.

## Invocation

```
/conversation-log
```

No arguments required. When called, extract non-development topics from the current conversation and save them as log files.

## Behavior

1. **Identify topics** — Scan the current conversation for substantive non-development exchanges. If multiple distinct topics exist, handle each separately.

2. **Determine save path** — Use the format `chat-logs/YYYYMMDD_{topic-name}.md` where:
   - `YYYYMMDD` is today's date (e.g., `20260325`)
   - `{topic-name}` is a short, lowercase, hyphen-separated description (e.g., `aws-contract-renewal`)
   - If a file with that name already exists today, append `_2`, `_3`, etc.

3. **Create the `chat-logs/` directory** if it does not exist.

4. **Write the log file** using the format below.

5. **Report** the saved file path: `ログを保存しました: chat-logs/YYYYMMDD_{topic-name}.md`

## Log File Format

```markdown
# 会話ログ: {トピック}

| 項目 | 内容 |
|------|------|
| 日時 | YYYY-MM-DD HH:MM |
| カテゴリ | {business/support/consultation/decision/other} |
| 関連先 | {会社名・サービス名・人名 etc.} |

## 要約
{3–5行の要約。何について話し、何が明らかになったかを簡潔に記述}

## 会話の流れ
{会話の実際のやり取りを時系列で記録する。誰が何を言ったかを再現できるレベルで書く}

**User**: {ユーザーの発言・質問・提供した情報}

**Assistant**: {アシスタントの回答・提案・作成した内容}

**User**: {ユーザーの返答・追加情報・判断}

...

{長い会話の場合でも省略しない。会話の流れが追えることが最優先}

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
| カテゴリ | One of: `business`, `support`, `consultation`, `decision`, `other` |
| 関連先 | Company name, service name, person name, or "不明" if unknown |
| 要約 | 3–5 lines; factual and concise — quick overview for scanning |
| 会話の流れ | Chronological record of the actual exchange. Reproduce what was said by whom. Do NOT summarize here — this section preserves the conversation as it happened. Include key context the user provided (pasted emails, screenshots descriptions, etc.) and the assistant's responses/proposals. For long conversations, keep all substantive exchanges; only omit pure filler ("ok", "thanks") if they add no context |
| 主要な決定事項 | Bullet list; omit if no decisions were made |
| 送付・作成した文面 | Block-quote any drafted messages/emails in full; omit section entirely if none |
| 次のアクション | Checkbox list of follow-up tasks; omit if none |

## Multiple Topics

If the conversation contains more than one qualifying topic, create a separate file for each topic and report all saved paths at the end.

## Customizing the Save Directory

By default, logs are saved to `chat-logs/` relative to the current working directory. To change this, update the path in this skill file or specify the target directory when invoking.
