# 英コミュ長文データ更新スキル

画像から英文・日本語訳・文法解説を文字起こしして `EIGO_SENTENCES` に追加し、
さらに英検3級以上の単語を抽出して `vocab/data.js` の `WORDS` にも追加する。

## 手順

1. `$ARGUMENTS` に画像パスがあれば Read ツールで読む。なければ「画像のパスを教えてください」と聞く。
2. 画像を読んで英文・日本語訳・文法解説を文ごとに抽出する（下記フォーマット参照）。
3. `/home/user/new/data.js` の末尾 `EIGO_SENTENCES` 配列に追加する。
4. 同じ英文から英検3級以上の単語を抽出し、`/home/user/new/vocab/data.js` の `WORDS` に追加する（下記フォーマット参照）。
5. `git add /home/user/new/data.js /home/user/new/vocab/data.js && git commit -m "..." && git push -u origin claude/create-matching-report-In93g`
6. 何文追加・何単語抽出したか報告する。

---

## EIGO_SENTENCES に追加する形式

**ファイル:** `/home/user/new/data.js` の末尾にある `const EIGO_SENTENCES = [...]`

```js
{ en: "An endangered species is a group of animals or plants.", ja: "絶滅危惧種とは、動物や植物の一群です。", grammar: "A is B「AはBです」。of animals or plants は「動物や植物の」。" },
```

- `en`: 画像の英文をそのまま1文ずつ
- `ja`: 対応する日本語訳
- `grammar`: その文の文法・構造の解説（画像にあればそのまま使う。なければ文の構造を簡潔に説明する）

---

## WORDS に追加する形式

**ファイル:** `/home/user/new/vocab/data.js` の `export const WORDS = [...]`

```js
{ id: 78, en: 'endangered', ja: '絶滅危惧の', level: 'pre2', ex: 'An endangered species needs our protection.', exja: '絶滅危惧種は私たちの保護を必要としています。' },
```

- `id`: 既存の最大IDの次から連番（vocab/data.js を読んで確認する）
- `en`: 英単語（原形）
- `ja`: 日本語の意味
- `level`: 英検レベル（`'3'` / `'pre2'` / `'2'` / `'pre1'`）
- `ex`: 英語の例文（英文テキストから引用するか、シンプルな例文を作る）
- `exja`: 例文の日本語訳

**抽出ルール:**
- 英検3級相当以上の単語のみ（be動詞・基本前置詞・数字など超基本語は除く）
- 同じ単語が複数文に出ても1回だけ追加
- すでに vocab/data.js にある単語は追加しない（重複チェック）
- レベルの目安:
  - `'3'` (3級): endangered, habitat, extinct, species など中学レベル応用語
  - `'pre2'` (準2級): destruction, pollution, penalty など高校基本語
  - `'2'` (2級): overfishing, sanctuary, biodiversity など高校発展語
  - `'pre1'` (準1級): それ以上の難度

---

## 注意

- `EIGO_SENTENCES` は `data.js` の末尾、`};` の後ろにある
- `WORDS` は `export const WORDS = [...]` の配列末尾に追加
- 既存データを消さない
