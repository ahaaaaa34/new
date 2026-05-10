# 単語データ更新スキル

ユーザーが送った単語データを `/home/user/new/vocab/data.js` に追加・更新するスキルです。

## 受け取るデータ形式（柔軟に対応）

ユーザーは以下のような形式で単語を送ってくる。形式が違っていても解釈して変換すること。

```
species - 種、種類 - 3級
例文: There are many species of birds in this forest.
訳: この森にはたくさんの種類の鳥がいます。

ancient - 古代の - 準2級
例文: We visited an ancient temple in Kyoto.
訳: 私たちは京都の古代の寺を訪れました。
```

例文・訳がない場合は省略して `ex: ''`, `exja: ''` とする（後で埋められる）。

## vocab/data.js のデータ構造

`/home/user/new/vocab/data.js` に `export const WORDS = [...]` がある。

各単語のオブジェクト形式：
```js
{
  id: 1,               // 既存の最大IDから連番
  en: 'species',       // 英単語
  ja: '種、種類',      // 日本語の意味
  level: '3',          // '3' / 'pre2' / '2' / 'pre1'
  ex: 'There are many species of birds.', // 例文（英語、単語をそのまま使う）
  exja: 'この森には多くの種類の鳥がいる。'  // 例文の日本語訳
}
```

level の対応：
- 3級 → `'3'`
- 準2級 → `'pre2'`
- 2級 → `'2'`
- 準1級 → `'pre1'`

## やること

1. **ユーザーのデータを確認**  
   `$ARGUMENTS` にデータがあればそれを使う。なければ「単語データを貼り付けてください」と聞く。

2. **データを解析して変換**  
   英単語・日本語・レベル・例文・例文訳を抽出して変換する。  
   例文がなければ空文字列にする。  
   IDは既存の最大IDから連番を続ける（vocab/data.js を読んで確認する）。

3. **vocab/data.js を更新**  
   WORDS配列の末尾に追加する。複数単語まとめて送られた場合は全部追加する。  
   既存データを消さないこと。

4. **vocab/index.html の WORDS 数は動的に更新される**  
   （app.js が起動時に自動で `${WORDS.length} WORDS` に書き換えるので、HTMLは変更不要）

5. **コミット＆プッシュ**  
   `git add vocab/data.js && git commit` してから `git push -u origin claude/create-matching-report-In93g`

6. **完了報告**  
   何単語追加したか、レベル別の内訳も報告する。

## 注意
- `export const WORDS` の形式を維持する（ESモジュール形式）
- 既存データのIDと重複しないようにする
- 全部置き換えたい場合は「全部置き換えて」とユーザーが言った時だけ行う
