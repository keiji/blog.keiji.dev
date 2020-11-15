---
title: みんなのコミック
author: Keiji Ariyama
type: post
date: 2015-12-24T15:00:33+00:00
url: /2015/12/mincomi-adventcalendar-25.html
categories:
  - Android
  - みんコミ Advent Calendar

---

----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**

2020/11/09 追記:
みんコミAdvent Calendarその他の知見を元に、漫画表示用カスタムビュー「MangaView」を公開しました。

 * https://github.com/keiji/mangaview
----


## はじめに

　この記事は「[みんコミ Advent Calendar][1]」の25日目の記事です。

　12月1日から24日まで、Webコミック配信サービス「[みんなのコミック][2]」に関する技術的な内容にフォーカスを当てて記事を書いてきました。

　いかがだったでしょうか？

　僕自身、１つのサービスについてこれだけ書くのは初めてのことだったので、新鮮な体験でした。

　また、[23日][3]には、「みんコミ」の中の人とお話しする場を持つこともでき（実際にお会いしたのは22日ですが）、ほとんど思いつきで始めたこときっかけで貴重な経験もできました。

　Advent Calendarをやって本当に良かったです！

<!--more-->

* * *

## 最もよく読まれている記事

　投稿した記事で最もよく読まれているのは、12月20日の「[SurfaceViewをつかってはいけない][4]」です。

　ViewよりSurfaceViewの方が遅い。この事実を知ったときは本当に驚きました。現在関わっているアプリ開発で、まさにSurfaceViewの描画の遅さに悩んでいたからです。

　Advent Calendar用のサンプルアプリがあったおかげで、改修コストの低いコードでじっくりとベンチマークを取ることができました。
  
（もちろん、今回の調査結果は仕事のコードにも反映して、描画速度は格段に向上しています）

## サンプルコードからの改善

　サンプルコードを作るにあたっては、きちんと動作するだけでなく、読みやすい。なぜこうしたのかを説明できることを心がけました（説明していない部分もありますが、聞かれたら答えられます）。

　実を言えば、普段仕事でアプリを開発するときに、`onTouch`から`ScaleGestureDetector`や`GestureDetector`に繋ぐところは、もっと複雑なコードを書いていました。

　今回「[Scrollerのススメ][5]」「[ZoomとかFocusとか聞くと眼鏡っ娘しか想起しない][6]」「[ScaleGestureとScroll][7]」と、記事にしたことで、今よりもシンプルにできることがわかり、プロダクトのコードをすっきりと書き直すことができました。

## サンプルから実際へのハードル

　これまでのサンプルコードは[GitHub][8]に置いてあります。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][8]

　サンプルでは、拡大縮小やスクロール（fling）など一通りの操作ができていますし、ViewPagerを使った画面やDesign Support Libraryを使った上タブ、下タブの画面もそれなりに動いています。

　ただ、言うまでもないことですが、サンプルで動いているのとそれを実際のアプリに組み込んできちんと動作させることは全くの別問題です。

　「みんコミ」アプリは、ユーザー認証のためにサーバーと通信をします。最新のランキング情報を取得したり、ユーザーのお気に入り情報を取得したり……。

　サンプルを現在のアプリにフィットさせるには、それなりの時間が必要であると理解しているので、どうか焦らずに頑張っていただきたいと思います。

## 作品について

　最後に、技術の話からは外れますが「みんコミ」というサービスにとって、Webやアプリは作品を見せる「インターフェース」でしかありません。ユーザーが直接触れるものなので重要であることは間違いありませんが、それだけではサービスが成り立たないのも事実です。

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    10歳になったおかあさんと僕の生活をマンガにしましたよ～。見てくださいね&#10;！　<a href="https://t.co/UfxCfaOYRZ">https://t.co/UfxCfaOYRZ</a>　 <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%81%AA%E3%81%AE%E3%82%B3%E3%83%9F%E3%83%83%E3%82%AF?src=hash">#みんなのコミック</a> <a href="https://t.co/2nHBnFus4n">pic.twitter.com/2nHBnFus4n</a>
  </p>
  
  <p>
    &mdash; 根雪れい.おかあさん(10)と僕。連載中 (@neyuki_rei) <a href="https://twitter.com/neyuki_rei/status/664369017038110720">November 11, 2015</a>
  </p>
</blockquote>

　[根雪れい][9]さんがきっかけで「みんコミ」を知った僕ですが、根雪さんの作品以外にも、みんコミで公開された作品はぜんぶ読んでいます。

　普段、書店で販売している「出来上がった作品」を読む機会が多いので「１話の２ページまでに主人公の紹介とストーリーの立ち上がりが云々」というスキームが、ぼくの脳内に形成されています。
  
　そのせいか最初「みんコミ」の作品を読んだとき「どうにも立ち上がりが遅いな」「展開がもっさりしているな」という印象はありました。

　けれど最初は「いまいちピンとこないな」と思った作品も、回を重ねると面白い部分が出てくる。２話で面白いなと感じて１話を見直すと面白い。次の話はもっと面白くなりそうだ。と、期待できる作品がたくさんあります。

　この１ヶ月を見ているうちに「作品を育てる雑誌」というコンセプトはそういう事なのかと、きちんと伝わってきました。

　Advent Calendarの記事でも、気になっている作品を紹介しました。残念ながらすべての作品を紹介することはできませんでしたが、23日には[眼鏡っ娘が出てくる新連載][10]も始まったので、このAdvent Calendarでみんコミに興味を持った人は、是非とも読んでみてください。

　「[みんコミ][2]」のAndroidアプリは、Google Playからインストールできます（執筆時点の最新バージョンは1.0.3です）。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][11]（公開終了しています）

　また、iOS版のアプリは、iTunes App Atoreからアプリからインストールできます（執筆時点の最新バージョンは1.0.1です）。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/available-on-the-app-store-1345130940.jpg" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][12]（公開終了しています）

　それでは、最後までお読みいただいた皆さん、本当にありがとうございました。

* * *

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、ご了承下さい。
    

* * *

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-23.html
 [4]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-20.html
 [5]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-18.html
 [6]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-19.html
 [7]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-21.html
 [8]: https://github.com/keiji/adventcalendar_2015_mincomi
 [9]: https://twitter.com/neyuki_rei
 [10]: https://www.mincomi.jp/title/?title=343057
 [11]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [12]: https://itunes.apple.com/jp/app/minnanokomikku/id1050104822?mt=8