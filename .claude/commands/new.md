# 新しいアプリ作成スキル

現在のアプリの枠組みをコピーして、新しいGitHubリポジトリに新バージョンを作るスキル。
git不要。curl + GitHub APIだけで完結する。
データ（問題文・英コミュの文・単語）は空の状態でスタートする。

## 前提
- `GITHUB_TOKEN` 環境変数が設定されていること（~/.claude/settings.json の env に記載）

## 手順

1. `$ARGUMENTS` にリポジトリ名があればそれを使う。なければ「新しいリポジトリ名を教えてください」と聞く。

2. GITHUB_TOKENを確認する：
   ```bash
   echo $GITHUB_TOKEN
   ```
   空なら「~/.claude/settings.json に GITHUB_TOKEN を設定してください」と伝えて終了。

3. GitHubリポジトリを作成する：
   ```bash
   curl -s -X POST https://api.github.com/user/repos \
     -H "Authorization: token $GITHUB_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"name":"<リポジトリ名>","private":false}'
   ```

4. 現在のプロジェクトのルートを特定する（`git rev-parse --show-toplevel` または現在のディレクトリ）。

5. 以下のファイルを Read ツールで読み、base64エンコードしてGitHub APIでアップロードする。
   アップロードは1ファイルずつ以下のコマンドで行う：
   ```bash
   CONTENT=$(base64 -w 0 <ファイルパス> 2>/dev/null || base64 <ファイルパス>)
   curl -s -X PUT https://api.github.com/repos/ahaaaaa34/<リポジトリ名>/contents/<パス> \
     -H "Authorization: token $GITHUB_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"message\":\"initial: app template\",\"content\":\"$CONTENT\"}"
   ```

   アップロードするファイル：
   - `index.html` → `index.html`
   - `app.js` → `app.js`
   - `sw.js` → `sw.js`
   - `manifest.json` → `manifest.json`
   - `tts.js` → `tts.js`
   - `icon.svg` → `icon.svg`（存在すれば）
   - `vocab/index.html` → `vocab/index.html`
   - `vocab/app.js` → `vocab/app.js`
   - `vocab/sw.js` → `vocab/sw.js`
   - `vocab/manifest.json` → `vocab/manifest.json`
   - `vocab/tts.js` → `vocab/tts.js`
   - `vocab/icon.svg` → `vocab/icon.svg`（存在すれば）
   - `.claude/commands/vocab.md` → `.claude/commands/vocab.md`
   - `.claude/commands/grammar.md` → `.claude/commands/grammar.md`
   - `.claude/commands/eigo.md` → `.claude/commands/eigo.md`
   - `.claude/commands/new.md` → `.claude/commands/new.md`

6. `data.js` と `vocab/data.js` は以下の空テンプレートをbase64エンコードしてアップロードする：

   **data.js:**
   ```
   const QUIZ_DATA = {
     frames: [],
     exA: [],
     exB: [],
     exC: []
   };

   const EIGO_SENTENCES = [];
   ```

   **vocab/data.js:**
   ```
   export const WORDS = [];
   ```

   ```bash
   CONTENT=$(echo '<上記の内容>' | base64 -w 0 2>/dev/null || echo '<上記の内容>' | base64)
   curl -s -X PUT https://api.github.com/repos/ahaaaaa34/<リポジトリ名>/contents/data.js \
     -H "Authorization: token $GITHUB_TOKEN" \
     -H "Content-Type: application/json" \
     -d "{\"message\":\"initial: app template\",\"content\":\"$CONTENT\"}"
   ```

7. 完了したら `https://github.com/ahaaaaa34/<リポジトリ名>` を報告する。
   次のステップ：新しいリポジトリをCloneしてClaude Codeで開き、/eigo・/grammar・/vocabでデータを追加する。
