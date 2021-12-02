# マージソート

マージソートは **分割統治法** を活用したソートアルゴリズムです。

**配列を半分に分割し、左右それぞれを再帰的にソートしていき、両者をマージしていく** という操作を繰り返す手法です。

区間aについて、半開区間 [left, right) についてのソートする関数を `MergeSort(a, left, right)` としたとき、

```
mid = (right - left) / 2
```

を得て、`MergeSort(a, left, mid)` 、 `MergeSort(a, mid, right)` をそれぞれ再帰的に呼び出していくというイメージ。

## 左右の返り値の併合イメージ

左右の整列済みの配列を併合するときは以下のような手順を踏みます。

1. 左の配列と右の配列の中身をそれぞれ外部配列にコピーする
2. 左側に対応する外部配列と右側に対応する外部配列がともに空になるまで `左側の最小値` と `右側の最小値` を比較して小さい方を選び、それを抜き出して結果配列に入れていく
3. 左右両方の配列が空になるまで繰り返す

## 実装

```swift
extension Array where Element: Comparable {
  /// マージソート
  mutating func mergeSort() {
    
    self = subSort(self, leftIndex: 0, rightIndex: self.count - 1)
    
    func subSort(_ a: [Element], leftIndex l: Int, rightIndex r: Int) -> [Element] {
      if l == r {
        return [ a[l] ]
      }
      
      // (r - l) / 2 ではないことに注意！
      let mid = (l + r) / 2
      var leftArr = subSort(a, leftIndex: l, rightIndex: mid)
      var rightArr = subSort(a, leftIndex: mid + 1, rightIndex: r)
      
      var result: [Element] = []
      // 左右とも配列要素がなくなるまで繰り返す
      while (!leftArr.isEmpty || !rightArr.isEmpty) {
        if leftAr.isEmpty {
          result.append(rightArr.removeFirst())
          continue
        }
        
        if rightArr.isEmpty {
          result.append(leftArr.removeFirst())
          continue
        }

        if leftArr[0] > rightArr[0] {
          result.append(rightArr.removeFirst())
        } else {
          result.append(leftArr.removeFirst())
        }
      }

      return result
    }
  }
}
```

## 計算量

マージソートは `O(NlogN)` の計算量で動作します。

## マージソートの性質


