---
title: 画像の縦横比 〜 ImageViewの辛みを添えて
author: Keiji Ariyama
type: post
date: 2015-12-07T15:00:10+00:00
url: /2015/12/mincomi-adventcalendar-8.html
categories:
  - Android
  - 雑記

---
## はじめに

この記事は「[みんコミ Advent Calendar][1]」の８日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.2）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

ビューアーで作品を読むとき、ディスプレイのアスペクト比が4:3の端末（Nexus 9など）では、上下の表示が切れてしまいます。

上下に十分な余白がある作品であれば読めないことはないのですが、昨日紹介した「[うわようじょつよい][4]」の場合は、紙面全体フルに使っているので、どうしても読めないセリフが出てきます。

<div id="attachment_828" style="max-width: 1546px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/Screenshot_20151114-220943.png"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/Screenshot_20151114-220943.png" alt="派手な看護婦著 「うわようじょつよい」 6pより" width="1536" height="262" class="size-full wp-image-828" /></a>
  
  <p class="wp-caption-text">
    派手な看護婦著 「うわようじょつよい」 6pより 上部が切れてセリフが読めない。拡大してスクロールもできない
  </p>
</div>

なぜこういうことが起きるかというと、現在のアプリの画像の拡大処理は、横幅のみを基準に拡大率を決めているのだと推測します。

例えば、Ａ４版の大きさを「4:3」と「16:9」の画面サイズにそれぞれ拡大する場合を考えてみましょう。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/aspect_ratio.001.jpeg" alt="aspect_ratio.001" width="1024" height="768" class="aligncenter size-full wp-image-825" />][5]

これを横幅のみを基準に拡大すると、次のようになります。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/aspect_ratio.002.jpeg" alt="aspect_ratio.002" width="1024" height="768" class="aligncenter size-full wp-image-826" />][6]

ごらんのように、4:3のディスプレイでは上下が切れてしまいました（紫色部分）。これがまさに今、アプリの中で起きていることです。

それでは、縦幅を基準に拡大してはどうでしょうか。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/aspect_ratio.003.jpeg" alt="aspect_ratio.003" width="1024" height="768" class="aligncenter size-full wp-image-827" />][7]

今度は16:9のディスプレイで横が切れてしまいました（紫色部分）。

この事から、縦横の拡大率のうち、より小さい倍率（画像がディスプレイからはみ出さない）を実際の拡大率として採用する必要があることがわかります。

しかし、この場合も新しい問題が出てきます。縦の拡大率を採用した場合、4:3のディスプレイでは左右に余白が出てしまいます。
  
横が切れると、見開きなどで絵が繋がらない場合があるので、表示中の画像が左右のページのどちらかの情報をもっておいて、それを参照して左または右に寄せる必要があります。

LogCatを見ると電子書籍データをJNIを使ってネイティブで処理しているようなので、描画部分がどれだけJava層にあるかは不明ですが……取り急ぎ対応をするのであれば、描画の対象になっているおそらくSurfaceViewのアスペクト比と位置を調整するロジックを組み込んでやることでしょうか（ApiDemosのCamera APIのサンプルが参考になると思います）。

### AspectRatioImageView

今回の課題は単純に拡大縮小のロジックに起因するものですが、Androidも似たような問題を抱えていますね。それもImageViewというかなりよく使うクラスで。

[Resizing ImageView to fit to aspect ratio][8]

ImageViewは、横幅基準にすると、アスペクト比を保持したまま画像を拡大縮小して表示できません。なので独自のクラスを作って対応することになります。

「AspectRatioImageView」で検索すると山ほど出てきますね。

知っていれば「ああ、またか」くらいのものなのですが、知らなければハマってしまうところなので注意したいことです。

* * *

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、ご了承下さい。
    

* * *

[みんコミ][2]といえば、僕が普段からお世話になっている根雪れい（@neyuki_rei）さんも連載していますね。

<blockquote class="twitter-tweet" lang="ja">
  <p lang="ja" dir="ltr">
    10歳になったおかあさんと僕の生活をマンガにしましたよ～。見てくださいね ！　<a href="https://t.co/UfxCfaOYRZ">https://t.co/UfxCfaOYRZ</a>　 <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%81%AA%E3%81%AE%E3%82%B3%E3%83%9F%E3%83%83%E3%82%AF?src=hash">#みんなのコミック</a> <a href="https://t.co/2nHBnFus4n">pic.twitter.com/2nHBnFus4n</a>
  </p>
  
  <p>
    — 根雪れい.おかあさん(10)と僕。連載中 (@neyuki_rei)
  </p>
  
  <p>
    <a href="https://twitter.com/neyuki_rei/status/664369017038110720">2015, 11月 11</a>
  </p>
</blockquote>

根雪さんの「おかあさん(10)と僕。」は……まだ更新されていませんか。そうですか。

それでは代わりにもう一つ、「みんコミ」で気になっている作品を紹介します。

<blockquote class="twitter-tweet" lang="ja">
  <p lang="ja" dir="ltr">
    まじとら！連載開始しました！オススメやシェアなどで応援どうぞよろしくお願いします　 <a href="https://t.co/nhxwiqO2OD">https://t.co/nhxwiqO2OD</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://t.co/PFqc6s0A93">pic.twitter.com/PFqc6s0A93</a>
  </p>
  
  <p>
    &mdash; 香椎ゆたか@まじとら！連載中 (@yutakashii) <a href="https://twitter.com/yutakashii/status/664373356435668992">2015, 11月 11</a>
  </p>
</blockquote>

１話を読んだときはピンとこなかったんですが、２話で面白みが増したと思います。

主人公はどうやら変態ですが、こう、明るい変態と言うんでしょうか。後ろ暗さが全くないところに好感が持てますね。

* * *

それでは明日、７日の担当は「魔法少女まどか☆マギカ」は１０話が本編と言って憚らない「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://www.mincomi.jp/title/?title=327720
 [5]: https://blog.keiji.dev/wp-content/uploads/2015/12/aspect_ratio.001.jpeg
 [6]: https://blog.keiji.dev/wp-content/uploads/2015/12/aspect_ratio.002.jpeg
 [7]: https://blog.keiji.dev/wp-content/uploads/2015/12/aspect_ratio.003.jpeg
 [8]: http://stackoverflow.com/questions/12400113/resizing-imageview-to-fit-to-aspect-ratio