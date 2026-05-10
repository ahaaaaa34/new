# 新しいアプリ作成スキル

現在のアプリの枠組みだけをコピーして、新しいGitHubリポジトリを作るスキル。
データ（問題文・英コミュの文・単語）は空の状態でスタートする。

## 手順

1. `$ARGUMENTS` にリポジトリ名があればそれを使う。なければ「新しいリポジトリ名を教えてください」と聞く。

2. `/tmp/<リポジトリ名>` に作業ディレクトリを作成する。

3. 現在のプロジェクトから以下をコピーする：
   - `index.html`
   - `app.js`
   - `sw.js`
   - `manifest.json`
   - `icon.svg`（あれば）
   - `vocab/index.html`
   - `vocab/app.js`
   - `vocab/tts.js`
   - `vocab/sw.js`
   - `vocab/manifest.json`
   - `.claude/commands/`（スキルごとコピー）

4. 以下のファイルは**空のテンプレート**で新規作成する：

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

5. `git init && git add -A && git commit -m "initial: app template"` を実行する。

6. GitHub に新しいリポジトリ `ahaaaaa34/<リポジトリ名>` を作成する（Public）。

7. `git remote add origin <新しいリポジトリのURL> && git push -u origin main` を実行する。

8. 完了したら新しいリポジトリのURLを報告する。
   次のステップ：新しいリポジトリをCloneしてClaude Codeで開き、/eigo・/grammar・/vocabでデータを追加する。
