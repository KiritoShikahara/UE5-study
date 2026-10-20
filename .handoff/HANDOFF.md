# 引き継ぎ (2026-10-20 09:51 / main)

次の一手: map 対応済みの unrealengine-cli で `unrealengine-cli tool call bp_add_function --args '{"path":"/Game/Project/Blueprints/UW_GameUI","name":"UpdateItemList","inputs":[{"name":"ItemList","type":"map<name,int>"}]}'` が通るか確認する

## 完了
- T01 BP_Key: RotatingMovement（Yaw 180）追加、旧 Timeline 回転処理を削除（`Content/Project/Blueprints/BP_Key.uasset`）
- T02 STM_MovingBox（100x100x50、M_Floor、Box コリジョン）（`Content/Project/Assets/Meshes/STM_MovingBox.uasset`）
- T03/T04 BP_MovingBox（親 BP_EventBase、InterpToMovement: Duration 3、4点、LoopReset、AutoActivate OFF）、BP_SwitchButton に TargetBox＋Sequence＋IsValid→SetActive
- T05 BP_Player の CollisionCylinder に ComponentTags `Player`
- T06 一部: AC_OverlapPlayer.NotifyToPlayer(OverlapActor, OwnerName) → Cast BP_Player → ReceivedNotifyFromEvent
- T08 一部: UW_ItemName（ルート TextBlock `ItemName`、DisplayItem: Text, ExposeOnSpawn、Construct→SetText）、UW_GameUI に VerticalBox `ItemList`（右上アンカー）

## 残り（優先順・最大5件）
- UW_GameUI: 暫定の Map 変数 `ItemNameList` を削除し、`UpdateItemList(ItemList: Map<Name,int>)` を資料 slide 35-37 どおり作る（ClearChildren→Keys→ForEach→Append「Key: n」→CreateWidget UW_ItemName→AddChildToVerticalBox）
- BP_Player.ReceivedNotifyFromEvent（関数・中身未実装）: Map_Add(ItemNameList, OwnerName, Find+1) → GameUI.UpdateItemList(ItemNameList)
- BP_Key: AC_OverlapPlayer を付与し、Overlap を ComponentHasTag(OtherComp,"Player")→Branch→NotifyToPlayer(OtherActor, KeyName)→SaveKeyStatus→SetEventFinished に差し替え（今は Cast To BP_Player のまま）
- レベルに BP_MovingBox を配置し、スイッチの TargetBox に設定 → PIE で確認
- 資料 012〜014（specs/001-lecture-011-014/tasks.md の T09 以降）

## 保留
- UpdateItemList の Map 引数 — unrealengine-cli の map 型対応待ち（ユーザーが修正中）

## 再開に必要なもの
- UE 5.7.4、`unrealengine-cli`（map 対応版）
- `unrealengine-cli setup --prebuilt` を実行（Plugins/UnrealCliBridge/Binaries は git 管理外）
- 資料の画像書き起こし（notes00〜03.md）は前の PC の一時フォルダにあるだけで git には入っていない。必要なら doc/*.pptx から `image` ロールで作り直す
