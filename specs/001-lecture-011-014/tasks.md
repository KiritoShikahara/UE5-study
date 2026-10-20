# Tasks: 講義011〜014

各タスク = エディタ作業1つ＋PIE確認。目安時間付き。

## Phase 1: コンポーネント（011） 約2時間
- [x] T01 BP_Key に RotatingMovement 追加、旧回転処理を削除（10分）
- [x] T02 STM_MovingBox 作成（マテリアル・コリジョン）（15分）
- [x] T03 BP_MovingBox 作成、InterpToMovement で2点→複数点移動（20分）
- [x] T04 BP_MovingBox の自動起動 OFF、BP_SwitchButton に TargetBox＋Sequence＋SetActive（15分）
- [x] T05 BP_Player のコリジョンにタグ `Player`（5分）
- [x] T06 AC_OverlapPlayer 作成、BP_Key に付与して名前をプレイヤーへ送る（20分）
- [x] T07 BP_Player に取得アイテムリスト（名前・個数）（15分）
- [x] T08 UW_ItemName 作成、UW_GameUI に ItemList＋更新イベント、BP_Player から呼ぶ（25分）

## Phase 2: インターフェース（012） 約1.5時間
- [x] T09 BPI_Player 作成、PlayerRecievedNotifyFromEvent を BP_Player の InterfaceGraph に実装（15分）
- [x] T10 AC_OverlapPlayer のキャストをインターフェース呼び出しへ（10分）
- [x] T11 BP_Door / BP_SwitchButton をタグ判定へ（15分）
- [x] T12 回復薬メッシュ作成（ピボット中央）（15分）
- [x] T13 BP_HealItem＋PlayerReceivedNotifyFromHealItem＋`Heal` 分岐＋HP全回復（25分）

## Phase 3: セーブ（013） 約3.5時間
- [x] T14 M_SaveLight / N_SaveLight 作成（30分）
- [x] T15 UW_ScreenEffect（Flash アニメ）、BP_Player BeginPlay で生成（25分）
- [x] T16 BP_SavePoint（N_SaveLight・Arrow）、触れると Flash（インターフェース経由）（25分）
- [x] T17 BPI_Player にセーブ用2関数、GameInstance に位置/向き保存、死亡時に Arrow 方向で復活（DelayUntilNextTick）（35分）
- [x] T18 GameInstance にアイテムリスト保存/取得、BP_Player 初期化時にロード＋UI更新（25分）
- [x] T19 BP_MovingBox 終了処理・BP_SwitchButton 押下処理の修正（20分）
- [x] T20 BP_MySaveGame 作成、Export（スロット出力）/ ImportSaveData＋Init オーバーライド（35分）
- [x] T21 R キーでセーブ削除（15分）

## Phase 4: AI（014） 約3時間
- [x] T22 BT_MyCharacter / BB_MyCharacter / BP_MyAiController 作成・紐付け、BP_MyCharacter に設定（20分）
- [x] T23 BB `TargetActor`、AIPerception（視覚）で保存、BT MoveTo（25分）
- [x] T24 NavMesh 配置、アニメーション設定、コリジョンプリセットの Camera 無視（20分）
- [x] T25 TargetPoint×3、PatrolPoints / PointPatrolIndex / 次ポイント関数（25分）
- [x] T26 BB `PatrolPoint`、BTT_GetNextPatrolPoint、Sequence＋Cooldown（30分）
- [x] T27 発見フラグ、AiController 更新、BTD_CheckUndiscovered、BT 統合、隠れ場所配置（35分）

## 依存
T05→T06→T07→T08→T09→T10→T13 / T03→T04→T19 / T15→T16→T17→T18→T20→T21 / T22→T23→T24→T25→T26→T27
