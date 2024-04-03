---
title: Linuxにおけるファイルとディレクトリ
tags:
  - Bash
  - Linux
  - ファイル操作
  - ディレクトリ
private: false
updated_at: '2023-07-18T20:17:22+09:00'
id: dee59e31e158ae1d5218
organization_url_name: null
slide: false
ignorePublish: false
---
# 参考書籍
[新しいLinuxの教科書](https://www.amazon.co.jp/%E6%96%B0%E3%81%97%E3%81%84Linux%E3%81%AE%E6%95%99%E7%A7%91%E6%9B%B8-%E4%B8%89%E5%AE%85-%E8%8B%B1%E6%98%8E/dp/4797380942/ref=sr_1_1?adgrpid=117229375656&hvadid=655144332605&hvdev=c&hvqmt=e&hvtargid=kwd-1152146940662&hydadcr=21814_13461165&jp-ad-ap=0&keywords=%E6%96%B0%E3%81%97%E3%81%84linux%E3%81%AE%E6%95%99%E7%A7%91%E6%9B%B8&qid=1688622508&sr=8-1)

# 動作環境
+ Windows11
+ Oracle VM VirtualBox
+ CentOS 7

# ファイル
+ Linux では Windows や MacOS X と同様に**情報（データ）は「ファイル」として扱われる**。
+ Linux ではハードディスクやキーボード、 Linuxカーネルもファイルが割り当てられており、**すべてがファイルとして表現されている**。

# ディレクトリ
+ ディレクトリとはファイルを整理する入れ物のことである。
+ Windows や MacOS X では「**フォルダ**」と呼ばれるものと同じである。
+ あるディレクトリの中にあるディレクトリを**サブディレクトリ**（または**子ディレクトリ**）といい、逆にあるディレクトリから見て1つ上にあるディレクトリを**親ディレクトリ**という。
<details><summary> ディレクトリ構造図の例</summary>

> `fruit`
　├ `apple`
　│  　└ `date`　
　│
　└ `orange`
>
>この例でみると`apple`ディレクトリのサブディレクトリは`data`ディレクトリ、親ディレクトリは`fruit`ディレクトリとなる。

</details>

### パス
+ あるファイルを指し示す情報のこと。
+ 各ディレクトリは`/`で区切る。（Windowsでは`\`）

</details>

#### 絶対パスと相対パス
+ 絶対パス（またはフルパス）とは、**[ルートディレクトリ][語句補足]を起点としたパス**のことである。
+ 相対パスとは、**[カレントディレクトリ][語句補足]を起点として表記されるパス**のことである。
<details><summary>パスの例</summary>

> `/`
└ `home`
&emsp;&emsp; └ `sumisumi` &emsp;&emsp; ← <strong>カレントディレクトリ</strong>
&emsp;&emsp;&emsp;&emsp;&emsp; └ `work`
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; └ `file`
> <br>
> + `file`の絶対パスは`/home/sumisumi/work/file`となり、<br>相対パスは`./work/file`もしくは`work/file`となる。
> + `home`の相対パスは`..`となり、`/`の相対パスは`../..`となる。
> +  `.`はカレントディレクトリを表し、`..`は親ディレクトリを表す。

</details>

## 語句補足
<dt>ルートディレクトリ</dt>
<dd>
<strong>ディレクトリ構造における一番上のディレクトリ</strong>のこと。<br>
全てのファイルとディレクトリは、親をたどっていくと最後にこのルートディレクトリにたどり着く。
</dd>

<dt>カレントディレクトリ</dt>
<dd>
今現在、<strong>自分が位置しているディレクトリ</strong>のこと。<br>
<code>pwd</code>コマンドで確認でき、<code>cd</code>コマンドで変更できる。。
</dd>

# 終わりに
ディレクトリとフォルダが同義であると認識することで理解しやすかった。

次はディレクトリとファイルに関するコマンドをまとめたいと思う。

[**次回**](https://qiita.com/sumisumi2000/items/1a0d561fe4c61f961452)

何かご指摘がありましたら、コメントしていただけると幸いです。
最後までお読みいただきありがとうございました！

[語句補足]: #語句補足
