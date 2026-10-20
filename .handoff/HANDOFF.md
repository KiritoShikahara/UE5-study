# 引き継ぎ (2026-10-20 10:53 / main)

次の一手: 修正済みの unrealengine-cli で `unrealengine-cli setup --prebuilt`（エディタを閉じてから）→ `BP_MySaveGame` に `EventStatus: map<name,EN_EventStatus>` を `bp_add_variable` で追加できるか確認（issue #18）

## 完了
- 講義012 T09〜T13: `BPI_Player`、`BP_Player` の `InterfaceGraph`、Cast→タグ/インターフェース化、`STM_HealItem`/`M_Green`/`BP_HealItem`（PIE で HP 25→100）
- 講義013 T15〜T18: `UW_ScreenEffect`（Flash）、`BP_SavePoint`（Capsule・N_SaveLight・Arrow、レベルの (300,-100,0) Yaw90）、位置/向きの保存と復活（`GameReset` 後に復活を PIE 確認）、ItemNameList の GameInstance 保存/読み込み（PIE で `Key002: 1` / `Key001: 1`）
- T14 一部: `M_SaveLight` 完成、`N_SaveLight` は数値のみ
## 残り（優先順・最大5件）
- T20 BP_MySaveGame（Enum map 変数2つ追加 → GameInstance に ExportSaveData / ImportSaveData、各 Save 関数の末尾で Export、Event Init → ImportSaveData。スロット `MySaveGame` / UserIndex 0）
- T19 BP_MovingBox の SetEventFinished オーバーライド（親呼び出し→InterpToMovement.SetActive(true)）、BP_SwitchButton の Sequence を Then0: TargetDoor→OpenDoor / Then1: TargetBox→SetEventFinished / Then2: self SetEventFinished に
- T21 R キーでセーブ削除（DeleteSaveData→Delay0.2→OpenLevel）
- 講義014 T22〜T27
## 保留
- T14 N_SaveLight の Shape=Cylinder / Lifetime Mode=Random / Velocity Mode=Linear・GravityForce 無効 / SpriteRenderer Material=M_SaveLight — ユーザーが手で修正して報告予定（issue #15）
- T19 — issue #17（オーバーライド関数・親関数呼び出しノード）
- T20/T21 — issue #18（Enum 型変数）
- 関数名 `PlayerRecievedNotifyFromEvent` を資料どおり `PlayerReceivedNotifyFromEvent` に直すか — ユーザー回答待ち
## 再開に必要なもの
- UE 5.7.4、修正済み `unrealengine-cli`
- `unrealengine-cli setup --prebuilt`（Plugins/UnrealCliBridge/Binaries は git 外）
- 資料の画像書き起こしは git 外（必要なら doc/*.pptx から `image` ロールで作り直す）
## メモ
- cli修正したやつを反映します
- CLI の issue: #11〜#18（#11 は bp_add_graph で解決済み、未クローズ）
