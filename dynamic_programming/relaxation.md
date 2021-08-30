# 動的計画法の諸概念 - 緩和

次の例にあるように、 **配列dpの各値が徐々に小さい値へと更新されていくイメージ**  を抱くことができればニュアンスが掴めたことになります。

緩和処理を実現するための関数 `chmin`
```swift
var dp: [Int] = [0]
print("緩和前", dp)

// 緩和用の関数
// a が inout になっている点が肝
// chmin: Choose Minimum
func chmin(a: inout Int, b: Int) {
  if b < a {
    a = b
  }
}

chmin(a: &dp[0], b: -1)
print("緩和後", dp)
```

### 結果

```shell
緩和前 [0]
緩和後 [-1]
```

## カエル問題の緩和を用いた解法

```swift
// 緩和用の関数
// a が inout になっている点が肝
// chmin: Choose Minimum
func chmin(a: inout Int, b: Int) {
  if b < a {
    a = b
  }
}

let h = [2,9,4,5,1,6,10]
let N = h.count
var dp: [Int] = Array(repeating: Int.max, count: N)
dp[0] = 0

for i in 1..<N {
  chmin(a: &dp[i], b: dp[i-1] + abs(h[i] - h[i-1]))
  if i > 1 {
    chmin(a: &dp[i], b: dp[i-2] + abs(h[i] - h[i-2]))
  }
}

print("Minimum cost is", dp[N-1])
```

### 結果

```shell
Minimum cost is 8
```
