# 引き継ぎ (2026-10-06 09:30 / main)

次の一手: `doc/011_講義資料（コンポーネントの活用）.pptx` のスライド36〜38(画像のみ)を開いて内容を確認する。

## 完了
- リポジトリをクローン (`UE5-study`)
- 講義資料011を読了。内容: 標準コンポーネント(RotatingMovement / 移動補間でBP_MovingBox / BP_SwitchButtonのTargetBox)と、ActorComponent自作(`AC_OverlapPlayer`、獲得アイテムリストを`UW_GameUI`の`ItemList`に表示)

## 残り（優先順・最大5件）
- プロジェクト現状確認(`BP_Key`等が講義のどの段階か)
- 講義011の前半を実装(RotatingMovement、BP_MovingBox、スイッチ連動)
- 講義011の後半を実装(AC_OverlapPlayer、UW_ItemName、UW_GameUI、BP_Player)

## 再開に必要なもの
- Unreal Engine 5(`ProjectStudy.uproject` を開く)
- `Intermediate/` `Saved/` `DerivedDataCache/` はgit追跡済みのキャッシュ。コミットせず除外(`.gitignore`に追加。追跡解除は未実施)
