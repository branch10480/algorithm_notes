# Union-Find

**Union-Find** とは、**グループ分けを管理するデータ構造** であり、以下のクエリを高速に処理するものです。

N個の要素 0, 1, ..., N-1 を扱うものとし、初期状態ではこれらがすべて別々のグループに属するものとします。

- **issame(x, y)**: 要素x, yが同じグループに属するかどうかを調べる
- **unite(x, y)**: 要素xを含むグループと、要素yを含むグループとを併合する

# Union-Find の仕組み

Union-Find は1つ1つのグループが根付き木を構成するようにして実現できます。

ヒープや二分探索木とは異なり、二分木である必要はありません。

# Union-Find の root 関数

- **root(x)**: 要素xを含むグループ（根付き木）の根を返す

やっていることは頂点xから辿っていき、根に到達したらそれを返すということです。

root(x) の計算量は `O(h)` です。（hは根付き木の高さ）

この root(x) を用いて、上に上げたクエリを処理できます。

|クエリ|処理|
|---:|:---|
|issame(x, y)|root(x) と root(y) が等しいか調べる|
|unite(x, y)|rx = root(x), ry = root(y) として、頂点rxが頂点ryの子頂点となるようにつなぐ|

いずれも root(x) 関数を用いることから計算量は `O(h)` となります。

# Union-Find の計算量を削減する工夫

- union by size (または union by rank)
- 経路圧縮

## union by size

比較的簡単に実現でき、かつ汎用性が高い方法です。

unite(x, y) の実現では以下の2通りが可能であることに着目します。

rx = root(x), ry = root(y) とします。

1. rx を ry の子頂点とする
2. ry を rx の子頂点とする

union by size では、**頂点数がより小さい方の根付き木の根が、小頂点となるようにつなぐ** ということをします。

こうすることで、計算量を `O(logN)` にすることができるのです。

証明は略。

この Union-Find における **サイズが小さい方のデータ構造を大きい方に併合する** という手法は一般的に使えるテクニックなので心に留めておくこと！

## 経路圧縮

union by size に加え、**経路圧縮** をすることで計算量をさらに `O(a(N))` にまで減らすことができます。

union by size は **unite(x, y)についての工夫** でしたが、経路圧縮は **issame(x, y)** についての工夫です。

経路圧縮なしの root(x) は以下のように実装できます。

> parent: [Int] について、頂点 x の親を parent[x] とします<br>
> また、parent[x] == -1 のときは頂点 x が根で有ることを示します

```swift
func root(x: Int) -> Int {
  if parent[x] == -1 {
    return x                // parent[x] == -1 は頂点のときなので x を返す
  }
  else {
    return root(parent[x])  // 親に対して再帰呼び出し
  }
}
```

次に root(x) に経路圧縮するときのことを考えます。

具体的には **xから上へと進んでいって根に到達するまでの経路中の頂点に対し、その親を根に張り替える** という作業です。

複雑そうに聞こえますが、以下のように簡単に実装できます。

```swift
func root(x: Int) -> Int {
  if parent[x] == -1 {
    return x              // x が根の場合は x を返す
  }
  else {
    parent[x] = root(parent[x])   // x の親 parent[x] を根に設定する
    return parent[x]
  }
}
```

## Union-Find の最終形

```swift
/// Union-Find
public struct UnionFind {
  /// 各頂点の親頂点の番号を示す
  private var parents: [Int]
  /// 各頂点の属する根付き期の頂点数を表す
  private var size: [Int]
  
  public init(vCount n: Int) {
    self.parents = Array(repeating: -1, count: n)
    self.size = Array(repeating: 1, count: n)
  }
  
  /// 根を求める
  public mutating func root(vIndex v: Int) -> Int {
    if parents[v] == -1  { return v }
    parents[v] = root(vIndex: parents[v])
    return parents[v]
  }
  
  /// v1 と v2 が同じグループに属するかどうか（根が一致するかどうか）
  public mutating func isSame(_ v1: Int, _ v2: Int) -> Bool {
    return root(vIndex: v1) == root(vIndex: v2)
  }
  
  /// x を含むグループと y を含むグループとを併合する
  public mutating func unite(_ v1: Int, _ v2: Int) {
    // 根を求める
    var rootV1 = root(vIndex: v1)
    var rootV2 = root(vIndex: v2)
    
    // すでに同じグループだった場合ここで抜ける
    guard rootV1 != rootV2 else { return }
    
    // union by size （rootV2 側のサイズが小さくなるようにする）
    if size[rootV1] < size[rootV2] {
      swap(&rootV1, &rootV2)
    }
    
    // 小さい方の根付き木を大きい方の根につなげる
    parents[rootV2] = rootV1
    size[rootV1] += size[rootV2]
  }
  
  /// v を含むグループのサイズ
  public mutating func size(_ v: Int) -> Int {
    return size[root(vIndex: v)]
  }
}

// MARK: - Main
func main() {
  var uf = UnionFind(vCount: 7)   // {0}, {1}, {2}, {3}, {4}, {5}, {6}
  
  uf.unite(1, 2)                  // {0}, {1, 2}, {3}, {4}, {5}, {6}
  uf.unite(2, 3)                  // {0}, {1, 2, 3}, {4}, {5}, {6}
  uf.unite(5, 6)                  // {0}, {1, 2, 3}, {4}, {5, 6}
  
  print(uf.isSame(1, 3))          // true
  print(uf.isSame(2, 5))          // false
  
  uf.unite(1, 6)                  // {0}, {1, 2, 3, 5, 6}, {4}
  
  print(uf.isSame(2, 5))          // true
}

```

## 無向グラフにおける連結成分個数を数える問題

この問題は **Union-Find を使い、連結成分をグループとして扱う** ことで解くことができます。

各辺 `e=(u, v)` に対し、 `unite(u, v)` を繰り返し、これにより Union-Find に含まれる根付き木の個数を求めるを求める問題へと帰着できます。

Union-Find 中の **根付き木の根となっている頂点の個数** を数えます。

具体的には `root(x) == x` となる x を数えます。

```swift
// 頂点数
let vCount = N
// 辺数
let edges: [(Int, Int)] = []
let eCount = edges.count

// Union-Find を要素数 N で初期化
var uf = UnionFind(vCount: N)

// 各辺に関する処理
for i in 0..<M {
  let e = edges[i]
  uf.unite(e.0, e.1)    // a を含むグループと b を含むグループとを併合する
}

// 集計
var result = 0
// 頂点の数だけ処理
for i in 0..<N {
  if uf.root(i) == i {
    result += 1
  }
}
print("連結成分の個数は \(result) 個")
```

# まとめ

Union-Find は **グループを効率的に管理するデータ構造** でした。

グラフに関する問題の多くは Union-Find によって解くことができます。

