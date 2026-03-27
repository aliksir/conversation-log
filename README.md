# conversation-log

A Claude Code skill that automatically detects conversations and saves them as structured Markdown log files.

## Why This Exists

Claude Code sessions are used for all kinds of work — development, business discussions, strategy consultations, support follow-ups, and more. Those exchanges vanish when the session ends or when context compression kicks in during long sessions, leaving no record of decisions made, actions agreed upon, or messages drafted.

**conversation-log** solves this by detecting substantive conversations and prompting you to save them before they disappear. It covers **all** conversation types — development discussions, business calls, and everything in between.

## Installation

### Quick Install

```bash
# 1. Clone the repository
git clone https://github.com/aliksir/conversation-log /tmp/conversation-log

# 2. Install the skill
mkdir -p ~/.claude/skills/conversation-log
cp /tmp/conversation-log/skills/conversation-log.md ~/.claude/skills/conversation-log/SKILL.md

# 3. Install the auto-detection rule (optional but recommended)
mkdir -p ~/.claude/rules
cp /tmp/conversation-log/rules/auto-detect-chat.md ~/.claude/rules/auto-detect-chat.md

# 4. Clean up
rm -rf /tmp/conversation-log
```

After installation:
- The `/conversation-log` slash command becomes available in all sessions
- If the rule is installed, Claude will automatically suggest saving conversations at natural break points

## Usage

### Automatic Detection

When the auto-detection rule is installed, Claude monitors your conversation in the background. When it detects a qualifying exchange (3+ turns, substantive content), it will suggest saving a log at a natural break point:

> "会話が長くなってきたので、ここまでのログを保存しますか？ `/conversation-log` を実行します。"

It also proactively suggests saving when:

- **Volume trigger**: The conversation reaches ~20 round-trips (to prevent context compression loss)
- **Phase transition**: The conversation shifts topics (e.g., chat → development)
- **Session end**: Before `/handover` or session wrap-up

### Manual Invocation

You can also trigger logging at any time:

```
/conversation-log
```

No arguments needed. The skill scans the current conversation, identifies topics, and writes one Markdown file per topic.

### Append Mode

If you run `/conversation-log` multiple times in the same session on the same topic, it **appends** to the existing file rather than creating a new one. This ensures a single continuous log per topic per day, even across multiple save points.

### Session-End Auto-Run

To automatically run `/conversation-log` before your session handover, add this to your `CLAUDE.md` or memory:

```yaml
session:
  end: ["/conversation-log", "/handover"]
  auto_approve: ["/conversation-log", "/handover"]
```

This ensures conversations are captured before the session context is lost.

## Saved File Format

Logs are saved to `chat-logs/` in your working directory using the naming pattern:

```
chat-logs/YYYYMMDD_{topic-name}.md
```

Multiple distinct topics on the same day get separate files: `_2.md`, `_3.md`, etc.

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

## 会話の流れ
**User**: What the user said, asked, or provided
**Assistant**: What was proposed, drafted, or answered
**User**: Follow-up, decisions, additional context
...
(Full chronological record — not summarized)

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
| `development` | Feature implementation, bug fixes, code reviews, refactoring, infrastructure changes |
| `other` | Substantive exchanges that don't fit the above |

## Configuration

### Changing the Save Directory

The default save location is `chat-logs/` relative to your working directory. To change it, edit the path in `skills/conversation-log.md`:

```markdown
# Default
chat-logs/YYYYMMDD_{topic-name}.md

# Example: save to home directory
~/.claude/chat-logs/YYYYMMDD_{topic-name}.md

# Example: project subfolder
docs/conversations/YYYYMMDD_{topic-name}.md
```

Alternatively, add this to your project's `CLAUDE.md` to override per-project:

```markdown
## conversation-log settings
conversation_log_dir: "~/my-chat-logs"
```

### Uninstalling

```bash
# Remove the skill
rm -rf ~/.claude/skills/conversation-log

# Remove the auto-detection rule
rm ~/.claude/rules/auto-detect-chat.md
```

### Disabling Auto-Detection Only

To turn off automatic prompts while keeping the `/conversation-log` command available, remove the rule file:

```bash
rm ~/.claude/rules/auto-detect-chat.md
```

## Language

- Log output format uses Japanese section headers (会話ログ, 要約, etc.)
- [Japanese README](README.ja.md) is also available

## License

GPL-3.0 — see [LICENSE](LICENSE) for details.
