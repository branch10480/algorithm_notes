# 連結リスト

**ポインタ** と呼ばれる矢印によって一列に繋いだものをいいます。

繋いだもののうち、1つ1つの要素のことを **ノード** と呼びます。

何もないことを示す **ダミーノード** も利用します。<br>またこのダミーノードのことを **番兵** と呼ぶ使い方もあります。

連結リストでは各要素が全体の何番目かという管理をしません。

# Swift における連結リスト

Swift では以下のように連結リストを定義できます。

```swift
class LinkedList<T> {
  var next: LinkedList<T>
  var value: T

  init(next: LinkedList<T>, value: T) {
    self.next = next
    self.value = value
  }
}
```

## 操作

### 挿入

ある特定の要素の直後に他の要素を挿入する 操作は以下のようになります。

```swift
// 連結リスト
// a -> b -> c
let c = LinkedList(
  next: nil,
  value: "c"
)
let b = LinkedList(
  next: c,
  value: "b"
)
let a = LinkedList(
  next: b,
  value: "a"
)

func insert<T>(_ node: LinkedList<T>, after: LinkedList<T>) {
  let next = after.next
  after.next = node
  node.next = next
}

func printLL<T>(startAt node: LinkedList<T>) {
  print("\(node.value)")
  guard let next = node.next else {
    return
  }
  printLL(startAt: next)
}

let aa = LinkedList(next: nil, value: "aa")
insert(aa, after: a)
printLL(startAt: a)
```

### 削除

連結リストにおいて特定のノードを削除できるようにするには、そのノードの前のノードを取得できるようにします。

```swift
func main1() {

  typealias Node = LinkedList<String>

  // NilNode の定義 - 番兵（Guard）
  let nilNode = Node(value: "nil", isNilNode: true)

  // 連結リストの初期化
  // a -> b -> c
  let values = [
    "a",
    "b",
    "c",
  ]
  var prevNode = nilNode
  for i in 0..<values.count {
    let newNode = Node(prev: prevNode, next: nil, value: values[i])
    prevNode.next = newNode
    prevNode = newNode

    // 循環参照を避けるために敢えて最後のノードを nilNode に繋げない
//    if i == values.count - 1  {
//      newNode.next = nilNode
//      nilNode.prev = newNode
//    }
  }

  func insert<T>(_ node: LinkedList<T>, after: LinkedList<T>) {
    let next = after.next
    after.next = node
    node.prev = after

    node.next = next
    next?.prev = node
  }

  func remove<T>(_ node: LinkedList<T>) {
    guard let prevNode = node.prev, let nextNode = node.next else {
      return
    }
    prevNode.next = nextNode
    nextNode.prev = prevNode
  }

  func printLL<T>(startAt node: LinkedList<T>) {
    guard let next = node.next, !next.isNilNode else {
      return
    }
    print("\(next.value)")

    // Recursive Call.
    printLL(startAt: next)
  }

  // Print node's values.
  printLL(startAt: nilNode)

  // Remove the b node.
  remove(nilNode.next!.next!)
  print("--- Removal completed! ---")

  // Show values again.
  printLL(startAt: nilNode)

}

class LinkedList<T> {
  weak var prev: LinkedList<T>?
  var next: LinkedList<T>?
  var value: T
  let isNilNode: Bool

  init(
    prev: LinkedList<T>? = nil,
    next: LinkedList<T>? = nil,
    value: T,
    isNilNode: Bool = false
  ) {
    self.prev = prev
    self.next = next
    self.value = value
    self.isNilNode = isNilNode
  }
}
```

## 得意なこと

[配列](array.md) で弱点だった

- 要素 x の直後に y を追加
- 要素 x の削除

の処理を計算量 `O(1)` で実行できます。

## 苦手なこと

配列では i 番目の要素へのアクセスが高速（`O(1)`）でしたが、連結リストでは逆に時間がかかります。（`O(N)`）

## 配列と連結リストの使いやすさ

一般的な実務のプログラミングでは配列がよく使われます。

それは i 番目の要素に頻繁にアクセスする必要があるからです。

一方の連結リストは普段使う機会が少ないかもしれませんが、**特定の場面では絶大な威力を発揮します**。

連結リストはそれ単体で使うというよりも、データ構造の部品として用いられるケースが多いです。

また、配列、結合リストともに中に特定の要素が存在するかを判定する場合の計算量は `O(N)` となり時間がかかります。（線形探索での探索）

[ハッシュテーブル](hash_table.md) を用いると計算量 `O(1)` で検索処理を実行できます。
