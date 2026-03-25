# conversation-log

非開発会話を自動検出し、構造化されたMarkdownログとして保存するClaude Codeプラグインです。

## なぜ必要か

Claude Codeのセッションは開発作業だけでなく、契約交渉、アカウント対応、技術相談、意思決定など、さまざまな会話にも使われます。しかし、セッションが終わると会話は消えてしまい、「あの時何を決めたっけ？」「何て送ったっけ？」が振り返れなくなります。

**conversation-log** は、非開発会話を検出して保存を促すことでこの問題を解決します。要約だけでなく、**会話の流れそのもの**を記録するので、後から経緯を追えます。

## インストール

Claude Codeプラグインとしてインストールします:

```bash
claude plugin add https://github.com/aliksir/conversation-log
```

または手動クローン:

```bash
git clone https://github.com/aliksir/conversation-log ~/.claude/plugins/conversation-log
```

インストール後、`rules/auto-detect-chat.md` が毎セッション自動ロードされ、`/conversation-log` コマンドが利用可能になります。

## 使い方

### 自動検出

プラグインが会話をバックグラウンドで監視します。非開発会話の条件（3往復以上・実質的内容あり・コード変更なし）を検出すると、自然な区切りで保存を提案します:

> "This looks like it could be worth saving — want me to log this conversation? I can run `/conversation-log` to save a structured summary."

### 手動実行

いつでもコマンドで保存できます:

```
/conversation-log
```

引数不要。現在の会話から非開発トピックを抽出し、トピックごとに1ファイル保存します。

## 保存フォーマット

ログは作業ディレクトリの `chat-logs/` に保存されます:

```
chat-logs/YYYYMMDD_{トピック名}.md
```

同日に複数保存する場合: `_2.md`, `_3.md` と連番が付きます。

各ファイルの構成:

```markdown
# 会話ログ: {トピック}

| 項目 | 内容 |
|------|------|
| 日時 | YYYY-MM-DD HH:MM |
| カテゴリ | business / support / consultation / decision / other |
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
conversation_log_dir: "C:/Users/yourname/chat-logs"
```

### 自動検出の無効化

自動検出を無効にし `/conversation-log` コマンドのみ使いたい場合は、`rules/auto-detect-chat.md` を削除またはリネームしてください。

## ライセンス

MIT License — 詳細は [LICENSE](LICENSE) を参照。
