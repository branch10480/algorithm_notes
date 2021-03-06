# ハッシュテーブル

整数とは限らない一般的なデータ集合 S の各要素 x に対し、 0 <= h(x) < M を満たす h(x) に対応させることを考えます。

このとき h(x) を **ハッシュ関数（hash function）** と呼びます。

また x のことをハッシュテーブルの **キー（key）** と呼び、ハッシュ関数の値 h(x) のことを **ハッシュ値（hash value）** と呼びます。

そして、S の集合ないのどの x に対してもハッシュ値 h(x) が異なるようなハッシュ関数を **完全ハッシュ関数（perfect hash function）** と呼びます。

ハッシュテーブルでは、

- 挿入
- 削除
- 検索

これらを `O(1)` の計算量で実行できます。

ハッシュテーブルはハッシュ値 h(x) と要素 x を関連づけて値を管理します。

## ハッシュの衝突

要素 x, y について h(x) = h(y) になる場合、ハッシュ値が **衝突（collision）** していると言います。

現実世界では完全ハッシュ関数を作ることは難しく、衝突する場合があります。

衝突対策にはさまざまありますが、**連結リストを使って要素を格納する** のが代表的な方法です.

