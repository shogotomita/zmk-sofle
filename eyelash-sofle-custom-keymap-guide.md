# Eyelash Sofle カスタムKeymap 作成・書き込み手順

## 概要

Eyelash Sofle キーボードのカスタムキーマップを作成し、ファームウェアをビルド・書き込みする手順です。

---

## 前提条件

- GitHubアカウント
- USBケーブル（キーボード接続用）
- Eyelash Sofle キーボード

---

## 方法1：GitHub経由でビルド（推奨）

### Step 1：リポジトリをフォーク

1. 以下のURLにアクセス  
   https://github.com/a741725193/zmk-sofle

2. 右上の「**Fork**」ボタンをクリック

3. 自分のGitHubアカウントにリポジトリがコピーされる

---

### Step 2：keymapファイルを編集

1. フォークしたリポジトリで以下のファイルを開く
   ```
   config/eyelash_sofle.keymap
   ```

2. 鉛筆アイコン（Edit this file）をクリック

3. ファイル内容を自分のキーマップに置き換え

4. ページ下部の「**Commit changes**」をクリック
   - Commit message: `Update keymap` など任意のメッセージ
   - 「Commit directly to the main branch」を選択
   - 「Commit changes」ボタンをクリック

---

### Step 3：GitHub Actionsでビルド

1. リポジトリの「**Actions**」タブをクリック

2. ワークフローが自動で開始される（コミット後）
   - 開始されない場合は「Run workflow」をクリック

3. ビルド完了まで待つ（約5〜10分）
   - 緑のチェックマーク ✅ が出れば成功

4. 完了したワークフローをクリック

5. 「**Artifacts**」セクションからファームウェアをダウンロード
   - `firmware.zip` または個別の `.uf2` ファイル

---

### Step 4：ファームウェアを書き込み

#### 左手側

1. 左手キーボードをUSBで接続

2. 背面のリセットボタンを**ダブルタップ**（素早く2回押す）

3. `NICENANO` という名前のUSBドライブが出現

4. ダウンロードした左手用ファイルをドラッグ＆ドロップ
   ```
   eyelash_sofle_studio_left-zmk.uf2
   ```
   または
   ```
   eyelash_sofle_left-zmk.uf2
   ```

5. 自動で再起動（ドライブが消える）

#### 右手側

1. 右手キーボードをUSBで接続

2. 背面のリセットボタンを**ダブルタップ**

3. `NICENANO` ドライブが出現

4. 右手用ファイルをドラッグ＆ドロップ
   ```
   eyelash_sofle_right-zmk.uf2
   ```
   または
   ```
   nice_view_custom-eyelash_sofle_right-zmk.uf2
   ```

5. 自動で再起動

---

## 方法2：ZMK Studioで変更（簡易）

キーマップの軽微な変更なら、ファームウェア再ビルド不要。

### 手順

1. ZMK Studio を開く
   - Webアプリ：https://zmk.studio
   - またはデスクトップアプリ

2. キーボードをUSBまたはBluetoothで接続

3. キーボードが認識されたら、キーをクリックして変更

4. 変更は自動保存される

### 制限事項

- 基本的なキー変更のみ
- 複雑なマクロ、コンボ、条件分岐は `.keymap` ファイル編集が必要

---

## トラブルシューティング

### ドライブが出現しない

- リセットボタンを**素早く2回**押す（ダブルクリックの感覚）
- USBケーブルがデータ転送対応か確認（充電専用ケーブルは不可）
- 別のUSBポートを試す

### ビルドが失敗する

1. Actionsタブでエラーログを確認
2. `.keymap` ファイルの構文エラーをチェック
   - 括弧の閉じ忘れ
   - セミコロン忘れ
   - キーコードのタイプミス

### 左右の接続が不安定

1. 両方のキーボードをリセット
2. `settings_reset` ファームウェアを書き込み
3. 通常のファームウェアを再書き込み

Settings Reset ファームウェア：
```
settings_reset-nice_nano_v2-zmk.uf2
```

---

## ファイル構成

```
zmk-sofle/
├── .github/workflows/      # GitHub Actions設定
├── boards/arm/eyelash_sofle/  # ボード定義
├── config/
│   ├── eyelash_sofle.conf     # 設定ファイル
│   └── eyelash_sofle.keymap   # ← これを編集
├── keymap-drawer/          # キーマップ画像生成
├── zephyr/
├── build.yaml              # ビルド設定
└── README.md
```

---

## キーマップファイルの基本構造

```dts
#include <behaviors.dtsi>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/bt.h>

/ {
  keymap {
    compatible = "zmk,keymap";

    default_layer {
      display-name = "Base";
      bindings = <
        // キーバインディング
      >;
    };

    lower_layer {
      display-name = "Lower";
      bindings = <
        // キーバインディング
      >;
    };

    // 他のレイヤー...
  };
};
```

---

## よく使うキーコード

### 基本キー

| キーコード | 説明 |
|-----------|------|
| `&kp A` - `&kp Z` | アルファベット |
| `&kp N0` - `&kp N9` | 数字 |
| `&kp SPACE` | スペース |
| `&kp RET` | Enter |
| `&kp BSPC` | Backspace |
| `&kp TAB` | Tab |
| `&kp ESC` | Escape |

### 修飾キー

| キーコード | 説明 |
|-----------|------|
| `&kp LSHFT` | 左Shift |
| `&kp LCTRL` | 左Ctrl |
| `&kp LALT` | 左Alt |
| `&kp LGUI` | 左GUI（Cmd/Win） |
| `&kp RSHFT` | 右Shift |

### レイヤー

| キーコード | 説明 |
|-----------|------|
| `&mo 1` | Layer 1を押している間有効 |
| `&mo 2` | Layer 2を押している間有効 |
| `&lt 1 SPACE` | 押すとLayer 1、タップでSpace |
| `&trans` | 透過（下のレイヤーのキーを使用） |

### 記号

| キーコード | 記号 |
|-----------|------|
| `&kp MINUS` | `-` |
| `&kp EQUAL` | `=` |
| `&kp LBKT` | `[` |
| `&kp RBKT` | `]` |
| `&kp BSLH` | `\` |
| `&kp SEMI` | `;` |
| `&kp SQT` | `'` |
| `&kp GRAVE` | `` ` `` |
| `&kp COMMA` | `,` |
| `&kp DOT` | `.` |
| `&kp FSLH` | `/` |

### Bluetooth

| キーコード | 説明 |
|-----------|------|
| `&bt BT_SEL 0` | プロファイル0に切り替え |
| `&bt BT_SEL 1` | プロファイル1に切り替え |
| `&bt BT_CLR` | 現在のプロファイルをクリア |

### IME（日本語入力）

| キーコード | 説明 |
|-----------|------|
| `&kp LANG1` | かな（IME ON） |
| `&kp LANG2` | 英数（IME OFF） |

---

## 参考リンク

- [ZMK公式ドキュメント](https://zmk.dev/docs)
- [ZMKキーコード一覧](https://zmk.dev/docs/codes)
- [ZMK Behaviors](https://zmk.dev/docs/behaviors)
- [元リポジトリ](https://github.com/a741725193/zmk-sofle)
- [ZMK Studio](https://zmk.studio)
