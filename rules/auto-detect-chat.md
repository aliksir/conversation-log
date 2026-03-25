# Auto-Detect Non-Development Conversations

Automatically detect conversations that contain substantive non-development content and prompt the user to save them as structured logs.

## Detection Criteria

A conversation qualifies for logging when **all** of the following conditions are met:

1. **No code-modification tools used** — No file edits, code writes, or shell commands that change project state
2. **Substantive content** — The exchange contains meaningful information (decisions, plans, contact details, instructions, or agreements), not just pleasantries
3. **At least 3 exchanges** — A minimum of 3 back-and-forth turns have occurred on the topic

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
- The session has been primarily a development task (file edits, implementation, debugging)
- The user has already declined to log in the same session

## Notes

- Do not log automatically without user confirmation — always prompt first
- One log file per distinct topic; if multiple topics qualify, save them separately
- The `/conversation-log` skill handles the actual file creation
