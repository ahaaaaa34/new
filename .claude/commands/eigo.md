# 英コミュ長文データ更新スキル

画像から英文・日本語訳・文法解説を抽出して追加するスキル。

## 手順

1. まず以下のファイルを読んで現在のデータ形式と末尾の状態を把握する：
   - `/home/user/new/data.js` （EIGO_SENTENCES の形式・末尾を確認）
   - `/home/user/new/vocab/data.js` （WORDS の形式・最後のIDを確認）

2. `$ARGUMENTS` に画像パスがあれば Read ツールで読む。なければ「画像のパスを教えてください」と聞く。

3. 画像から以下を1文ずつ抽出する：
   - 英文
   - 日本語訳
   - 文法・構造の解説（画像にあればそのまま、なければ文の構造を簡潔に説明）

4. 手順1で確認した EIGO_SENTENCES の形式に合わせて `data.js` の末尾に追加する。

5. 同じ英文から英検3級以上の語彙を抽出する：
   - be動詞・超基本語（a/the/in/on/is等）は除く
   - すでに vocab/data.js にある単語は追加しない
   - レベル判定の目安：3級=中学応用語、準2級=高校基本語、2級=高校発展語、準1級=それ以上
   - 例文はテキストから引用するか短い例文を作る

6. 手順1で確認した WORDS の形式・IDの続きで `vocab/data.js` の末尾に追加する。

7. `git add /home/user/new/data.js /home/user/new/vocab/data.js && git commit -m "eigo: add N sentences and M words from image" && git push -u origin claude/create-matching-report-In93g`

8. 何文・何単語追加したか報告する。
