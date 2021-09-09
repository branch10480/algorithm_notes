# 配列の二分探索

配列の二分探索をするためには **配列がソート済みであること** が必須条件になります。

## 例題

> サイズ N=8 のソート済み配列 `a=[3,5,8,10,14,17,21,39]` の中に、値 9 が含まれているかどうかを判定してください。

## 回答への足掛かり

`left=0` `right=N-1` として、 `a[(left + right)/2]: 10` と比べます。

## 解答

```swift
//  let l = [3,5,8,10,14,17,21,39]
let l = [3,5,9,10,14,17,21]
let N = l.count
let target = 9

func contains(_ target: Int) -> Bool {
  // 二分探索法 (Binary Search) を使用
  var left = 0
  var right = N - 1
  var result = false

  while left <= right {
    let i = (right + left) / 2
    if l[i] == target {
      result = true
      break
    }
    else if l[i] < target {
      left = i + 1
      continue
    }
    else if l[i] > target {
      right = i - 1
      continue
    }
  }
  return result
}

if contains(target) {
  print("\(target) は存在します")
} else {
  print("\(target) は存在しません")
}
```

## 配列の二分探索の計算量

計算量は配列要素数を `N` として、 `O(logN)` です。

### 導き方

N (>= 0) について、

```
2^k <= N < 2^k+1
```

となる 0以上の整数 `k` が存在します。

式を変形して以下を得ます。

```
k <= logN < K + 1
```

二分探索では1ステップごとに配列要素が 1/2 になっていきます。

よって、式変換で得た関係を考慮すると、 `k` は最大 `logN` を取ることになります。

したがって、配列の二分探索の計算量は `O(logN)` となります。
