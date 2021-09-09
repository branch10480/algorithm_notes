# 一般化した二分探索法1

一般化を考えた場合、次のようなことができる手法を考えます。

> 各整数 x について、 true / false の2値で判定される条件 P が与えられていて、ある整数 l, r (l < r) が存在して以下が成立するものとします。
>
> - P(l): false
> - P(r): true
> - ある整数 M (l < M <= r) が存在して、 x<M なる x に対して P(x): true が成り立つ
>
> このとき、 D = r-l として、二分探索法は M を O(logN) の計算量で求めることができるアルゴリズムと言えます。

## 解答への足掛かり

最終的には `r - l == 1` と隣り合うまで判定します。

```swift
let mid = (right + left) / 2
if P(i: mid) {
  right = mid
}
else {
  left = mid
}
```

このように left, right の値を変更しながら試行を重ねる時、 **left は常に P(x) が false になる位置に存在し、right は常に true になる位置にある** ことに着目します。

## 解答

```swift
let l = [3,5,8,10,14,17,21,39]
//  let l = [3,5,9,10,14,17,21]
let N = l.count

/// 判定式
func P(i: Int) -> Bool {
  let trueBorder = 21
  return l[i] >= trueBorder
}

/// P(x) == true となる最小の index を返す
func getTrueBorder() -> Int {
  // 二分探索法 (Binary Search) を使用
  var left = 0
  var right = l.count - 1
  while right - left > 1 {
    let mid = (right + left) / 2
    if P(i: mid) {
      right = mid
    }
    else {
      left = mid
    }
  }
  return right
}

let index = getTrueBorder()
print("getTrueBorder():", index)
print("l[\(index)] is", l[index])
```
