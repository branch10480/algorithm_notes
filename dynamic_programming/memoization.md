# 全探索のメモ化としての動的計画法

動的計画法は全探索だと指数時間となるような問題でも、多項式時間アルゴリズムを導けるような方法です。

**メモ化再帰** で動的計画法を実現します。

```swift
let h = [2,9,4,5,1,6,10]
var dp: [Int] = Array(repeating: Int.max, count: h.count)
func minCost(i: Int, h: [Int], dp: inout [Int]) -> Int {
  // ベースケース
  if i < 1 {
    dp[0] = 0
    return dp[0]
  }
  if i == 1 {
    dp[1] = abs(h[0] - h[1])
    return dp[1]
  }
  // i番目に至る経路は i-1 からと i-2 からのケースがある
  let costOfCase1 =  minCost(i: i-1, h: h, dp: &dp) + abs(h[i] - h[i-1])
  let costOfCase2 =  minCost(i: i-2, h: h, dp: &dp) + abs(h[i] - h[i-2])
  dp[i] = min(costOfCase1, costOfCase2)
  return dp[i]
}

print("Minimum cost is", minCost(i: h.count-1, h: h, dp: &dp))
```

dp には i までの最短経路が濃縮された形で格納されており、計算量を大幅に減らすことに貢献しています。

この **探索過程のうちまとめられるところをまとめ、同じ計算を二度行わないようにする** という工夫で計算量を減らしているわけです。

この **探索過程をまとめる** という考え方はまさに動的計画法そのものです。

