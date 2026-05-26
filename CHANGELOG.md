# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2026-05-25

### Added
- YAML frontmatter (`name` / `description`) to `skills/conversation-log.md` so the skill is correctly discovered by Claude Code's plugin loader.
- This `CHANGELOG.md`.

### Changed
- `README.md` / `README.ja.md`: aligned the category-description structure between the English and Japanese versions.

## [1.0.0] - 2026-03-25

### Initial Release
- `/conversation-log` skill: save the current conversation as structured Markdown log files (`chat-logs/YYYYMMDD_{topic}.md`).
- Auto-detection rule (`rules/auto-detect-chat.md`) for substantive conversations, with criteria tuned to exclude dev-adjacent chatter.
- Category classification: `business` / `support` / `consultation` / `decision` / `development` / `other`.
- Conversation-flow section enforcing raw conversation format.
- Bilingual documentation (`README.md` / `README.ja.md`).
- Licensed under GPL-3.0-only.
