# Auto-Detect Non-Development Conversations

Automatically detect conversations that contain substantive non-development content and prompt the user to save them as structured logs.

## Detection Criteria

A conversation qualifies for logging when **all** of the following conditions are met:

1. **Not part of a development task** — The topic is not a coding task, bug fix, feature implementation, or any work that involves modifying source code. If the session includes both development work and non-development conversation (e.g., a contract discussion that happens between coding tasks), only the non-development portions qualify
2. **No code-modification tools used for this topic** — The specific conversation thread does not involve Edit, Write, NotebookEdit, or shell commands that change project files. Note: writing the conversation log file itself does not count as code modification
3. **Substantive content** — The exchange contains meaningful information (decisions, plans, contact details, instructions, or agreements), not just pleasantries
4. **At least 3 exchanges** — A minimum of 3 back-and-forth turns have occurred on the topic

## Category Classification

Classify the conversation into one of the following categories:

| Category | Examples |
|----------|---------|
| `business` | Contracts, billing, negotiations, vendor relations, procurement |
| `support` | Account issues, application procedures, service requests, troubleshooting |
| `consultation` | Technical advice, strategy discussions, architecture guidance, planning |
| `decision` | Choices made, options evaluated, direction established |
| `other` | Substantive exchanges that don't fit the above |

## When to Prompt

Prompt the user to save a conversation log at natural break points:

- When a topic appears to be wrapping up
- When the conversation shifts to a clearly different subject
- At the end of a session that contained qualifying content

**Prompt phrasing** (adapt naturally to context):

> "This looks like it could be worth saving — want me to log this conversation? I can run `/conversation-log` to save a structured summary."

## Exclusion Conditions

Do **not** prompt for logging when:

- The exchange consists only of greetings, thanks, or one-liners
- Fewer than 3 substantive turns have occurred on the topic
- The topic is part of an active development workflow (implementation, debugging, code review, testing) — even if no code has been modified *yet*, planning or designing a code change counts as development
- The conversation is about how to use Claude Code itself (tool usage, configuration, troubleshooting)
- The user has already declined to log in the same session

## Notes

- Do not log automatically without user confirmation — always prompt first
- One log file per distinct topic; if multiple topics qualify, save them separately
- The `/conversation-log` skill handles the actual file creation
