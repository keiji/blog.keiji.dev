---
title: アプリの素材と解像度
author: Keiji Ariyama
type: post
date: 2015-12-02T15:00:00+00:00
url: /2015/12/mincomi-adventcalendar-3.html
categories:
  - Android
  - みんコミ Advent Calendar

---
「[みんコミ Advent Calendar][1]」３日目を担当する有山圭二です。 ３日目はアプリに表示する画像素材についてです。

本記事で話題にする「[みんコミ][2]」のAndroidアプリは、「Google Play Store」からインストールできます。さっそくインストールしてみましょう。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

※ なお、本記事は「バージョン1.0.1」を元に執筆しています。

<!--more-->

* * *

トップ画面を開いてまず目に入るのは、一番上のViewPagerを利用したサムネイルです。

そして、その下方に配置されているページ・インジケータ（iOSで言えばUIPageControlといえばわかりやすいのでしょうか）が、画像の解像度が足りていないのでしょうか、円形ではなく菱形に表示され、ジャギーが見えているのが気になりました。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/page_control.png" alt="page_control" width="240" height="73" class="aligncenter size-full wp-image-723" />][4]

このような単純な素材であれば、画像ではなく[Shape Drawable][5]で作成してしまうのが良いと思います。

例えば、res/drawableに「page\_indicator\_normal.xml」を作り、内容を次のようにします。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval"&gt;

    &lt;size
        android:width="10dp"
        android:height="10dp"/&gt;

    &lt;solid
        android:color="@android:color/white"/&gt;

&lt;/shape&gt;
</code></pre>

もう一つ「page\_indicator\_selected.xml」を作ります。「page\_indicator\_normal.xml」との違いは色が赤色と言うことです。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval"&gt;

    &lt;size
        android:width="10dp"
        android:height="10dp"/&gt;

    &lt;solid
        android:color="#ff0000"/&gt;

&lt;/shape&gt;
</code></pre>

最後に、この二つのDrawableを状態に応じて切り替えるセレクターとして「page_indicator.xml」を作成します。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;selector xmlns:android="http://schemas.android.com/apk/res/android"&gt;

    &lt;item android:drawable="@drawable/page_indicator_selected" android:state_selected="true"/&gt;
    &lt;item android:drawable="@drawable/page_indicator_normal"/&gt;
&lt;/selector&gt;
</code></pre>

あとは、「page_indicator.xml」をDrawableとしてImageViewに読み込ませれば、ImageViewの状態（selected）に応じて色が変わります。

このように、簡単な画像素材であればXMLとして作成することができます。また、Lolipop（Android 5.0）以上であればVectorDrawableに対応しているので、SVGに近い形式で記述すれば複雑な形も描画できますが、こちらはまだ少し使い勝手が悪いと感じているので、あまりお勧めはしません。

サンプルプロジェクトを[GitHub][6]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][6]

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-01-154732.png" alt="device-2015-12-01-154732" width="180" height="320" class="aligncenter size-full wp-image-725" />][7]

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、あらかじめご了承下さい。
    

* * *

蛇足になりますが、[みんコミ][2]といえば、僕が普段からお世話になっている根雪れい（@neyuki_rei）さんも連載陣に名を連ねていますね。

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

そろそろ次の話が更新されてくれないと書くことも少なくなってきました。
  
早く第２話を読みたいものです……！！

* * *

それでは明日、４日の担当は「有山圭二」さんです。
  
よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/wp-content/uploads/2015/12/page_control.png
 [5]: http://developer.android.com/intl/ja/guide/topics/resources/drawable-resource.html#Shape
 [6]: https://github.com/keiji/adventcalendar_2015_mincomi
 [7]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-01-154732.png