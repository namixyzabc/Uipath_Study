# ExcelVBAのFunction（関数）について

## Functionとは何か

**Function（関数）**は、特定の処理を実行して**値を返す**プログラムの部品です。VBAでは、計算や処理を行った結果を呼び出し元に返すことができます。

## Functionの基本構造

```vba
Function 関数名(引数1 As データ型, 引数2 As データ型) As 戻り値のデータ型
    '処理内容
    関数名 = 戻り値
End Function
```

### 各部分の説明
- **関数名**：作成する関数の名前
- **引数**：関数に渡すデータ（省略可能）
- **データ型**：変数や値の種類（Integer：整数、String：文字列など）
- **戻り値**：関数が処理結果として返す値

## 具体的な例

### 例1：2つの数を足し算する関数

```vba
Function 足し算(数値1 As Integer, 数値2 As Integer) As Integer
    足し算 = 数値1 + 数値2
End Function
```

この関数の使用例：
```vba
Sub テスト()
    Dim 結果 As Integer
    結果 = 足し算(5, 3)
    MsgBox "結果は " & 結果 & " です"  '「結果は 8 です」と表示
End Sub
```

### 例2：消費税を計算する関数

```vba
Function 消費税計算(価格 As Double) As Double
    消費税計算 = 価格 * 0.1
End Function
```

## Functionの重要な特徴

### **戻り値を持つ**
- Functionは必ず値を返します
- 戻り値は`関数名 = 値`の形で設定します

### **Excelの数式で使用可能**
VBAで作成したFunctionは、Excelのセルに直接入力して使用できます。

例：セルに`=足し算(A1, B1)`と入力すると、A1とB1の値を足した結果が表示されます。

## 引数の種類

### 値渡し（ByVal）
```vba
Function テスト(ByVal 値 As Integer) As Integer
    値 = 値 + 10
    テスト = 値
End Function
```
- **値渡し**：元の変数の値は変更されません
- デフォルトではByValが適用されます

### 参照渡し（ByRef）
```vba
Function テスト(ByRef 値 As Integer) As Integer
    値 = 値 + 10
    テスト = 値
End Function
```
- **参照渡し**：元の変数の値も変更されます

## 実践的な例

### 文字列を処理する関数

```vba
Function 姓名結合(姓 As String, 名 As String) As String
    姓名結合 = 姓 & " " & 名
End Function
```

### 条件分岐を含む関数

```vba
Function 成績判定(点数 As Integer) As String
    If 点数 >= 80 Then
        成績判定 = "優"
    ElseIf 点数 >= 60 Then
        成績判定 = "良"
    Else
        成績判定 = "不可"
    End If
End Function
```

## FunctionとSubの違い

| 項目 | Function | Sub |
|------|----------|-----|
| **戻り値** | あり | なし |
| **Excelでの使用** | 可能 | 不可能 |
| **主な用途** | 計算・変換処理 | 操作・実行処理 |

## 注意点とベストプラクティス

### **エラーハンドリング**
```vba
Function 安全な割り算(分子 As Double, 分母 As Double) As Double
    If 分母 = 0 Then
        安全な割り算 = 0  'エラーを避けるため0を返す
    Else
        安全な割り算 = 分子 / 分母
    End If
End Function
```

### **適切な命名**
- 関数名は処理内容が分かる名前にする
- 日本語も使用可能ですが、英語での命名も推奨

### **データ型の指定**
引数と戻り値のデータ型を明確に指定することで、予期しないエラーを防げます。

