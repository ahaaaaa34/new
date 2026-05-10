# 新しいアプリ作成スキル

現在のアプリの枠組みをコピーして、新しいGitHubリポジトリに新バージョンを作るスキル。
データ（問題文・英コミュの文・単語）は空の状態でスタートする。

## 手順

1. `$ARGUMENTS` にリポジトリ名があればそれを使う。なければ「新しいリポジトリ名を教えてください」と聞く。

2. 現在のプロジェクトから以下のファイルを Read ツールで読む：
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

3. `mcp__github__create_repository` で新しいリポジトリ `<リポジトリ名>` を作成する（Public、autoInit: false）。

4. `mcp__github__push_files` で以下をまとめて1コミットでpushする：
   - 手順2で読んだファイルをそのままの内容で
   - `data.js` は以下の空テンプレートで：
     ```js
     const QUIZ_DATA = {
       frames: [],
       exA: [],
       exB: [],
       exC: []
     };

     const EIGO_SENTENCES = [];
     ```
   - `vocab/data.js` は以下の空テンプレートで：
     ```js
     export const WORDS = [];
     ```
   - owner: `ahaaaaa34`、branch: `main`、message: `initial: app template`

5. 完了したら新しいリポジトリのURLを報告する。
   次のステップ：新しいリポジトリをCloneしてClaude Codeで開き、/eigo・/grammar・/vocabでデータを追加する。
