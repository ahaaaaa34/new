# 単語データ更新スキル

ユーザーが以下のフォーマットで貼り付けたデータを `/home/user/new/vocab/data.js` に追加する。

## ユーザーが送るデータのフォーマット

```
英単語 / 日本語の意味 / レベル（3級・準2級・2級・準1級）
例文: 英語の例文（省略可）
訳: 例文の日本語訳（省略可）

英単語 / 日本語の意味 / レベル
例文: ...
訳: ...
```

### 例

```
abandon / 捨てる、断念する / 2級
例文: He abandoned his plan to study abroad.
訳: 彼は留学する計画を諦めた。

ambition / 野心、大志 / 準2級
例文: She has a great ambition to become a doctor.
訳: 彼女は医者になるという大きな野心を持っている。

scrutinize / 詳しく調べる / 準1級
```

（例文・訳は省略可。省略した場合は空文字列にする）

---

## やること

1. `$ARGUMENTS` にデータがあればそれを使う。なければ「単語データを貼り付けてください」と聞く。
2. `/home/user/new/vocab/data.js` を読んで最後のIDを確認する。
3. 上記フォーマットを解析して `vocab/data.js` の正しい形式に変換し、WORDS配列の末尾に追加する。
4. `git add /home/user/new/vocab/data.js && git commit -m "..." && git push -u origin claude/create-matching-report-In93g`
5. 何単語追加したかレベル別に報告する。
