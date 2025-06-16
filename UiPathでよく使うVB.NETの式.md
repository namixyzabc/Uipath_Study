
---

## よく使うVB.NET式

### 1. 文字列変換（`.ToString`）

#### 用途
数値や日付などを文字列に変換する際に使用

#### システム面の説明
- UiPathでは、データ型が異なるとエラーが発生します
- 数値型（Integer）と文字列型（String）は互換性がないため、変換が必要

#### 構文
```vb
変数名.ToString
```

#### 実例
```vb
' 数値を文字列に変換してExcelのセル番号を作成
"A" + rowNumber.ToString  ' 結果: "A5"（rowNumberが5の場合）

' 現在の日時を文字列として取得
DateTime.Now.ToString("yyyy-MM-dd")  ' 結果: "2024-03-15"
```

---

### 2. ファイル一覧取得（`Directory.GetFiles()`）

#### 用途
指定フォルダ内のファイルを一括で処理する際に使用

#### システム面の説明
- `Directory`は.NET Frameworkのクラス
- ファイルシステムにアクセスして、ファイル一覧を配列として返す

#### 構文
```vb
Directory.GetFiles("フォルダパス")
Directory.GetFiles("フォルダパス", "*.xlsx")  ' 特定の拡張子のみ
```

#### 実例
```vb
' すべてのファイルを取得
Directory.GetFiles("C:\MyDocuments\Reports")

' Excelファイルのみを取得
Directory.GetFiles("C:\MyDocuments\Reports", "*.xlsx")

' サブフォルダも含めて検索
Directory.GetFiles("C:\MyDocuments", "*.*", SearchOption.AllDirectories)
```

---

### 3. 文字列検索（`.Contains()`）

#### 用途
文字列の中に特定の文字が含まれているかチェック

#### システム面の説明
- 戻り値は`Boolean`型（TrueまたはFalse）
- 大文字小文字を区別する

#### 構文
```vb
対象文字列.Contains("検索したい文字列")
```

#### 実例
```vb
' 基本的な使用例
fileName.Contains("請求書")  ' ファイル名に「請求書」が含まれるかチェック

' 条件分岐での使用例
If customerName.Contains("株式会社") Then
    ' 法人の場合の処理
End If

' 大文字小文字を無視する場合
customerName.ToUpper.Contains("TOYOTA")
```

---

### 4. 文字列抽出（`.Substring()`）

#### 用途
文字列から特定の部分を切り出す

#### システム面の説明
- インデックスは0から開始（最初の文字が0番目）
- 指定した位置から指定した長さだけ文字を取得

#### 構文
```vb
文字列.Substring(開始位置)          ' 指定位置から最後まで
文字列.Substring(開始位置, 文字数)   ' 指定位置から指定文字数分
```

#### 実例
```vb
' ファイル名「20240315_売上報告書.xlsx」から日付部分を抽出
fileName.Substring(0, 8)  ' 結果: "20240315"

' 社員番号「EMP001_田中太郎」から社員番号のみ抽出
employeeData.Substring(0, 6)  ' 結果: "EMP001"

' 拡張子を除いたファイル名を取得
fileName.Substring(0, fileName.LastIndexOf("."))
```

---

### 5. 文字列分割（`.Split()`）

#### 用途
区切り文字で文字列を分割して配列にする

#### 構文
```vb
文字列.Split("区切り文字"c)
```

#### 実例
```vb
' CSV形式のデータを分割
"田中,25,営業部".Split(","c)  ' 結果: {"田中", "25", "営業部"}

' パスを分割
"C:\Users\Documents\file.txt".Split("\"c)
```

---

### 6. 文字列置換（`.Replace()`）

#### 用途
文字列内の特定の文字を別の文字に置き換える

#### 構文
```vb
文字列.Replace("置換前", "置換後")
```

#### 実例
```vb
' スペースをアンダースコアに置換
customerName.Replace(" ", "_")

' 全角を半角に変換
phoneNumber.Replace("０", "0").Replace("１", "1")
```

---

## VB.NET式の調べ方・学習方法

### 1. UiPath公式リソース

#### UiPath Academy
- **URL**: [academy.uipath.com](https://academy.uipath.com)
- **推奨コース**: 
  - 「RPA Developer Foundation」
  - 「Variables, Arguments, and Control Flow」
- **特徴**: 最新の情報と実践的な例が豊富

#### UiPath公式ドキュメント
- **URL**: [docs.uipath.com](https://docs.uipath.com)
- **活用法**: アクティビティ別にVB.NET式の例が記載

### 2. 式エディターの活用（AI機能）

#### 使用方法
1. **式エディターを開く**: アクティビティの「fx」ボタンをクリック
2. **自然言語で入力**: 「variable1に"合格"が含まれる」と日本語で記述
3. **変換実行**: 矢印ボタンをクリック
4. **結果確認**: `variable1.Contains("合格")`のようなVB.NET式が生成される

#### 活用例
```
入力: "今日の日付を取得したい"
出力: DateTime.Now.ToString("yyyy/MM/dd")

入力: "文字列の長さを調べたい"
出力: stringVariable.Length
```

### 3. コミュニティリソース

#### UiPath Community Forum
- **URL**: [forum.uipath.com](https://forum.uipath.com)
- **活用法**: 具体的な問題で検索、質問投稿

#### Qiita
- **検索方法**: 「UiPath VB.NET」「UiPath 式」で検索
- **特徴**: 日本語での解説記事が豊富

#### Stack Overflow
- **検索方法**: 「UiPath」「VB.NET expression」で検索
- **特徴**: 英語だが情報量が多い

---

## よくあるエラーと対処法

### 1. データ型エラー
```vb
' ❌ エラーになる例
"セル番号: " + 5

' ✅ 正しい例
"セル番号: " + 5.ToString
```

### 2. Null参照エラー
```vb
' ❌ エラーになる可能性
stringVariable.Contains("検索文字")

' ✅ 安全な書き方
If stringVariable IsNot Nothing AndAlso stringVariable.Contains("検索文字") Then
```

### 3. インデックスエラー
```vb
' ❌ 文字列の長さを超えてアクセス
shortString.Substring(10, 5)

' ✅ 長さをチェック
If shortString.Length >= 15 Then
    result = shortString.Substring(10, 5)
End If
```

---

## 実践的な応用例

### ファイル処理の完全例
```vb
' フォルダ内のExcelファイルを処理
For Each filePath In Directory.GetFiles("C:\Reports", "*.xlsx")
    Dim fileName As String = Path.GetFileName(filePath)
    
    ' 日付を含むファイルのみ処理
    If fileName.Contains("2024") Then
        ' 日付部分を抽出
        Dim dateString As String = fileName.Substring(0, 8)
        
        ' ファイル名から部署名を抽出
        Dim parts() As String = fileName.Replace(".xlsx", "").Split("_"c)
        If parts.Length > 1 Then
            Dim department As String = parts(1)
        End If
    End If
Next
```

---

