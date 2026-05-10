# 文法データ更新スキル

問題と回答が書かれた画像から問題を抽出して `/home/user/new/data.js` の `QUIZ_DATA` に追加する。

## 手順

1. `$ARGUMENTS` に画像パスがあれば Read ツールで読む。なければ「画像のパスを教えてください」と聞く。
2. 画像を読んで問題タイプ（FRAME / ExA / ExB / ExC）を判断し、全問を抽出する。
3. `/home/user/new/data.js` を読んで各セクションの最後のIDを確認する。
4. 下記の正確な形式に変換して該当セクションの配列末尾に追加する。
5. `git add /home/user/new/data.js && git commit -m "..." && git push -u origin claude/create-matching-report-In93g`
6. 何問追加したか報告する。

---

## FRAME / Exercise A（選択問題）の形式

```js
{
  id: 'f010', type: 'choice', section: 'frames', sectionName: 'FRAME',
  label: 'FRAME 010', tagClass: 'tag-f',
  question: 'She ( ) the flower arrangement club.',
  options: ['belongs', 'belongs to', 'is belonging', 'is belonging to'],
  answer: 1, source: '共立女子大',
  translation: '彼女は華道部に所属している。',
  explanation: 'belongは状態動詞なので進行形にできない...'
},
```

- `id`: FRAME → `f` + 3桁ゼロ埋め。ExA → `a` + 2桁ゼロ埋め
- `label`: FRAME → `'FRAME 010'`、ExA → `'Ex. A 22'`
- `tagClass`: FRAME → `'tag-f'`、ExA → `'tag-a'`
- `options`: 選択肢テキストのみ（①②の番号は含めない）
- `answer`: 0始まりインデックス（①=0, ②=1, ③=2, ④=3）

---

## Exercise B（誤文訂正）の形式

```js
{
  id: 'b07', type: 'exB', section: 'exB', sectionName: 'Exercise B',
  label: 'Ex. B 07', tagClass: 'tag-b',
  question: 'We ①will notify you ②as soon as we ③will have further ④information.',
  options: ['①', '②', '③', '④'],
  answer: 2, source: '学習院大',
  correction: '③ will have → have',
  corrected: 'We will notify you as soon as we have further information.',
  translation: '私たちがさらに情報を得たらすぐにあなたにお知らせします。',
  explanation: 'as soon as節は時の副詞節なので未来でも現在形を使う...'
},
```

- `id`: `b` + 2桁ゼロ埋め
- `question`: 文中に `①②③④` を埋め込む
- `options`: 常に `['①', '②', '③', '④']` 固定
- `answer`: 誤り箇所の0始まりインデックス（①=0, ②=1, ③=2, ④=3）
- `correction`: `'③ will have → have'` 形式
- `corrected`: 全文修正後の文

---

## Exercise C（整序英作文）の形式

```js
{
  id: 'c08', type: 'exC', section: 'exC', sectionName: 'Exercise C',
  label: 'Ex. C 08', tagClass: 'tag-c',
  japanese: '昨年以来，ビタミン剤を飲んでいる。',
  prefix: '', suffix: 'year.',
  words: ['been', 'have', 'I', 'last', 'since', 'supplements', 'taking', 'vitamin'],
  note: null, source: '金沢工業大',
  answer: 'I have been taking vitamin supplements since last year.',
  explanation: 'since last yearで継続を表し現在完了進行形にする...'
},
```

- `id`: `c` + 2桁ゼロ埋め
- `japanese`: 日本語問題文（なければ `null`）
- `prefix`: 文頭の固定部分（なければ `''`）
- `suffix`: 文末の固定部分（なければ `''`）
- `words`: 並び替える語・句の配列（prefix/suffixの部分は含めない）
- `note`: 不要語がある場合 `'gotとhasが不要'` 形式、なければ `null`
- `answer`: prefix + 並び替え結果 + suffix の完全な正解文

---

## 注意

- `QUIZ_DATA = { frames:[...], exA:[...], exB:[...], exC:[...] }` の構造は変えない
- 既存データのIDと重複しないこと
- 既存データを消さないこと
