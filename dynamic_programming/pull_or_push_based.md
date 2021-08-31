# push based と pull based

[push based と pull based](dynamic_programming/pull_or_push_based.md) で見てきた緩和の方針は **頂点 i に向かってくる遷移を考える** ということでした。この形式を **pull based (貰う遷移形式)** といいます。

対して **push based** 、つまり **頂点 i から伸びていく遷移を考える** 形式で考えることもできます。

pull based では dp[i-2] や dp[i-1] の値が確定している時に dp[i] の値を更新していましたが、push based においては dp[i] が確定しているときにその値を用いて dp[i+1] や dp[i+2] の値を更新します。

```swift
// Pull Based
// dp[i-2], dp[i-1] が確定している時に dp[i] を求める
for i in 1..<N {
  chmin(a: &dp[i], b: dp[i-1] + abs(h[i] - h[i-1]))
  if i > 1 {
    chmin(a: &dp[i], b: dp[i-2] + abs(h[i] - h[i-2]))
  }
}

// Push Based
// dp[i] が確定している時に dp[i+1], dp[i+2] を求める
for i in 0..<N-1 {
  chmin(a: &dp[i+1], b: dp[i] + abs(h[i+1] - h[i]))
  if i+2 < N-1 {
    chmin(a: &dp[i+2], b: dp[i] + abs(h[i+2] - h[i]))
  }
}
```

pull based も push based も各辺に対して緩和処理を1回ずつ行っていることになります。

この緩和において重要なポイントは以下です。

> **緩和処理のポイント** <br>
> 頂点 u から 頂点 v に遷移する辺に関する緩和処理を成立させるためには、 dp[u] の値が確定していることが重要<br>
> つまり、始点となる頂点のコストが確定している必要があるということです


