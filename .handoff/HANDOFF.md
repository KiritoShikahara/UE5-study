# 引き継ぎ (2026-10-20 10:10 / main)

次の一手: issue #11（https://github.com/KiritoShikahara/unrealengine-cli/issues/11）の結果を確認し、資料の `InterfaceGraph` が手で追加したイベントグラフのページかどうか決める

## 完了
- T06〜T08（講義011）: `UW_GameUI.UpdateItemList(Map<Name,int>)`、`BP_Player.ReceivedNotifyFromEvent`（個数+1→UI更新）、`BP_Key` を `AC_OverlapPlayer` 経由の Player タグ判定に変更、`TestLevel` に `BP_MovingBox` を (400,-290,0) へ配置し `BP_SwitchButton.TargetBox` に設定。PIE で `Key: 2` 表示を確認
- T09: `BPI_Player`（`PlayerRecievedNotifyFromEvent(OwnerName)`）作成、`BP_Player` に実装。イベントは仮に `EventGraph` に配置（→ `ReceivedNotifyFromEvent` を呼ぶ）
- T10（PIE 未確認）: `AC_OverlapPlayer.NotifyToPlayer` の Cast をインターフェース呼び出しに変更
## 残り（優先順・最大5件）
- T10 の PIE 確認（鍵を取って UI 表示）→ tasks.md の T09/T10 にチェック
- T11 BP_Door / BP_SwitchButton をタグ判定へ
- T12 回復薬メッシュ（ピボット中央）
- T13 BP_HealItem＋PlayerReceivedNotifyFromHealItem＋`Heal` 分岐＋HP全回復
- 資料 013〜014（T14 以降）
## 保留
- BP_Player のインターフェースイベントを `InterfaceGraph` に移す — CLI にイベントグラフ追加ツールがない（issue #11）。手で追加したページなら CLI 対応待ち、自動生成のものなら現状のままで issue を閉じる
## 再開に必要なもの
- UE 5.7.4、`unrealengine-cli` 0.6.0 以降
- `unrealengine-cli setup --prebuilt`（Plugins/UnrealCliBridge/Binaries は git 管理外）
- 資料の画像書き起こしは git 外。必要なら doc/*.pptx から `image` ロールで作り直す
