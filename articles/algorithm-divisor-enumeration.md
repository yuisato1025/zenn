---
title: "約数列挙のアルゴリズムの実装と改善"
emoji: "🧠"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["algorithm", "atcoder", "python"]
published: true
---

# 約数列挙とは

正の約数 N において、N の約数を全て求めること。

例) 12 の約数列挙　 → 1,2,3,4,6,12

# 通常の実装(O(N))

### 【方針】

- 1 以上 n 以下の整数を i として順番に見ていき、割り切れたら(n % i == 0)i は n の約数と判定する
- 時間計算量は 1 から n まで全てを見ていくため O(N)になる。

### 【実装】

```python
n = int(input())
result = []
for i in range(1, n+1):
    if n % i == 0:
        result.append(i)
print(*result)
```

上記の実装では、下の問題のように N の数が大きすぎると(N が 10^6 より大きい場合) 2 秒以上実行時間がかかり TLE になってしまうため、改善が必要になる。

https://atcoder.jp/contests/abc180/tasks/abc180_c

# 実装の改善(O(√N))

### 【方針】

- 1 から √n までの整数を i として見ていき、割り切れたら(n % i == 0)i は n の約数と判定する。
- n を i で割った数も同様に n の約数であるため、i と同じ値でなければ追加する。
- 時間計算量は 1 から √n まで全てを見ていくため O(√n)になる。
- d を n の約数の個数とした時、ソートをする関係上、厳密な時間計算量は O(√n + dLog(d))だが、約数の個数より遥かに n の方が大きいと考えられるため O(√n)と考えて問題ない。

### 【実装】

```python
n = int(input())
result = []
for i in range(1, n+1):
    # 半分以上調べる必要はないのでbreak(√nで終わり)
    if i * i > n:
        break

    # 割り切れない = nの約数ではないのでスキップ
    if n % i != 0:
        continue

    result.append(i)

    # N ÷ i も同様に約数であり、iと同じ値(重複を入れる必要はないため)でなければ追加
    if n // i != i:
        result.append(n // i)

# 順番を揃えるためソート
result.sort()
print(*result)
```

ということで、仮に N が 10^12 という大きな値でも O(√10^12) = O(10^6)となり、この実装で TLE を回避することができます。

以上、ではまた 👋
