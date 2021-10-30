# 挿入ソート

考え方の根本は **左からi枚目までがソートされている状態から、i+1枚目がソートされている状態にする** ということです。

## 実装

```swift
extension Array where Element: Comparable {
  /// 挿入ソート
  mutating func insertionSort() {
    let size = count
    for i in 1..<size {
      let v = self[i]
      var j = i
      while j > 0 {
        if self[j-1] > v {
          self[j] = self[j-1]
          j -= 1
          continue
        }
        break
      }
      self[j] = v
    }
  }
}
```

## 計算量

挿入ソートの最悪の計算量は `O(N^2)` です。

対象の配列がすべて降順になっていた場合に最悪となり、その場合の計算量は

```
1 + 2 + 3 + ... + N-1 + N = N(N-1) / 2
```

となるので `O(N^2)` となります。

## 挿入ソートの性質

- in-place なソート
- stable ソート

