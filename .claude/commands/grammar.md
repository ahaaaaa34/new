# 文法データ更新スキル

ユーザーが貼り付けた問題データを `/home/user/new/data.js` に追加・更新するスキルです。

## 受け取るデータ形式（柔軟に対応）

ユーザーは以下のような形式で問題を送ってくる。形式が多少違っていても解釈して変換すること。

```
FRAME 001
I ( ) in France for three years.
① have been  ② lived  ③ have once stayed  ④ went
答え: ②
訳: 私は子どものころ3年間フランスに住んでいた。
解説: when以下より過去の内容...
出典: 奥羽大
```

## data.js のデータ構造

`/home/user/new/data.js` に `const QUIZ_DATA = { frames, exA, exB, exC }` がある。

各セクションのオブジェクト形式：

**FRAME / Exercise A（type: 'choice'）**
```js
{
  id: 'f001',           // FRAME→f001〜, exA→a001〜
  type: 'choice',
  section: 'frames',    // または 'exA'
  sectionName: 'FRAME', // または 'Exercise A'
  label: 'FRAME 001',
  tagClass: 'tag-f',    // frames→tag-f, exA→tag-a
  question: '...',      // ( ) が空欄
  options: ['...', '...', '...', '...'],
  answer: 1,            // 0始まりのインデックス
  source: '大学名',
  translation: '...',
  explanation: '...'
}
```

**Exercise B（type: 'exB'）**
```js
{
  id: 'b001',
  type: 'exB',
  section: 'exB',
  sectionName: 'Exercise B',
  label: 'Exercise B 01',
  tagClass: 'tag-b',
  question: '誤りを含む文（①②③④の番号付き）',
  answer: 2,            // 誤りの番号（0始まり）
  correct: '正しい形',
  source: '大学名',
  translation: '...',
  explanation: '...'
}
```

**Exercise C（type: 'exC'）**
```js
{
  id: 'c001',
  type: 'exC',
  section: 'exC',
  sectionName: 'Exercise C',
  label: 'Exercise C 01',
  tagClass: 'tag-c',
  question: '[ ] に続く内容など問題文',
  context: 'フレーム文（あれば）',
  words: ['word1', 'word2', 'word3'],  // 並び替えさせる単語群
  answer: 'The correct sentence.',
  prefix: '',   // 答え文の前に固定される部分（あれば）
  suffix: '.',  // 末尾に固定される部分（あれば）
  source: '大学名',
  translation: '...',
  explanation: '...'
}
```

## やること

1. **ユーザーのデータを確認**  
   `$ARGUMENTS` にデータがあればそれを使う。なければ「問題データを貼り付けてください」と聞く。

2. **データを解析して変換**  
   どのセクション（FRAME / exA / exB / exC）か判断し、適切な形式に変換する。  
   IDは既存の最後のIDから連番を続ける（data.js を読んで確認する）。

3. **data.js を更新**  
   該当セクションの配列の末尾に追加する。複数問まとめて送られた場合は全部追加する。  
   既存データを消さないこと。

4. **ホーム画面の問題数表示を更新**  
   index.html の `.sec-sub` テキスト（例: "9問 · 時制の基本パターン"）にある問題数を更新する。

5. **コミット＆プッシュ**  
   `git add data.js index.html && git commit` してから `git push -u origin claude/create-matching-report-In93g`

6. **完了報告**  
   何問追加したか、どのセクションか報告する。

## 注意
- `const QUIZ_DATA` の外側は変えない（モジュール形式ではなく通常のconst）
- answer は0始まりのインデックス（①=0, ②=1, ③=2, ④=3）
- 既存データのIDと重複しないようにする
