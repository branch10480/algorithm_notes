# 再帰と分割統治法

## 再帰とは？

手続きの中で自分自身を呼び出すことを **再帰呼び出し** といい、再帰呼び出しをする関数を **再帰関数** といいます。

## 再帰関数のテンプレート

```swift
func recursion(case: N) -> Int {
  // ここが一番重要!!
  // このベースケースの処理を行わないと無限実行になってしまう
  if ベースケース {
    return ベースケースの場合の値
  }
  // 次のケースの関数呼び出し
  recursion(N - 1)
  // この関数における結果を返す
  return 結果
}
```

## 例題

> 1 〜 N までの整数の総和を求めてください。

### 解答
```swift
let N = 3

func getSum(startAt num: Int) -> Int {
  if num < 1 {
    return 0
  }
  return num + getSum(startAt: num - 1)
}

let sum = getSum(startAt: N)
print("The sum is", sum)
```