# SUNSET DRIFT（151-racing3）仕様書

- 仕様書名: SUNSET DRIFT
- 対象ゲーム: `151-racing3`
- 作成日: 2026-08-20
- 更新日: 2026-08-20
- ステータス: 改修中（公開ゲート用の最低限）
- 参照ファイル: `index.html` / `LEARNINGS.md`

## 1. ゲーム概要

- ジャンル: 疑似3Dレーシング
- 一言説明: ネオンのコースを3周し、ベストラップと順位を狙うブラウザレーサー
- 想定プレイ時間: 1レース 1〜3分
- 想定プレイヤー: iPhone Safari（主戦場）、PCブラウザ
- クリア体験の要点: 3周完走後に表彰台とタイムを出す

## 2. 対象環境

### 必須
- 配信先: GitHub Pages
- 最優先端末: iPhone Safari
- 対応画面幅: 320px〜430px を基準
- 実装方式: HTML / CSS / JavaScript の静的ファイルのみ

### 任意
- PC: キーボード操作
- iPad: タッチ操作（十字キー＋操作ボタン）

## 3. ファイル構成

### 必須
- 公開エントリは `index.html`（現状は CSS / JS を同ファイルに内包）
- 追加JSはゲームフォルダ直下（`js/` は作らない）

### 現状
```text
151-racing3/
  index.html
  LEARNINGS.md
  SPEC.md
  .nojekyll
```

### 未確定
- `index.html` が 1000行超のため、`style.css` / `script.js` への分割時期

## 4. コアループ

- 開始: タイトルでコースと車サイズを選ぶ → START
- カウントダウン 3-2-1 のあと走行
- 3周完走で結果画面（順位・タイム・リトライ）
- スコア: 周回タイム、ベストラップ、最終順位、ニアミス数、コンボ

## 5. 画面 / 状態遷移

- 画面: タイトル / カウントダウン / プレイ / 結果
- 遷移: `title -> count -> play -> done`、結果から RETRY で `count` に戻る
- ポーズ画面: なし

## 6. 操作

### 必須（携帯入力規則）
- 操作ドックはプレイ画面の下25%
- 左: 十字キー（左右ステア、上=アクセル、下=ブレーキ）
- 右: GAS（アクセル）/ BRAKE（ブレーキ）/ BOOST（ブースト）
- 打感: `pointerdown`、押下時 `scale(0.92)`（80ms以内）、短い振動、短いSE
- 初回タップで AudioContext を unlock
- ダブルタップズーム防止、スクロール防止、safe-area 対応

### PC
- ← → ステア / ↑ または W または Space アクセル / ↓ または S ブレーキ / Shift または B ブースト

## 7. 勝敗条件

- 目的: 3周を完走し、順位とベストラップを出す
- クリア: 3周完走（ゲームオーバーなし）
- リトライ: 結果画面の RETRY
- コースアウトは減速。ブーストは走行スキルで貯めて消費する

## 8. UI / HUD

- TIME / LAP / BEST / RANK、速度、ゴースト差、ニトロゲージ、ミュート
- 携帯時は速度メーターを操作ドックの上へ退避
- Safe Area: `env(safe-area-inset-*)` を HUD と操作ドックに適用

## 9. 音声

### 必須
- 開始: 初回タップで `AudioContext` 初期化
- 走行音: WebAudio のエンジン音
- 効果音: 操作のポチ音、ブースト、完走
- ミュート: HUD の ♪ ON/OFF、`localStorage` に保存
- 復帰: `visibilitychange` / `pageshow` / `focus` で unlock

### 未確定
- フォルダ内 MP3（`ハンドル切って.mp3` 系）は現行コードから未参照

## 10. 保存

- `localStorage`: ミュート、ベストラップゴースト
- 個人情報は保存しない

## 11. 実装制約

- バニラ JS、ビルドなし、`requestAnimationFrame`
- 公開実体 20MB 以下（`.git` / `_playtest` 除外）
- 外部 CDN 非依存

## 12. テスト項目

- [x] 構文チェック（esbuild / node --check）
- [x] harness（2026-08-20T12:21:46Z PASS、通信量 0.08MB）
- [ ] iPhone 実機の親指バランス
- [ ] GitHub Pages 200

## 13. 未確定事項（公開ブロッカーではない）

- 未参照 MP3 の削除可否
- `index.html` 分割の実施時期
- 横向き時のボタン下限サイズの最終調整
