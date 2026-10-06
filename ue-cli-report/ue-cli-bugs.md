# ue-cli バグ・制限レポート (2026-10-06)

環境: UE 5.7 / Windows 11 / ue-cli プラグイン (protocolVersion 3)
作業: 講義011 後半 (`BP_Player` / `AC_OverlapPlayer` / `UW_GameUI`)

## 1. ワイルドカード配列ノードの型が `ue_connect_pins` で確定しない (重大)

- 対象: `KismetArrayLibrary` の `Array_Add` / `Array_Find` / `Array_Get` / `Array_Set` など
- 再現:
  1. `ue_add_variable` で `name[]` を作る (`ItemNames`)
  2. `ue_add_node` で `CallFunction` `Array_Find` (memberParent `KismetArrayLibrary`) を追加
  3. `ItemNames` の `VariableGet` を追加し、`ue_connect_pins` で `TargetArray` に接続
  4. `ue_get_graph`: `TargetArray` / `ItemToFind` が `category: wildcard` のまま
  5. `ue_compile_blueprint`: 「Target Array のタイプが不確定です」(接続数だけエラー、11件)
- 切断 → 再接続しても変わらない
- 期待: GUI と同様に、接続時に配列要素型が伝播する
- 推測原因: `MakeLinkTo` 相当の接続で `NotifyPinConnectionListChanged` / ノードの再構築が呼ばれていない
- 回避策: なし (Python の `BlueprintEditorLibrary` に再構築 API なし)。今回は配列ロジックを断念した
- 提案: `ue_connect_pins` 後に `UEdGraphSchema_K2::TryCreateConnection` を使うか、ノードを `ReconstructNode` する

## 2. Map 型の変数を `ue_add_variable` で作れない

- `map<name,int>` → `Could not interpret 'map<name,int>' as a variable type.`
- `ue_get_blueprint` は既存の Map を `TMap<name>` と表示する (値型が欠ける)
- Python でも `unreal.EPinContainerType` が無く、作れない
- 提案: `map<key,value>` 形式をサポートする

## 3. ウィジェットの `bIsVariable` を設定できない

- `ue_add_widget` で追加したウィジェットは `isVariable: false`
- `ue_set_property ... bIsVariable` → `not editable`。Python も同様
- 結果: `VariableGet` が `pins: []` の壊れたノードになり、グラフから参照できない
- 提案: `ue_add_widget` に `isVariable` 引数を追加する。または `ue_set_widget_variable` を新設する

## 4. `ue_create_blueprint` が UserWidget 親で通常 Blueprint を作る

- `parentClass: UserWidget` → クラスは `Blueprint` (`WidgetBlueprint` ではない)。ウィジェットツリーが無く `ue_add_widget` が使えない
- 回避策: Python で `WidgetBlueprintFactory` + `AssetTools.create_asset`
- 提案: 親が `UserWidget` 系なら `WidgetBlueprint` を作る

## 5. `N<k>` ノード ID が変更操作のたびに振り直される

- `ue_delete_node` で `N2` を消すと、以降の `N3...` が詰まる
- 連続削除で意図しないノードを消した (`N10` 以降は「存在しない」エラー)
- ドキュメントには「1回の読み取り内でのみ安定」とあるが、削除・追加のたびに変わる点が危険
- 回避策: guid を使う
- 提案: 変更系ツールの応答に注意書きを入れる。または既定で guid を返す

## 6. `ue_add_node` の Cast が `BP_Player` を解決できない

- `memberName: BP_Player` → `did not resolve`。`BP_Player_C` なら成功
- 他ツールとの不一致 (関数の memberParent も `_C` 必須)。エラー文にヒントがない
- 提案: Blueprint 名を `_C` なしでも解決する。または候補をエラー文に出す

## 7. ツール接続の一時切断

- `ue_run_python` が `Could not reach the UE CLI Plugin at http://127.0.0.1:8720/` で1回失敗 (ログに `[Callstack]` 出力あり)
- 数十秒後に `ue_ping` で復帰。編集内容は保持されていた
- 原因は未調査
