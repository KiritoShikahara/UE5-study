# 引き継ぎ (2026-10-20 09:35 / main)

次の一手: プロジェクトフォルダで `unrealengine-cli doctor` を実行し、結果を確認する

## 完了
- 講義資料 011〜014（`doc/*.pptx`）を読んで spec を作成 → `specs/001-lecture-011-014/spec.md`
- 27タスクに分解（4フェーズ、約10時間）→ `specs/001-lecture-011-014/tasks.md`
- `unrealengine-cli` で BP を編集できることを確認（`blueprint add-node/connect/compile/save` など）

## 残り（優先順）
- `unrealengine-cli setup --add-code-module --build` を実行する（ユーザーの承認待ち。下の「保留」を参照）
- `unrealengine-cli editor open --headless` → `ping`
- `tasks.md` の Phase 1（T01〜T08: コンポーネント）を実装
- Phase 2〜4（T09〜T27: インターフェース / セーブ / AI）を実装
- タスクごとに `blueprint compile` と PIE で受け入れ条件を確認

## 保留
- setup — このプロジェクトは BP のみのため、setup すると C++ プロジェクトになる（`Source/` が追加され、`.uproject` も変更される）。これをユーザーが承認するのを待っている

## 再開に必要なもの
- UE 5.7
- `unrealengine-cli`（`C:/Users/com/.cargo/bin` に入っているもの。ほかの PC には `install.ps1` でインストール）
- Visual Studio Build Tools（C++ モジュールのビルドに使う）
- 注意: `Intermediate/` と `Saved/` は `.gitignore` に書いてあるのに、過去に git に追加済みのため変更が出続けている。今回のコミットには含めていない
