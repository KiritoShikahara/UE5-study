# 引き継ぎ (2026-10-06 10:40 / feature/lecture-011)

次の一手: UE エディタで `UW_GameUI` のデザイナーを開き、`ItemList` の「変数か」にチェックを入れて保存する(1分)。その後 `/pickup` で再開する。

## 完了
- `AC_OverlapPlayer` を新規作成。`NotifyToPlayer(OverlapActor, OwnerName)` が `BP_Player` にキャストし、`ReceivedNotifyFromEvent` を呼ぶ
- `BP_Key` に `AC_OverlapPlayer` を追加。Overlap から `NotifyToPlayer(OtherActor, KeyName)` を呼ぶ
- `BP_Player.ReceivedNotifyFromEvent`: 暫定で `OwnerName` を PrintString に出力(コンパイル成功)。変数 `ItemNames`(name[])と `ItemCounts`(int[])を追加済み(未使用)
- `UW_ItemName` を作成。`TextBlock`(`ItemName`)と変数 `DisplayName`、`GetItemNameText` でバインド
- `UW_GameUI` に `VerticalBox`(`ItemList`)と関数 `UpdateItemList(Names, Counts)` の枠を追加(中身は空)
- ue-cli の制限をまとめた: `ue-cli-report/ue-cli-bugs.md` / `ue-cli-limitations.md`

## 残り（優先順・最大5件）
1. `UW_GameUI.UpdateItemList` の実装(`ItemList` クリア → 名前ごとに `UW_ItemName` を作成、`DisplayName` を設定、`AddChild`)
2. `BP_Player.ReceivedNotifyFromEvent` の実装(`ItemNames` を `Find` → なければ `Add` と個数1、あれば個数+1 を `Set Array Elem`、最後に `GameUI.UpdateItemList(ItemNames, ItemCounts)`)
3. PIE で動作確認(鍵を取ると右上のリストに名前と個数が出るか、スイッチで `BP_MovingBox` が動くか)
4. `STM_MovingBox` の作成(現状は標準 `Cube` で代用)

## 保留
- 残り1・2 — ue-cli の制限(配列ノードの型が確定しない、ウィジェットを変数にできない)。更新後・エディタ再起動後も再現。GUI での操作か、ue-cli のプラグイン手動ビルド後の再検証が必要
- スライドの画像 — この環境では読めない(OCR で断片のみ)。ノード構成は私の推定

## 再開に必要なもの
- Unreal Engine 5.7 と `uecli`(`ProjectStudy.uproject` を開く)
- `Plugins/UECli/` `.mcp.json` `.claude/` `claude-1-ultra-sonnet-adhd.cmd` は git 管理外。`uecli setup` で再生成する
- ノード ID(`N<k>`)は変更のたびに変わる。ue-cli では guid を使う
- スライド画像は pptx を展開して取り出す(`ppt/media/imageN.png`)
