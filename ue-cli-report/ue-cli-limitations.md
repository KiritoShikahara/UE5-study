# ue-cli の制限まとめ (2026-10-06)

詳細な再現手順は `ue-cli-bugs.md` を参照。

## 実装を止めた制限

### 1. 配列ノードの型が確定しない (bugs.md 項目1)
- 対象: `Array_Add` / `Array_Find` / `Array_Get` / `Array_Set`
- `ue_connect_pins` で `name[]` 変数をつないでも、ピンが `wildcard` のまま
- コンパイルで「Target Array のタイプが不確定です」(11件)
- GUI では接続時に型が自動で決まる。CLI の接続では決まらない
- 切断して再接続しても直らない

### 2. ウィジェットを変数にできない (bugs.md 項目3)
- `ue_add_widget` で追加したウィジェットは `isVariable: false`
- `bIsVariable` は CLI (`ue_set_property`) からも Python からも変更不可
- グラフで `ItemList` を参照する `VariableGet` が作れず、`UW_GameUI.UpdateItemList` の中身を組めない

### 更新後の再検証 (2026-10-06)
- ue-cli 更新後も、制限1・2は再現した
  - 制限1: `ReceivedNotifyFromEvent` で配列ノードを配線すると、同じ11件のエラー
  - 制限2: `ue_add_widget` で追加した `ItemList` は `isVariable: false` のまま。`ue_set_property bIsVariable` も `not editable`
- 注意: 出力を使わない純粋ノード (`Array_Length` など) は、型が不確定でもコンパイルが通る
  - 単独のテストでは成功に見える。出力を実行ピンの先につないで初めてエラーになる
- `ue_find_tools` のスキーマに変更なし。エディタが旧プラグインのままの可能性がある (未確認)

### エディタ再起動後の再検証
- 同じ結果だった
  - `Array_Add` を実行ピンにつなぐと「Target Array / New Item のタイプが不確定です」(2件)
  - `ItemList` は `isVariable: false` のまま
- 補足: `ue_rebuild` は、エディタが `UnrealEditor-UECliPlugin.dll` を掴んでいるため `LNK1104` で失敗した。プラグインの更新を反映するには、エディタを閉じた状態でビルドする必要がある

## 回避済みの制限
- Map 型の変数を作れない (項目2) → 配列2本 (`ItemNames` / `ItemCounts`) で代用する設計にした
- `UserWidget` 親の Blueprint が通常 Blueprint になる (項目4) → Python の `WidgetBlueprintFactory` で作り直した
- `N<k>` ノード ID が変更のたびに振り直される (項目5) → guid で指定する
- Cast の対象に `_C` が必要 (項目6) → `BP_Player_C` を指定した

## 現在の実装状況 (講義011 後半)
| 項目 | 状態 |
|---|---|
| `AC_OverlapPlayer.NotifyToPlayer` | 完了 |
| `BP_Key` から `NotifyToPlayer` を呼ぶ | 完了 |
| `BP_Player.ReceivedNotifyFromEvent` | 暫定: `OwnerName` を PrintString で出力 |
| `UW_ItemName` (`DisplayName` を Text にバインド) | 完了 |
| `UW_GameUI` の `ItemList` と `UpdateItemList` の枠 | 枠のみ。中身は未実装 |
| アイテム個数の管理と表示 | 未実装 (制限1・2が原因) |
| PIE での動作確認 | 未実施 |

## GUI で必要な操作 (計2か所)
1. `UW_GameUI` のデザイナーで `ItemList` を選び、「変数か」にチェックを入れて保存する
2. `BP_Player.ReceivedNotifyFromEvent` に配列ノードを GUI で配線する
   - 流れ: `ItemNames` を `Find` → 見つからなければ `Add` と個数 `1` を追加、見つかれば個数を +1 して `Set Array Elem`
   - 最後に `GameUI.UpdateItemList(ItemNames, ItemCounts)` を呼ぶ

## ツール側への要望
- `ue_connect_pins` 後にワイルドカードの型を伝播させる
- `ue_add_widget` に `isVariable` 引数を追加する
- `ue_add_variable` で `map<key,value>` をサポートする
