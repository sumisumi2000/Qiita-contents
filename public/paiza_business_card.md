---
title: paiza×Qiita Bランク 名刺バインダー管理を解いてみた
tags:
  - Ruby
  - paiza
private: true
updated_at: '2024-08-04T01:52:40+09:00'
id: fbe22248dd921a5d6a0f
organization_url_name: null
slide: false
ignorePublish: false
---

# はじめに

https://paiza.jp/pages/campaign/paiza-qiita

paiza が面白そうなキャンペーンを行っていたのでやってみました。

# 作成したコード＆解説

今回は[名刺バインダー管理](https://paiza.jp/works/mondai/b_rank_skillcheck_archive/name_card)を解いてみました。
表裏に名刺を入れられるファイルが存在し、1 つのファイルには n 個のポケットがついています。
（1 つのファイルには 2n 枚の名刺を収納できる）
そのファイルの m 番目の名刺の裏の番号を求めよという問題です。

### コードを書く前に考えたこと

まず、同じファイルであれば表と裏の総和は一定だということに気づきました。

```
ひとつのファイルにあるポケットが３つの場合

    1 枚目              2 枚目            3 枚目

  1, 2, 3          7,  8,  9        13, 14, 15  （表）
+ 6, 5, 4       + 12, 11, 10      + 18, 17, 16  （裏）
----------      -------------     -------------
  7, 7, 7         19, 19, 19        31, 31, 31  （総和）

ひとつのファイルにあるポケットが5つの場合

               1 枚目                      2 枚目                    3 枚目

   1,  2,  3,  4,  5         11, 12, 13, 14, 15        21, 22, 23, 24, 25 （表）
+ 10,  9,  8,  7,  6       + 20, 19, 18, 17, 16      + 30, 29, 28, 27, 26 （裏）
---------------------      ---------------------     ---------------------
  11, 11, 11, 11, 11         31, 31, 31, 31, 31        51, 51, 51, 51, 51 （総和）
```

このことから

```
m 番の名刺の裏側の番号 = m 番の名刺が収納されているファイルの裏表の総和 - m
```

という式が成り立つと考えました。

### コード

Ruby で以下のようなコードを作成しました。

```ruby
# 入力受け取り
N, M = gets.split.map(&:to_i)

# 1つのファイルに入っている名刺の数
total_business_cards = N * 2

# m 番目の名刺が入っているファイルが何枚目か
file_number = (M / total_business_cards.to_f).ceil

# m 番目の名刺が入っているファイルの表裏の総和を計算
sum = total_business_cards * (file_number - 1) + 1 + total_business_cards * file_number

# 総和から m 番目の数字を引くことで裏側の数字を出力
puts sum - M
```

### コード解説

##### 標準入力受け取り

```ruby
N, M = gets.split.map(&:to_i)
```

この行では `gets` メソッドを使用して標準入力から値を受け取っています。
`String#split` メソッドを使用し、空白区切りで文字列を分割し、配列とします。
（`"3 6"` が入力されると、 `["3", "6"]` となる）

https://docs.ruby-lang.org/ja/latest/method/String/i/split.html

このままでは文字列のままなので `Array#map` メソッドを使用して整数へと変換し、多重代入によって `N` と `M` に値を保存します。

https://docs.ruby-lang.org/ja/latest/method/Array/i/collect.html

https://docs.ruby-lang.org/ja/latest/doc/spec=2foperator.html#multiassign

##### 何枚目のファイルに m 番目の名刺が入っているかを計算

```ruby
# 1つのファイルに入っている名刺の数
total_business_cards = N * 2

# m 番目の名刺が入っているファイルが何枚目か
file_number = (M / total_business_cards.to_f).ceil
```

例えば n = 3 の場合、m が 1 以上 6 以下のときは m 番目の名刺は 1 枚目のファイルに入っています。
n = 5 の場合、 m が 11 以上 20 以下のときは m 番目の名刺は 2 枚目のファイルに入っています。

`Float#ceil` メソッドは `self` と等しい、もしくは `self` より大きな整数の中で最小のものを返します。

https://docs.ruby-lang.org/ja/latest/method/Float/i/ceil.html

`Integer#to_f` メソッドを使用することで、 `M / total_business_cards` の計算結果を `Float` 型で出すようにしています。

https://docs.ruby-lang.org/ja/latest/method/Integer/i/to_f.html

##### m 番目の名刺が入っているファイルの総和を計算

```ruby
# m 番目の名刺が入っているファイルの表裏の総和を計算
sum = total_business_cards * (file_number - 1) + 1 + total_business_cards * file_number
```

1 つのファイルに名刺を x 枚収納できるファイルの、 y 枚目のファイルに収納されている最小の数字は `x * (y - 1) + 1`、最大の数字は `x * y` で求めることができます。

<details><summary>例</summary>

名刺を 6 枚収納できるファイル（n = 3）の 3 枚目のファイル
最小の数字 -> `6 * (3 - 1) + 1 = 13`
最大の数字 -> `6 * 3 = 18`

</details>

##### 出力

```ruby
# 総和から m 番目の数字を引くことで裏側の数字を出力
puts sum - M
```

ここまで求められればあとは出力するだけです。
裏表の総和から `m` の数字を引くことで答えを求めることができます。

# おわりに

問題そのものは特別難しいわけではありませんでしたが、自分自身の思考を言葉で書き表すことに苦労しました。
また paiza のようなオンラインでできるプログラミング問題について解説する機会はあまりなかったので、良い経験になりました。
B ランクを取得して 3 年ほど経っているのでそろそろ A ランクにも挑戦しようと思います！
