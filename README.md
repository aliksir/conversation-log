# conversation-log

A Claude Code plugin that automatically detects non-development conversations and saves them as structured Markdown log files.

## Why This Exists

Claude Code sessions are great for development work — but they are also used for business calls, support follow-ups, strategy consultations, and other substantive conversations. Those exchanges vanish when the session ends, leaving no record of decisions made, actions agreed upon, or messages drafted.

**conversation-log** solves this by detecting when a conversation contains non-development content and prompting you to save it before it disappears.

## Installation

Install as a Claude Code plugin by adding this repository to your plugin sources:

```bash
# From the Claude Code plugin directory
claude plugin add https://github.com/aliksir/conversation-log
```

Or clone it manually and reference the local path:

```bash
git clone https://github.com/aliksir/conversation-log ~/.claude/plugins/conversation-log
```

Once installed, the `rules/auto-detect-chat.md` rule loads automatically into every session, and the `/conversation-log` skill becomes available as a slash command.

## Usage

### Automatic Detection

The plugin monitors your conversation in the background. When it detects a qualifying non-development exchange (3+ turns, substantive content, no code changes), it will suggest saving a log at a natural break point:

> "This looks like it could be worth saving — want me to log this conversation? I can run `/conversation-log` to save a structured summary."

### Manual Invocation

You can also trigger logging at any time:

```
/conversation-log
```

No arguments needed. The skill scans the current conversation, identifies non-development topics, and writes one Markdown file per topic.

## Saved File Format

Logs are saved to `chat-logs/` in your working directory using the naming pattern:

```
chat-logs/YYYYMMDD_{topic-name}.md
```

Multiple files on the same day are numbered: `_2.md`, `_3.md`, etc.

Each file contains:

```markdown
# 会話ログ: {トピック}

| 項目 | 内容 |
|------|------|
| 日時 | YYYY-MM-DD HH:MM |
| カテゴリ | {category} |
| 関連先 | {company / service / person} |

## 要約
3–5 line summary of what was discussed.

## 主要な決定事項
- Key decisions or agreements

## 送付・作成した文面
> Any messages or documents drafted during the conversation (omitted if none)

## 次のアクション
- [ ] Follow-up tasks
```

## Categories

| Category | When to use |
|----------|-------------|
| `business` | Contracts, billing, negotiations, vendor or partner relations |
| `support` | Account issues, application procedures, service requests |
| `consultation` | Technical advice, strategy discussions, architecture guidance |
| `decision` | Choices made, options evaluated, direction established |
| `other` | Substantive exchanges that don't fit the above |

## Configuration

### Changing the Save Directory

The default save location is `chat-logs/` relative to your working directory. To change it, edit the path in `skills/conversation-log.md`.

### Disabling Auto-Detection

To turn off automatic prompts while keeping the `/conversation-log` command available, remove or rename `rules/auto-detect-chat.md`.

## License

MIT License — see [LICENSE](LICENSE) for details.
