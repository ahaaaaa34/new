# 文法データ更新スキル

ユーザーが以下のフォーマットで貼り付けたデータを `/home/user/new/data.js` に追加する。

## ユーザーが送るデータのフォーマット

### FRAME / Exercise A（選択問題）

```
[FRAME または ExA]
問題文（空欄は ( ) で表す）
① 選択肢1  ② 選択肢2  ③ 選択肢3  ④ 選択肢4
答え: ②
出典: 大学名
訳: 日本語訳
解説: 解説文
```

### Exercise B（誤文訂正）

```
[ExB]
① 文の部分1 ② 文の部分2 ③ 文の部分3 ④ 文の部分4.
誤り: ③
修正: will have → have
出典: 大学名
訳: 日本語訳
解説: 解説文
```

### Exercise C（整序英作文）

```
[ExC]
日本語: 昨年以来，ビタミン剤を飲んでいる。（なければ省略）
固定(前): （文頭に固定する語句、なければ省略）
固定(後): year.（文末に固定する語句、なければ省略）
語群: been / have / I / last / since / supplements / taking / vitamin
不要語: gotとhas（なければ省略）
答え: I have been taking vitamin supplements since last year.
出典: 大学名
解説: 解説文
```

---

## やること

1. `$ARGUMENTS` にデータがあればそれを使う。なければ「問題データを貼り付けてください」と聞く。
2. `/home/user/new/data.js` を読んで各セクションの最後のIDを確認する。
3. 上記フォーマットを解析して `data.js` の正しい形式（既存データに合わせたJS）に変換し、該当セクション末尾に追加する。
4. `git add /home/user/new/data.js && git commit -m "..." && git push -u origin claude/create-matching-report-In93g`
5. 何問追加したか報告する。
