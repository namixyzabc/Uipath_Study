# UiPath設定ファイル

## 1. 設定ファイルを使う理由

### 従来の問題点
- メールアドレスやファイルパスを直接変数に書き込み
- 変更時にはUiPathのライセンス保有者がXamlファイルを直接修正する必要
- **非効率的で時間がかかる**

### 設定ファイル使用後のメリット
- 変更が必要な情報を外部ファイルに保存
- **ファイル更新のみで簡単に変更可能**
- 開発者以外でもメンテナンス可能

## 2. 設定ファイルに保存すべき情報

| 変更頻度 | 情報の種類 | 具体例 |
|---------|-----------|--------|
| **頻繁** | メール設定 | 送信先メールアドレス |
| **頻繁** | 処理制御 | 実施要否フラグ、リトライ回数 |
| **時々** | システム設定 | ファイルパス、URL |

## 3. 設定ファイルの構成

### 基本的な3列構成

| 列名 | 役割 | 例 |
|------|------|-----|
| **Key** | UiPathで値を取得する際の識別子 | `MAIL_ADDRESS` |
| **Value** | 実際の設定値（変更対象） | `user@company.com` |
| **Description** | 設定内容の説明 | 処理結果送信先メールアドレス |

### 設定ファイル例（Excel）

| Key | Value | Description |
|-----|-------|-------------|
| MAIL_ADDRESS | user@company.com | 処理結果送信先メールアドレス |
| RETRY_COUNT | 3 | エラー時のリトライ回数 |
| FILE_PATH | C:\Data\input.xlsx | 入力ファイルのパス |
| SYSTEM_URL | https://system.company.com | システムのURL |

> ⚠️ **セキュリティ注意点**  
> パスワードの記載は組織のセキュリティポリシーに従って判断してください

## 4. UiPathでの読み込み手順

### ステップ1: Excelファイルの読み込み
```
「Excelアプリケーションスコープ」
　└「範囲を読み込み」
　　 └ 結果をDataTable型変数に格納
```

### ステップ2: Dictionary変数の初期化
```
「代入」アクティビティ
　└ Dictionary変数を作成
```

### ステップ3: データの変換
```
「繰り返し（データテーブルの各行）」
　└「代入」アクティビティ
　　 ├ 左辺: dicSettings(CurrentRow("Key").ToString.Trim)
　　 └ 右辺: CurrentRow("Value").ToString.Trim
```

### ステップ4: データの使用
```
設定値の取得方法:
dicSettings("KEY名") → 対応するValue値が返される
```

## 5. 運用のベストプラクティス

### ファイル構成のコツ

| 推奨方法 | 説明 | 例 |
|----------|------|-----|
| **種類別グループ化** | 関連する設定をまとめる | システムA（URL、ID、パスワード）をまとめて記載 |
| **シート分割** | 情報量が多い場合はシートを分ける | メール設定シート、ファイルパス設定シート |

### 管理上の注意点
- ✅ Value列のみを変更する
- ✅ Key名は分かりやすく統一的な命名にする
- ✅ Description列で設定内容を明確に説明する
- ⚠️ ファイルのバックアップを定期的に取得する

この方法により、**効率的で保守性の高いRPA運用**が可能になります。
