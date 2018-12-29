---
title: SurfaceViewを使ってはいけない
author: Keiji Ariyama
type: post
date: 2015-12-19T15:00:52+00:00
url: /2015/12/mincomi-adventcalendar-20.html
categories:
  - Android
  - 雑記

---
## はじめに

この記事は「[みんコミ Advent Calendar][1]」の20日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.io/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

## abstract

別スレッドから描画でき、高速な描画が期待できるSurfaceViewは、描画にハードウェア支援（Hardware Acceleration）が適用されない。

[Canvas in SurfaceView &#8211; hardware acceleration][4]

Bitmapを描画するパフォーマンステストを実行したところ、通常のViewを拡張した描画と比較して、SurfaceViewによる描画は10倍以上遅いということがわかった（単位はnano second）。

| View       | SurfaceView  |
|:---------- |:------------ |
| 142,590.55 | 1,989,301.12 |

以上のことから、描画速度を求めるのであればSurfaceViewではなく、Viewを継承したCustom Viewを実装することが適切である。

## SurfaceView

「みんコミ」アプリのコミックビューアーには現在`SurfaceView`が使われていると理解しています。

なぜそう思ったかというと、

  1. 最初のページで端末を横向きにして画面を回転させる。
  2. ページを拡大
  3. ページを縮小

すると、次のように描画にノイズが入るんですね。再描画時にCanvasがリセットされないのはSurfaceViewの特徴なので、きっとSurfaceViewを使っているのだろうと判断しました。

<div id="attachment_984" style="max-width: 970px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.io/wp-content/uploads/2015/12/surfaceview.png"><img src="https://blog.keiji.io/wp-content/uploads/2015/12/surfaceview.png" alt="東皓司著 「ひとはくん、ひとりぼっち？」 ２話 １ページ" width="960" height="540" class="size-full wp-image-984" /></a>
  
  <p class="wp-caption-text">
    東皓司著 「ひとはくん、ひとりぼっち？」 ２話 １ページ
  </p>
</div>

指摘した現象自体は簡単に解決できます。描画の最初にCavnasにdrawColorしてキャンバスを一度塗りつぶしてやれば良いだけです。

しかし今回は、表題の通り「SurfaceViewを使ってはいけない」という話です。

SurfaceView、使いますよね。
  
だって、コミックの大きな画像を滑らかに拡大縮小・スクロールしたいわけですから、普通は実装するときに「SurfaceView」を選択すると思います（もし違っていたらごめんなさい）。

## SurfaceViewをつかってはいけない

SurfaceViewが速かったのは過去の話です。

突然こんなこと言ってごめんね。
  
でも本当です。

Android 4.0以降で標準で有効になっている「ハードウェアアクセラレーション」による描画支援を「View」は受けられますが、「SurfaceView」は受けられません。

[Canvas in SurfaceView &#8211; hardware acceleration][4]

すると、SurfaceViewで描画すると逆にViewより遅くなってしまうのです。

[Why “SurfaceView” is slower than a custom “view”?][5]

［00:38 追記］ベンチマークの条件を追記しました。

試しに、ViewとSurfaceViewでどの程度速度が違うのか、Nexus 5X（Android 6.0.1 &#8211; 給電状態）で、500&#215;600のビットマップを1000回、描画して平均を取ってみました（単位はnano second）。

| View       | SurfaceView  |
|:---------- |:------------ |
| 142,590.55 | 1,989,301.12 |

その差「10倍」以上です。

そう。僕たちが速度を求めて選んでいた`SurfaceView`は、実はViewよりも遙かに遅いのです。
  
またSurfaceViewはCamera2 APIからはプレビューにも使えません。
  
それが今のSurfaceViewなんです。

なんでまだdeprecatedじゃないんでしょうか。これ。

一応、SurfaceViewの使いどころみたいなのを挙げるのであれば……例えば、サーバーから非同期にダウンロードするデータを描画していきたい場合でしょうか。
  
UIスレッド内のonDraw内でネットワーク通信はできませんし、複雑な処理をするとやはりANR(Application Not Responding)が発生してしまうことが予想されます。

［00:17 追記］そういう時にはTextureViewですね。

 <iframe src="//www.slideshare.net/slideshow/embed_code/key/zFoChI8uMzGcK7" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen></iframe>

<div style="margin-bottom:5px">
  <strong> <a href="//www.slideshare.net/eaglesakura/abc2014spring-final" title="あ・・・ありのまま今起こったことを話すぜ！おれはTextureViewでプロジェクトを開始していたと思ったらいつのまにかSurfaceViewに戻っていた" target="_blank">あ・・・ありのまま今起こったことを話すぜ！おれはTextureViewでプロジェクトを開始していたと思ったらいつのまにかSurfaceViewに戻っていた</a> </strong> from <strong><a href="//www.slideshare.net/eaglesakura" target="_blank">Yamashita Takeshi</a></strong>
</div>

TextureViewのベンチマークも掲載しておきます。単純なBitmapの描画であればやはりViewの方が速いです。

| View       | SurfaceView  | TextureView |
|:---------- |:------------ |:----------- |
| 142,590.55 | 1,989,301.12 | 518,378.38  |

Googleはもっと積極的に「SurfaceViewは使うな。Viewの方が速い」とアピールすべきだと思います。そうでないと、SurfaceViewを使って速度が上がらず呻く開発者がこれからも出続けるでしょう。

自戒を込めて。

あらためて、パフォーマンステスト（DrawPerformanceActivity）を含めたコードを[GitHub][6]に置いておきます。参考になれば幸いです。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][6]

* * *

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、ご了承下さい。
    

* * *

[みんコミ][2]といえば、僕が普段からお世話になっている根雪れい（@neyuki_rei）さんも連載していますね！

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    おかあさん(10)と僕。という10歳のおかあさんマンガが2話まで読めますよー…読んでくださいませ～　<a href="https://t.co/UfxCfaOYRZ">https://t.co/UfxCfaOYRZ</a> <a href="https://t.co/M7ioKciVZX">pic.twitter.com/M7ioKciVZX</a>
  </p>
  
  <p>
    &mdash; 根雪れい.おかあさん(10)と僕。連載中 (@neyuki_rei) <a href="https://twitter.com/neyuki_rei/status/678186480552968192">December 19, 2015</a>
  </p>
</blockquote>

根雪さんの作品の更新は来年になるので、

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    連載始まりました、よろしくおねがいします！１話めはこちらからどうぞ〜！ >&#10;ひとはくん、ひとりぼっち？ <a href="https://t.co/7bAjspvWDa">https://t.co/7bAjspvWDa</a> 　<a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a>
  </p>
  
  <p>
    &mdash; 東皓司 (@azumakohji) <a href="https://twitter.com/azumakohji/status/664371526041366528">November 11, 2015</a>
  </p>
</blockquote>

人間以外にしか好かれない男の子が主人公です。ヒロインの「こまこ」の仕草が可愛いです。あと眉毛。

二話目になると単純に使い捨ての悪者かと思っていたキャラクターが再登場したり、最後には「これからストーリーが動き出すぞ」と、感じさせる引きがあったりと構成が上手いなぁと思います。

あ、眼鏡っ娘は出ません。
  
残念です。

* * *

それでは明日21日の担当は、ずっとSurfaceViewを信じてきたのに裏切られた気分の「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: http://stackoverflow.com/questions/9588776/canvas-in-surfaceview-hardware-acceleration
 [5]: http://stackoverflow.com/questions/23893266/why-surfaceview-is-slower-than-a-custom-view
 [6]: https://github.com/keiji/adventcalendar_2015_mincomi