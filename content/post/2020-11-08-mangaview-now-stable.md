---
title: MangaView 1.0.0をリリースしました
author: Keiji Ariyama
type: post
date: 2020-11-08T15:00:10+09:00
url: /2020/11/mangaview-now-stable.html
categories:
  - 習作
  - Android

---

　[今年の夏から作っていた](/2020/08/mangaview.html)漫画ビューアー用のカスタムビュー「MangaView」、ついに安定版となる1.0.0をリリースすることができました。

https://github.com/keiji/mangaview

　これまでAndroidアプリはいくつもGoogle Playで公開してきたし、ライブラリを作る仕事はいくつも手がけてきたけれど、公開するライブラリを手がけたのははじめて。パッケージングからBintrayでの配信、jcenterでの公開まで、とても良い勉強になったと思います。

<!--more-->

　ちなみにBintrayは、最初Premiumアカウントで登録していたことに気づかず、トライアル期間の満了でライブラリのデータが消えてしまい、OSSアカウントで登録し直すというトラブルもありました。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">ん？　bintrayのアカウントが消えてる？？？<a href="https://t.co/L2hy4U1y7W">https://t.co/L2hy4U1y7W</a></p>&mdash; Keiji ARIYAMA (Rated KG) (@keiji_ariyama) <a href="https://twitter.com/keiji_ariyama/status/1323788223269593088?ref_src=twsrc%5Etfw">November 4, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">Bintrayのアカウントが消えたのでどうしてかと思ってたんだけど、&quot;Sign In&quot;からGitHubをクリックするとURLに”enterprise”って入っていて、どうやらプレミアム版のアカウントに誘導されている。僕の場合お金を払わなかったのでアカウントが消えた。</p>&mdash; Keiji ARIYAMA (Rated KG) (@keiji_ariyama) <a href="https://twitter.com/keiji_ariyama/status/1323790982232432642?ref_src=twsrc%5Etfw">November 4, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">Bintrayのアカウント作る時は入り口から <a href="https://t.co/DqkFcLsXEO">https://t.co/DqkFcLsXEO</a> を使わないとダメなのか……あとでブログでも書こう</p>&mdash; Keiji ARIYAMA (Rated KG) (@keiji_ariyama) <a href="https://twitter.com/keiji_ariyama/status/1323791234792525824?ref_src=twsrc%5Etfw">November 4, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

　MangaView 1.0.0では、MangaViewをRecyclerViewやViewPager2に入れることができるようにしています。

![mangaview1](https://github.com/keiji/mangaview/blob/gallery/with_viewpager2.gif?raw=true)

　ギリギリまで迷ったのですが、やはり漫画コンテンツと連続して既存のViewを表示したいという需要は大きいようです。漫画を読み終わった流れで「いいね」ボタンを表示したり、SNSでのシェアにつないだりする目的での利用を想定しています。
