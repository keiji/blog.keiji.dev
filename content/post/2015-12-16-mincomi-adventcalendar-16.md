---
title: ！！ーバクーシ　！転反
author: Keiji Ariyama
type: post
date: 2015-12-15T15:00:06+00:00
url: /2015/12/mincomi-adventcalendar-16.html
categories:
  - Android
  - 雑記

---
## はじめに

この記事は「[みんコミ Advent Calendar][1]」の１６日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

「みんコミ」のコミックビューアーの下部には、ページ送りのシークバーが表示されます（普段は隠れていて、画面中央付近をタップすると現れます）。

以前の記事にも書きましたが、ここで使っている画像素材も解像度が足りていないようです。

<div id="attachment_908" style="max-width: 1204px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/Lollipop.png"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/Lollipop.png" alt="若杉佳著「まりもな日々」2話 1ページより" width="1194" height="141" class="size-full wp-image-908" /></a>
  
  <p class="wp-caption-text">
    背景 &#8211; 若杉佳著「まりもな日々」2話 1ページより
  </p>
</div>

また、Nexus 5X（Marshmallow）では下地の四角い画像が表示されず、丸（Thumb）だけが表示されているので、これでページ位置を変えられるのだと理解しづらいように思います。

<div id="attachment_909" style="max-width: 1090px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/Marshmallow.png"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/Marshmallow.png" alt="若杉佳著「まりもな日々」2話 1ページより" width="1080" height="153" class="size-full wp-image-909" /></a>
  
  <p class="wp-caption-text">
    背景 &#8211; 若杉佳著「まりもな日々」2話 1ページより
  </p>
</div>

電子書籍等のコンテンツリーダーを作ったことがある人ならご存じと思いますが、AndroidのSeekBarは、漫画や小説のページ送りとしてそのままでは使えません。

マンガは基本的に右綴じなのでページが右から左に流れます。一方、AndroidのSeekBar（やProgressBar）は、左から右に値が上がります。

試しに、レイアウトファイルにSeekBarを追加してみます。

<pre><code class="xml">    &lt;SeekBar
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:max="50"
        android:progress="10"/&gt;
</code></pre>

<div id="attachment_913" style="max-width: 1090px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-15-224607.png"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-15-224607.png" alt="下部にシークバーを追加した" width="50%" height="50%" class="aligncenter size-full wp-image-913" /></a>
  
  <p class="wp-caption-text">
    下部にシークバーを追加した
  </p>
</div>

このように、左側に色が付いています。

以前も紹介した`Drawable XML`を使えばSeekBarの色の方向を（見た目上）逆転できます。

まず、`re/drawable/`に`seekbar.xml`を作成します。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;layer-list xmlns:android="http://schemas.android.com/apk/res/android"&gt;

    &lt;item android:id="@android:id/background"&gt;

        &lt;shape android:shape="line"&gt;
            &lt;stroke
                android:width="6dp"
                android:color="@color/colorAccent"/&gt;
        &lt;/shape&gt;

    &lt;/item&gt;

    &lt;item android:id="@android:id/progress"&gt;

        &lt;clip&gt;
            &lt;shape android:shape="line"&gt;
                &lt;stroke
                    android:width="6dp"
                    android:color="@android:color/darker_gray"/&gt;
            &lt;/shape&gt;
        &lt;/clip&gt;

    &lt;/item&gt;

&lt;/layer-list&gt;
</code></pre>

次に、レイアウトファイルのSeekBarの`progressDrawable`属性に、先ほど作成した`drawable/seekbar`を設定します。

<pre><code class="xml">    &lt;SeekBar
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:max="50"
        android:progress="10"
        android:progressDrawable="@drawable/seekbar"/&gt;
</code></pre>

こうすれば、SeekBarの左右の色が反転して表示されます。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-15-232009.png" alt="device-2015-12-15-232009" width="50%" height="50%" class="aligncenter size-full wp-image-914" />][4]

ただし、これはあくまでも見た目を変えたに過ぎません。つまり「値を示する色」と「地の色（@android:color/darker_gray）」を入れ替えて、右から左に増えているように見えるようにしただけです。

プログラム上のSeekBarは、相変わらず左から右に向かって値が増えていきます。そのため、プログラムから値を設定するときやユーザーの入力を受け取るときは「`seekbar.getMax() - seekbar.getProgress()`」などのようにして値を調整する必要があります。

サンプルプロジェクトを[GitHub][5]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][5]

### 実装参照

## * http://stackoverflow.com/questions/5745814/android-change-horizonal-progress-bar-color

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

根雪さんの「おかあさん(10)と僕。」は、いよいよ明日（第３木曜日）ですね！
  
楽しみにしています！

それでは明日17日の担当は、香川でうどんを食べ過ぎた「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-15-232009.png
 [5]: https://github.com/keiji/adventcalendar_2015_mincomi