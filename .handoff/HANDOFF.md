# 引き継ぎ (2026-10-06 09:55 / feature/lecture-011)

次の一手: `uecli setup --project <ProjectStudy.uproject のパス>` を実行し、エディタを開いて `uecli ping` で接続を確認する。

## 完了
- 講義011の前半を実装(コンパイル成功・保存済み)
  - `BP_Key`: `RotatingMovement`(Yaw 90)を追加し、タイムライン回転ノードを削除
  - `BP_MovingBox`(新規): `Cube` + `InterpToMovement`、PingPong、Z+400 を 3秒、自動起動オフ
  - `BP_SwitchButton`: `TargetBox`(インスタンス編集可)を追加。Sequence で `OpenDoor` と `SetActive(true)` を実行
- `Intermediate/` `Saved/` `DerivedDataCache/` を git 追跡から外した(ファイルは残してある)

## 残り（優先順・最大5件）
- 講義011の後半: `AC_OverlapPlayer`、`BP_Key` と `BP_Player` の修正、`UW_ItemName`、`UW_GameUI` の `ItemList`
- レベルへ `BP_MovingBox` を置き、`BP_SwitchButton` の `TargetBox` を設定して動作確認

## 保留
- 後半の実装 — スライド25〜27・29・36〜38 のノード構成が画像のみ。スクショをもらうか、私の設計で進めるかの判断待ち
- `STM_MovingBox` 未作成 — 現状は標準 `Cube` で代用

## 再開に必要なもの
- Unreal Engine 5.7 と `uecli`(`ProjectStudy.uproject` を開く)
- `Plugins/UECli/` `.mcp.json` `.claude/` `claude-1-ultra-sonnet-adhd.cmd` は git 管理外。`uecli setup` で再生成する(`.uproject` はプラグインを参照済み)
