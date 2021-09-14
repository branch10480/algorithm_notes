# 一般化した二分探索法2

## 例題

> N個の整数 a0, a1, ..., aN-1 と、 N個の整数 b0, b1, ..., bN-1 が与えられます。
> 二組の整数列からそれぞれ1個ずつ整数を選んで和をとります。
> 
> その和として考えられる値のうち、整数 K 以上の範囲内での最小値を求めてください。

## 解答への足掛かり

a0, a1, ..., aN-1 から選ぶ数値を固定されるという考え方をします。<br>
ここではその数値を ai と表現します。

このような条件下で以下を解決するようにしてみます。

> N個の正の整数 b0, b1, ..., bN-1 が与えられます。<br>
> このうち、K-ai 以上の範囲内での最小値を求める。

整数列 b0, b1, ..., bN-1 についてはあらかじめソートしておきます。

このとき、二分探索法を使って K-ai 以上になる境界値を求めます。

## 解答

```swift
let a = [2,3,5]
let b = [1,2,7]
let K = 1

func lower_bound(sortedArray array: [Int], borderValue border: Int) -> Int? {
  var left = 0
  var right = array.count - 1
  while right - left > 1 {
    let mid = (right - left) / 2
    if array[mid] >= border {
      right = mid
    }
    else {
      left = mid + 1
    }
  }
  if array[left] >= border {
    return array[left]
  }
  if array[right] >= border {
    return array[right]
  }
  return nil
}

// 固定する a を決めて、その値で b の選択を試行していく
var minValue: Int?
for i in 0..<a.count {
  let ai = a[i]
  if let bi = lower_bound(sortedArray: b, borderValue: K - ai) {
    let sum = ai + bi
    if let nonNilMin = minValue {
      minValue = min(nonNilMin, sum)
    }
    else {
      minValue = sum
    }
  }
}

if let minValue = minValue, minValue >= K {
  print("Minimum sum is", minValue)
}
else {
  print("Nothing.")
}
```
