# エクセルVBAの2次元配列

## 2次元配列とは

**2次元配列**は、データを表形式（行と列）で格納できるデータ構造です。エクセルのセル範囲のように、縦と横の位置でデータを管理できます。

1次元配列が一列に並んだ箱のようなものなら、2次元配列は**行と列を持つ表のような箱**です。

## 2次元配列の宣言方法

### 基本的な宣言

```vba
Dim 配列名(行の最大値, 列の最大値) As データ型
```

### 具体例

```vba
' 3行4列の2次元配列を宣言
Dim myArray(2, 3) As String

' 動的配列として宣言（後でサイズを決める）
Dim dynamicArray() As Integer
```

**重要なポイント：**
- VBAの配列は**0から始まる**のが既定です
- `(2, 3)`と指定すると、実際は3行4列の配列になります
- 動的配列は後から`ReDim`でサイズを変更できます

## 配列への値の代入

### 個別に代入

```vba
Sub Sample1()
    Dim arr(1, 2) As String
    
    ' 各要素に値を代入
    arr(0, 0) = "A1"
    arr(0, 1) = "B1"
    arr(0, 2) = "C1"
    arr(1, 0) = "A2"
    arr(1, 1) = "B2"
    arr(1, 2) = "C2"
End Sub
```

### ループを使った代入

```vba
Sub Sample2()
    Dim arr(2, 2) As Integer
    Dim i As Integer, j As Integer
    
    ' 二重ループで値を代入
    For i = 0 To 2
        For j = 0 To 2
            arr(i, j) = i * 3 + j + 1
        Next j
    Next i
End Sub
```

## セル範囲から配列への読み込み

エクセルのセル範囲を直接配列に読み込むことができます。

```vba
Sub ReadFromRange()
    Dim arr As Variant
    
    ' A1:C3の範囲を配列に読み込み
    arr = Range("A1:C3").Value
    
    ' 注意：セル範囲から読み込んだ配列は1から始まる
    Debug.Print arr(1, 1) ' A1セルの値
    Debug.Print arr(2, 2) ' B2セルの値
End Sub
```

**重要なポイント：**
- セル範囲から読み込んだ配列は**1から始まります**
- データ型は`Variant`にする必要があります

## 配列からセル範囲への書き込み

```vba
Sub WriteToRange()
    Dim arr(1, 2) As Variant
    
    ' 配列に値を設定
    arr(0, 0) = "商品A"
    arr(0, 1) = 100
    arr(0, 2) = "個"
    arr(1, 0) = "商品B"
    arr(1, 1) = 200
    arr(1, 2) = "個"
    
    ' セル範囲に一括書き込み
    Range("A1:C2").Value = arr
End Sub
```

## 動的配列の活用

サイズが事前に分からない場合は動的配列を使用します。

```vba
Sub DynamicArray()
    Dim arr() As String
    Dim dataCount As Integer
    
    ' データの件数を取得
    dataCount = Range("A1").CurrentRegion.Rows.Count
    
    ' 配列のサイズを決定
    ReDim arr(dataCount - 1, 2)
    
    ' データを配列に読み込み
    Dim i As Integer
    For i = 0 To dataCount - 1
        arr(i, 0) = Cells(i + 1, 1).Value ' A列
        arr(i, 1) = Cells(i + 1, 2).Value ' B列
        arr(i, 2) = Cells(i + 1, 3).Value ' C列
    Next i
End Sub
```

## 実践的な活用例

### 売上データの集計処理

```vba
Sub SalesAnalysis()
    Dim salesData As Variant
    Dim result(9, 2) As Variant
    Dim i As Integer, j As Integer
    
    ' 売上データを配列に読み込み
    salesData = Range("A2:C11").Value ' 商品名、単価、数量
    
    ' 結果配列のヘッダーを設定
    result(0, 0) = "商品名"
    result(0, 1) = "単価"
    result(0, 2) = "売上金額"
    
    ' 売上金額を計算
    For i = 1 To 9
        result(i, 0) = salesData(i, 1) ' 商品名
        result(i, 1) = salesData(i, 2) ' 単価
        result(i, 2) = salesData(i, 2) * salesData(i, 3) ' 売上金額
    Next i
    
    ' 結果をシートに出力
    Range("E1:G10").Value = result
End Sub
```

## 配列のサイズを取得する方法

```vba
Sub GetArraySize()
    Dim arr(3, 4) As Integer
    
    ' 配列の境界を取得
    Debug.Print "行の下限: " & LBound(arr, 1) ' 0
    Debug.Print "行の上限: " & UBound(arr, 1) ' 3
    Debug.Print "列の下限: " & LBound(arr, 2) ' 0
    Debug.Print "列の上限: " & UBound(arr, 2) ' 4
    
    ' 実際の行数・列数
    Debug.Print "行数: " & (UBound(arr, 1) - LBound(arr, 1) + 1) ' 4
    Debug.Print "列数: " & (UBound(arr, 2) - LBound(arr, 2) + 1) ' 5
End Sub
```

## よくある注意点とエラー対策

### インデックス範囲外エラーの回避

```vba
Sub SafeArrayAccess()
    Dim arr(2, 2) As String
    Dim i As Integer, j As Integer
    
    ' 安全な配列アクセス
    For i = LBound(arr, 1) To UBound(arr, 1)
        For j = LBound(arr, 2) To UBound(arr, 2)
            arr(i, j) = "Row" & i & "Col" & j
        Next j
    Next i
End Sub
```

### 配列の初期化確認

```vba
Sub CheckArrayInitialization()
    Dim arr() As Integer
    
    ' 配列が初期化されているかチェック
    On Error Resume Next
    Dim size As Integer
    size = UBound(arr)
    
    If Err.Number <> 0 Then
        Debug.Print "配列は初期化されていません"
        ReDim arr(2, 2)
        Err.Clear
    End If
    On Error GoTo 0
End Sub
```

## パフォーマンスを向上させるコツ

- **セルへの直接アクセスを避ける**：ループ内でCellsやRangeを使わず、配列経由でデータを処理する
- **一括読み込み・書き込みを活用**：Range.Valueを使って配列とセル範囲を一度に操作する
- **適切なデータ型を選択**：用途に応じてVariant、String、Integerなどを使い分ける
