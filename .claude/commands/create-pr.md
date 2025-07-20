# PR作成コマンド

このコマンドは、既存のPRテンプレート（`.github/PULL_REQUEST_TEMPLATE.md`）に基づいてPRを作成します。

## 使用方法

```bash
gh pr create --title "タイトル" --body "$(cat <<'EOF'
## What

この PR が何のための PR なのか記載してください

## How

具体的な作業内容を記載してください

## Why ( option )

複数の選択があってあえてこの実装を選んだ場合などに理由を記載してください

🤖 Generated with [Claude Code](https://claude.ai/code)
EOF
)"
```

## 具体例

### 機能追加の場合
```bash
gh pr create --title "feat: ユーザー認証機能を追加" --body "$(cat <<'EOF'
## What

ユーザーがアカウントを作成し、ログイン・ログアウトできる認証機能を追加

## How

- Deviseを使用して認証機能を実装
- ユーザーモデルを作成
- ログイン/サインアップ画面を追加
- セッション管理を実装

## Why ( option )

Deviseを選択した理由：
- Rails標準の認証gemで実績が豊富
- セキュリティ面で信頼性が高い
- カスタマイズが容易

🤖 Generated with [Claude Code](https://claude.ai/code)
EOF
)"
```

### バグ修正の場合
```bash
gh pr create --title "fix: ログイン時のリダイレクトエラーを修正" --body "$(cat <<'EOF'
## What

ログイン後に正しいページにリダイレクトされない問題を修正

## How

- `after_sign_in_path_for`メソッドを修正
- セッションに保存されていたURLを正しく取得するように変更
- テストケースを追加

🤖 Generated with [Claude Code](https://claude.ai/code)
EOF
)"
```

### リファクタリングの場合
```bash
gh pr create --title "refactor: コントローラーのコードを整理" --body "$(cat <<'EOF'
## What

UsersControllerの重複コードを削除し、可読性を向上

## How

- 共通処理をbefore_actionに移動
- プライベートメソッドで処理を分割
- 不要なコードを削除

## Why ( option )

- DRY原則に従うため
- 今後の機能追加時のメンテナンス性を向上させるため

🤖 Generated with [Claude Code](https://claude.ai/code)
EOF
)"
```

## 便利なエイリアス設定

以下を`.bashrc`や`.zshrc`に追加すると、より簡単にPRを作成できます：

```bash
# Claude PR作成関数
claude-pr() {
    if [ $# -lt 3 ]; then
        echo "使用方法: claude-pr \"タイトル\" \"What\" \"How\" [\"Why\"]"
        return 1
    fi
    
    local title="$1"
    local what="$2"
    local how="$3"
    local why="${4:-}"
    
    local body="## What

$what

## How

$how"
    
    if [ -n "$why" ]; then
        body="$body

## Why ( option )

$why"
    fi
    
    body="$body

🤖 Generated with [Claude Code](https://claude.ai/code)"
    
    gh pr create --title "$title" --body "$body"
}
```

使用例：
```bash
claude-pr "feat: 検索機能を追加" \
  "ユーザーが投稿を検索できる機能を追加" \
  "- 検索フォームを追加\n- 検索ロジックを実装\n- 検索結果ページを作成" \
  "全文検索ではなくLIKE検索を選択（実装の簡単さを優先）"
```