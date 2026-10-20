# Spec: 講義011〜014 の実装

出典: `doc/011〜014_講義資料*.pptx`（スライド内のテキストより抽出。BPグラフの細部はスライド画像のみに記載）
対象: UE 5.7 / `Content/Project/`（Blueprint のみ、C++ なし）

## 前提（既存アセット）
BP_Player, BP_MyCharacter, BP_Key, BP_Door, BP_SwitchButton, BP_EventBase, BP_MyGameInstance, UW_GameUI, N_Fire, InGame/TestLevel

## F1. コンポーネントの活用（011）
- F1-1 BP_Key の回転を `RotatingMovement` コンポーネントに置き換え、旧回転処理を削除
- F1-2 `STM_MovingBox`（ジオメトリで作るボックス＋マテリアル＋コリジョン）と `BP_MovingBox`（InterpToMovement で2点/複数点移動）
- F1-3 BP_MovingBox は自動起動しない。BP_SwitchButton に `TargetBox`（インスタンス編集可能）を追加し、Sequence で Door を開く処理と `SetActive` の両方を実行
- F1-4 ActorComponent `AC_OverlapPlayer`: タグ `Player` を持つコリジョンとの重なりを検知し、オーナー名をプレイヤーへ通知
- F1-5 BP_Player に取得アイテムリスト（名前→個数）を持たせる
- F1-6 `UW_ItemName`（Text `ItemName`、変数 `DisplayName`）と UW_GameUI の VerticalBox `ItemList`（右上アンカー）＋更新用カスタムイベント
- 受け入れ: スイッチで扉が開き床が動く／鍵の名前と個数が右上に一覧表示される

## F2. インターフェース（012）
- F2-1 `BPI_Player` に `PlayerRecievedNotifyFromEvent(OwnerName)`。BP_Player に実装（`InterfaceGraph` に配置）
- F2-2 AC_OverlapPlayer のキャストをインターフェース呼び出しに変更
- F2-3 BP_Door / BP_SwitchButton の当たり判定をタグ判定に変更
- F2-4 回復薬メッシュ（ピボット中央）と `BP_HealItem`。文字列 `Heal` を送信
- F2-5 BPI_Player に `PlayerReceivedNotifyFromHealItem`。AC_OverlapPlayer で `Heal` の場合に分岐、BP_Player で HP 全回復
- 受け入れ: キャストなしで F1 と同じ挙動／回復薬取得で HP 全回復

## F3. セーブ機能（013）
- F3-1 `M_SaveLight`（BlendMode: TranslucentGreyTransmittance）と `N_SaveLight`（Fountain テンプレ、SpriteRenderer に M_SaveLight）
- F3-2 `UW_ScreenEffect`: ScaleBox＞Border `Effect`、0.3秒で RenderOpacity 0→0.5→0、再生イベント `Flash`。BP_Player の BeginPlay で生成し変数保持
- F3-3 `BP_SavePoint`（Actor 親、N_SaveLight、Arrow）。触れるとインターフェース経由で Flash＋位置/向きを BP_MyGameInstance に保存
- F3-4 BPI_Player にセーブ用インターフェース2つ追加・BP_Player に実装。死亡時は Arrow 方向で復活（読み込みは `DelayUntilNextTick` 後）
- F3-5 GameInstance にアイテムリストの保存/取得。ItemNameList 変更時に保存、BP_Player 初期化時にロードし UI 更新
- F3-6 BP_MovingBox のイベント終了処理と BP_SwitchButton の押下処理を修正（イベント状態を正しく管理）
- F3-7 `BP_MySaveGame`（SaveGame 親、GameInstance と同じ変数）。スロット名付きで出力、`Init` オーバーライドで `ImportSaveData`
- F3-8 R キーでセーブデータ削除
- 受け入れ: 鍵1→死亡→復活→鍵2 で UI が 2 個表示／PIE 再起動で状態復元／`Saved/SaveGame` にファイル出力／R で初期状態

## F4. AI 制御の基礎（014）
- F4-1 `BT_MyCharacter` / `BB_MyCharacter` / `BP_MyAiController`（BT 実行）。BP_MyCharacter の AIController に設定
- F4-2 BB キー `TargetActor`(Object)。AIPerception（視覚）で検知したプレイヤーを保存、BT の MoveTo で追跡
- F4-3 NavMeshBoundsVolume 配置。BP_MyCharacter のアニメーション設定修正。コリジョンプリセットで Pawn / CharacterMesh の Camera を無視
- F4-4 TargetPoint×3、BP_MyCharacter に `PatrolPoints`（インスタンス編集可能）・`PointPatrolIndex`・次ポイント取得関数
- F4-5 BB キー `PatrolPoint`、`BTT_GetNextPatrolPoint`（Finish Execute Success=true）、Sequence＋Cooldown
- F4-6 BB 発見フラグ、AiController でフラグ/Target 更新、`BTD_CheckUndiscovered`（未発見で true）。未発見→巡回／発見→追跡
- 受け入れ: 未発見→巡回、発見→追跡、見失う→巡回に戻る

## 制約
- `.uasset` はバイナリのため、実装は UE エディタ上の手作業。各タスク後に PIE で受け入れ確認
- 名前は資料表記のまま（`Recieved` の綴りも資料通り）
