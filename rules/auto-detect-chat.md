# Auto-Detect Conversations for Logging

Automatically detect conversations that contain substantive content and prompt the user to save them as structured logs.

## Detection Criteria

A conversation qualifies for logging when **all** of the following conditions are met:

1. **Substantive content** — The exchange contains meaningful information (decisions, plans, contact details, instructions, agreements, implementation discussions, or design rationale), not just pleasantries
2. **At least 3 exchanges** — A minimum of 3 back-and-forth turns have occurred on the topic

## Category Classification

Classify the conversation into one of the following categories:

| Category | Examples |
|----------|---------|
| `business` | Contracts, billing, negotiations, vendor relations, procurement |
| `support` | Account issues, application procedures, service requests, troubleshooting |
| `consultation` | Technical advice, strategy discussions, architecture guidance, planning |
| `decision` | Choices made, options evaluated, direction established |
| `development` | Feature implementation, bug fixes, code reviews, refactoring, infrastructure changes |
| `other` | Substantive exchanges that don't fit the above |

## Auto-Trigger Conditions

Proactively suggest running `/conversation-log` when any of the following conditions are met:

### 1. Volume Trigger (20 round-trips)

When the conversation reaches approximately 20 back-and-forth exchanges, suggest saving the log to prevent context compression from losing earlier content.

> "会話が長くなってきたので、ここまでのログを保存しますか？ `/conversation-log` を実行します。"

### 2. Phase Transition Trigger

When the conversation shifts between distinct phases (e.g., casual chat → development task, or development → consultation), suggest saving the preceding phase before continuing.

> "話題が変わるので、ここまでの会話を保存しますか？"

### 3. Session End Trigger

At the end of a session (when `/handover` or session wrap-up is initiated), prompt to save any unsaved conversation content.

## When to Prompt

Prompt the user to save a conversation log at natural break points:

- When a topic appears to be wrapping up
- When the conversation shifts to a clearly different subject
- When any auto-trigger condition is met
- At the end of a session that contained qualifying content

## Exclusion Conditions

Do **not** prompt for logging when:

- The exchange consists only of greetings, thanks, or one-liners
- Fewer than 3 substantive turns have occurred on the topic
- The user has already declined to log in the same session
- The conversation has already been saved and no new substantive content has been added since

## Notes

- Do not log automatically without user confirmation — always prompt first
- One log file per distinct topic; if multiple topics qualify, save them separately
- The `/conversation-log` skill handles the actual file creation
- When appending to an existing log, only the new (unsaved) portion of the conversation should be added
