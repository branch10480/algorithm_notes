# フィボナッチ数列

再帰呼び出しで再起関数を複数呼び出すパターンが **フィボナッチ数列の第N項** を求めるアルゴリズムです。

## フィボナッチ数列の第N項を求める

```swift
while true {
  let n = readLineInt()
  func fibo(index n: Int) -> Int {
    // ベース処理
    if n == 1 {
      return 0
    }
    if n == 2 {
      return 1
    }
    return fibo(index: n - 2) + fibo(index: n - 1)
  }
  print(fibo(index: n), "at \(n)")
}
```

### 結果

```shell
Please input: Please input: 3
1 at 3
Please input: Please input: 1
0 at 1
Please input: Please input: 2
1 at 2
Please input: Please input: 3
1 at 3
Please input: Please input: 4
2 at 4
Please input: Please input: 5
3 at 5
Please input: Please input: 6
5 at 6
Please input: Please input: 7
8 at 7
```

## 上記の再帰関数を用いるデメリット

上記の再帰処理を用いる解法だと、同じ計算を複数回行うことで処理量が多くなっています。

具体的には、 `fibo(6)` を実行した場合、 `fibo(4)` は 2回、 `fibo(2)` に至っては 5回処理を実行しています。

重複処理を効率的に行うために、解のメモを取るようにします。（いわゆる**キャッシュを用いる方法**です。）

```swift
while true {
  let n = readLineInt()
  var cache: [Int: Int] = [:]
  func fibo(index n: Int) -> Int {
    // ベース処理
    if n == 1 {
      return 0
    }
    if n == 2 {
      return 1
    }
    if let value = cache[n] {
      return value
    }
    let result = fibo(index: n - 2) + fibo(index: n - 1)
    cache[n] = result
    return result
  }
  print(fibo(index: n), "at \(n)")
}
```