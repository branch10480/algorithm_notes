# ユークリッドの互除法

再帰処理で明快に記述できるアルゴリズムに **ユークリッドの互除法** があります。

## ユークリッドの互除法とは？

2つの整数 m, n の最大公約数 `GCD(m,n)` を求めるアルゴリズムのことです。

以下の性質を利用します。

> **最大公約数の性質**<br>
> m を n で割った時のあまりを r とした時、
> ```
> GCD(m,n) == GCD(n,r)
> ```
> が成立する

## コード

```swift
// TODO: 51 と 15 の最大公約数を求める
let M = 51
let N = 15

func euclidean(m: Int, n: Int) -> Int {
  // ベースケース
  let r = m % n
  if r == 0 {
    return n
  }
  // 最大公約数の性質を利用して次の式にして返す
  return euclidean(m: n, n: r)
}

let gcd = euclidean(m: M, n: N)
print("GCD of \(M) and \(N) is", gcd)

let gcd2 = euclidean(m: N, n: M)
print("GCD of \(N) and \(M) is", gcd2)
```

### 結果
```shell
GCD of 51 and 15 is 3
GCD of 15 and 51 is 3
```