---
title: Let’s EncryptでHTTPS化したけれど
author: Keiji Ariyama
type: post
date: 2015-11-12T05:50:15+00:00
url: /2015/11/lets-encrypt.html
categories:
  - メモ
  - 雑記

---
[Let&#8217;s Encrypt][1]のクローズドベータ。このドメインでもインビテーションをもらっていたのでさっそくHTTPS化したわけですが、HTTPSにするとURLが変わるから、ソーシャル系の「いいね！」とかの値が０になるんですね。

調べてみるとこんな記事が。

[HTTPからHTTPSへの変更時にソーシャルボタンのカウント数を(見た目上)引き継ぐ方法][2]

うん。諦めた。

これからコンテンツ系のサイト始める人は、最初からHTTPSに対応しておくのが無難ですね。

 [1]: https://letsencrypt.org/
 [2]: http://www.ayudante.jp/column/2015-07-01/17-24/