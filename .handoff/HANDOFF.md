# 引き継ぎ (2026-10-06 10:10 / feature/lecture-011)

次の一手: スライド25・26・27・29・36・37・38 のスクショをチャットに直接貼る(画像が読めれば後半の実装に進める)。貼れない場合は「私の設計で進める」と指示する。

## 完了
- 講義011の前半を実装(前回コミット 96ed801)
  - `BP_Key`: `RotatingMovement`(Yaw 90)、`BP_MovingBox`(新規)、`BP_SwitchButton` の `TargetBox`
- レベル `TestLevel` に `BP_MovingBox` を配置(210,-600,0)し、`BP_SwitchButton.TargetBox` に設定。保存済み
- スライド画像を `doc/011_講義資料（コンポーネントの活用）.pptx` から抽出して対応を確認
  - スライド25→image21、26→image24、27→image26・45、29→image35、36→image44、37→image43、38→image32

## 残り（優先順・最大5件）
- 講義011の後半: `AC_OverlapPlayer`、`BP_Key` と `BP_Player` の修正、`UW_ItemName`、`UW_GameUI` の `ItemList`
- 動作確認(PIE で スイッチ → `BP_MovingBox` が上下に動くか)

## 保留
- 後半の実装 — ノード構成が画像のみ。この環境(`read` と worker)では画像を読めなかった(`CANNOT_SEE_IMAGE`)。スクショの提供か設計一任の判断待ち
- `STM_MovingBox` 未作成 — 現状は標準 `Cube` で代用

## 再開に必要なもの
- Unreal Engine 5.7 と `uecli`(`ProjectStudy.uproject` を開く)
- `Plugins/UECli/` `.mcp.json` `.claude/` `claude-1-ultra-sonnet-adhd.cmd` は git 管理外。`uecli setup` で再生成する
- スライド画像は pptx を展開して取り出す(`ppt/media/imageN.png`)
