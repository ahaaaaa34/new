# 新しいアプリ作成スキル

現在のアプリの枠組みをコピーして、新しいGitHubリポジトリに新バージョンを作るスキル。
データ（問題文・英コミュの文・単語）は空の状態でスタートする。

## 前提
- `GITHUB_TOKEN` 環境変数が設定されていること（~/.claude/settings.json の env に記載）
- `git` がインストールされていること

## 手順

1. `$ARGUMENTS` にリポジトリ名があればそれを使う。なければ「新しいリポジトリ名を教えてください」と聞く。

2. 現在のプロジェクトのルートを特定する（`git rev-parse --show-toplevel`）。

3. `/tmp/<リポジトリ名>` に作業ディレクトリを作成する。

4. 現在のプロジェクトから以下をコピーする：
   - `index.html`
   - `app.js`
   - `sw.js`
   - `manifest.json`
   - `tts.js`
   - `icon.svg`（あれば）
   - `vocab/index.html`
   - `vocab/app.js`
   - `vocab/sw.js`
   - `vocab/manifest.json`
   - `vocab/tts.js`
   - `vocab/icon.svg`（あれば）
   - `.claude/commands/vocab.md`
   - `.claude/commands/grammar.md`
   - `.claude/commands/eigo.md`
   - `.claude/commands/new.md`

5. 以下のファイルは**空のテンプレート**で新規作成する：

   **data.js:**
   ```js
   const QUIZ_DATA = {
     frames: [],
     exA: [],
     exB: [],
     exC: []
   };

   const EIGO_SENTENCES = [];
   ```

   **vocab/data.js:**
   ```js
   export const WORDS = [];
   ```

6. GitHub APIでリポジトリを作成する：
   ```bash
   curl -s -X POST https://api.github.com/user/repos \
     -H "Authorization: token $GITHUB_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"name":"<リポジトリ名>","private":false}'
   ```

7. git操作でpushする：
   ```bash
   cd /tmp/<リポジトリ名>
   git init
   git add -A
   git commit -m "initial: app template"
   git remote add origin https://$GITHUB_TOKEN@github.com/ahaaaaa34/<リポジトリ名>.git
   git push -u origin main
   ```

8. 完了したら `https://github.com/ahaaaaa34/<リポジトリ名>` を報告する。
   次のステップ：新しいリポジトリをCloneしてClaude Codeで開き、/eigo・/grammar・/vocabでデータを追加する。
