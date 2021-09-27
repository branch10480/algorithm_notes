# 単調性

※ 単調性と言う表現は独自のものです。

貪欲法が有効なのはそれまでに最適化されてきたものの次の手を考える時に、現時点で考えた最善の手が一番有効になる場合です。

## 例題

AtCoder Grand Contest 009 A - Multiple Array より

> 0 以上の整数からなる N項の数列 A0, A1, ..., AN-1 と、N個のボタンが与えられます。<br>
> i(=0,1,...,N-1) 個目のボタンを押すと、A0, A1, ..., Ai の値がそれぞれ1ずつ増加します。
> 
> 一方、1以上の整数からなる N項の数列 B0, B1, ..., N-1 が与えられます。ボタンを何回か押して、全ての iに対し、Ai が Bi の倍数になるようにしたいものとします。
> 
> ボタンを押す数の最小値を求めてください。

## 解答への足掛かり

ボタン 0,1,...,N-1 を押す回数をそれぞれ D0, D1, ..., DN-1 とすると、以下の条件を満たすように D0 + D1 + ... + DN-1 の最小値を求める問題という問題と置き換えることができます。

- A0 + (D0 + D1 + ... + DN-1)は B0 の倍数
- A1 + (D1 + D2 + ... + DN-1)は B1 の倍数
- ...
- AN-1 + DN-1 は BN-1 の倍数

## 解答

```swift
var a = [2,12,0,3,10,20]
let b = [2,6,1,3,6,5]

// ボタン i を押す回数を格納する配列を用意する
var d: [Int] = []

for i in 0..<a.count {
let index = a.count - 1 - i
let aItem = a[index]
let bItem = b[index]

    let remainder = aItem % bItem
    if remainder > 0 {
      d.insert(bItem - remainder, at: 0)
    }
    else {
      d.insert(0, at: 0)
    }

    // ボタンを押した回数分だけ加算する
    for j in 0..<index {
      a[j] += d[0]
    }
}

print("Elements of d.")
print(d)
```
