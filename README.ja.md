# conversation-log

会話を自動検出し、構造化されたMarkdownログとして保存するClaude Codeスキルです。

## なぜ必要か

Claude Codeのセッションは開発作業だけでなく、契約交渉、アカウント対応、技術相談、意思決定など、さまざまな会話にも使われます。しかし、セッションが終わったりコンテキスト圧縮が発生すると会話は消えてしまい、「あの時何を決めたっけ？」「何て送ったっけ？」「なぜその設計にしたっけ？」が振り返れなくなります。

**conversation-log** は、**開発会話を含む全ての実質的な会話**を検出して保存を促すことでこの問題を解決します。要約だけでなく、**会話の流れそのもの**を記録するので、後から経緯を追えます。

## インストール

### クイックインストール

```bash
# 1. リポジトリをクローン
git clone https://github.com/aliksir/conversation-log /tmp/conversation-log

# 2. スキルをインストール
mkdir -p ~/.claude/skills/conversation-log
cp /tmp/conversation-log/skills/conversation-log.md ~/.claude/skills/conversation-log/SKILL.md

# 3. 自動検出ルールをインストール（任意・推奨）
mkdir -p ~/.claude/rules
cp /tmp/conversation-log/rules/auto-detect-chat.md ~/.claude/rules/auto-detect-chat.md

# 4. クリーンアップ
rm -rf /tmp/conversation-log
```

インストール後:
- `/conversation-log` スラッシュコマンドが全セッションで利用可能に
- ルールをインストールした場合、会話の自然な区切りで自動的に保存を提案

## 使い方

### 自動検出

自動検出ルールがインストールされていると、Claude が会話をバックグラウンドで監視します。実質的な会話（3往復以上）を検出すると、自然な区切りで保存を提案します:

> "会話が長くなってきたので、ここまでのログを保存しますか？ `/conversation-log` を実行します。"

また、以下の条件でも自動的に保存を提案します:

- **ボリュームトリガー**: 会話が約20往復に達した場合（コンテキスト圧縮によるロスト防止）
- **フェーズ切替トリガー**: 話題が変わった場合（例: 雑談 → 開発タスク）
- **セッション終了トリガー**: `/handover` やセッション終了前

### 手動実行

いつでもコマンドで保存できます:

```
/conversation-log
```

引数不要。現在の会話からトピックを抽出し、トピックごとに1ファイル保存します。

### 追記モード

同じセッション内で同じトピックについて `/conversation-log` を複数回実行した場合、既存ファイルに**追記**します。新しいファイルは作成されません。これにより、1トピック1ファイルの一覧性を保ちつつ、長いセッションでもログが途切れません。

### セッション終了時の自動実行

セッション終了前に自動で `/conversation-log` を実行するには、`CLAUDE.md` またはメモリに以下を追加します:

```yaml
session:
  end: ["/conversation-log", "/handover"]
  auto_approve: ["/conversation-log", "/handover"]
```

これにより、セッションのコンテキストが失われる前に会話が自動で保存されます。

## 保存フォーマット

ログは作業ディレクトリの `chat-logs/` に保存されます:

```
chat-logs/YYYYMMDD_{トピック名}.md
```

同日の異なるトピックは別ファイル: `_2.md`, `_3.md` と連番が付きます。

各ファイルの構成:

```markdown
# 会話ログ: {トピック}

| 項目 | 内容 |
|------|------|
| 日時 | YYYY-MM-DD HH:MM |
| カテゴリ | business / support / consultation / decision / development / other |
| 関連先 | 会社名・サービス名・人名 |

## 要約
3〜5行の概要。パッと見で何の話か分かるように。

## 会話の流れ
**User**: ユーザーの発言・質問・提供した情報
**Assistant**: 回答・提案・作成した内容
**User**: 返答・判断・追加情報
...
（要約ではなく、実際のやり取りを時系列で再現）

## 主要な決定事項
- 決定・合意した内容

## 送付・作成した文面
> 会話中に作成したメール・申請文・返信文（なければ省略）

## 次のアクション
- [ ] フォローアップTODO
```

## カテゴリ

| カテゴリ | 用途 |
|---------|------|
| `business` | 契約・請求・交渉・取引先対応 |
| `support` | アカウント対応・申請・問い合わせ |
| `consultation` | 技術相談・方針相談・アーキテクチャ検討 |
| `decision` | 意思決定・選択肢の評価・方向性の確定 |
| `development` | 機能実装・バグ修正・コードレビュー・リファクタリング・インフラ変更 |
| `other` | 上記に当てはまらない実質的な会話 |

## 設定

### 保存先ディレクトリの変更

デフォルトの保存先は作業ディレクトリ直下の `chat-logs/` です。

変更するには、`skills/conversation-log.md` 内のパスを編集します:

```markdown
# 変更前（デフォルト）
chat-logs/YYYYMMDD_{topic-name}.md

# 変更例: ホームディレクトリ配下
~/.claude/chat-logs/YYYYMMDD_{topic-name}.md

# 変更例: プロジェクト内の特定フォルダ
docs/conversations/YYYYMMDD_{topic-name}.md
```

または、プロジェクトの `CLAUDE.md` に以下を追記して上書きできます:

```markdown
## conversation-log 設定
conversation_log_dir: "~/my-chat-logs"
```

### アンインストール

```bash
# スキルを削除
rm -rf ~/.claude/skills/conversation-log

# 自動検出ルールを削除
rm ~/.claude/rules/auto-detect-chat.md
```

### 自動検出のみ無効化

自動検出を無効にし `/conversation-log` コマンドのみ使いたい場合:

```bash
rm ~/.claude/rules/auto-detect-chat.md
```

## 言語

- ログ出力は日本語のセクション見出し（会話ログ、要約 等）を使用
- [English README](README.md) も利用可能

## ライセンス

GPL-3.0 — 詳細は [LICENSE](LICENSE) を参照。
