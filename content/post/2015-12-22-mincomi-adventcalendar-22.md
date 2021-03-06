---
title: アプリのナビゲーション（下タブについて）
author: Keiji Ariyama
type: post
date: 2015-12-21T15:00:26+00:00
url: /2015/12/mincomi-adventcalendar-22.html
enclosure:
  - |
    |
        https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-205953.mp4
        828189
        video/mp4
        
  - |
    |
        https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-213530.mp4
        1492389
        video/mp4
        
categories:
  - Android
  - みんコミ Advent Calendar

---

----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**
----

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の22日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]（公開終了しています）

<!--more-->

* * *

「みんコミ」アプリは、基本的に画面の下にあるタブ（下タブ）で画面を切り替えます（ランキング画面などでは上タブが現れることがあります）。

iOSでは一般的なこのナビゲーションですが、Androidでは下タブによる画面の切り替えは推奨されていません。

[Don&#8217;t use bottom tab bars][4]

Androidの場合、下端にはナビゲーションバーがあるので、誤操作の原因になりやすいというのが理由だと僕は考えています。

<div id="attachment_1028" style="max-width: 730px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-223002.png"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-223002.png" alt="ナビゲーションバー" width="360" height="47" class="size-full wp-image-1028" /></a>
  
  <p class="wp-caption-text">
    ナビゲーションバー
  </p>
</div>

Androidでは、上部に表示されている「ActionBar(ToolBar)」を使います。
  
厳密に言うと、Androidでも画面の下端に拡張する「Split ActionBar」という仕組みがあるのですが、メニューの配置を柔軟にできないので滅多に使いません（僕は使ったことはありません）。

「みんコミ」アプリに話を戻しましょう。

こう言う場合、最上段にActionBarを置いて、その下にタブを表示、残りの領域をコンテンツ表示領域……というのが定石だと思いますが……正直、「みんコミ」アプリの場合は定石をそのまま当てはめるのは難しいところだと思います。

現在の「みんコミ」アプリのホーム画面は、最上部にViewPagerで構築された作品の「バナー」があり、その下に作品が並ぶという構造です。
  
この場合、一番見せたいのは「コンテンツ」であって、タブでもなければActionBar(ToolBar)でもありません。

折衷案として、コンテンツを一番上に置いたまま上タブを実現するUIを作ってみました（動画です）。

<div style="width: 360px;" class="wp-video">
  <!--[if lt IE 9]><![endif]--><video class="wp-video-shortcode" id="video-738-1" width="360" height="640" preload="metadata" controls="controls"><source type="video/mp4" src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-205953.mp4?_=1" />
  
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-205953.mp4">https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-205953.mp4</a></video>
</div>

[Design Support Library][5]を使っていて、最初はバナーが表示されていますが、下にスクロールすると視差効果（パララックス）とともにバナーが隠れていきます。

そしてバナーが隠れきった後も、タブは一番上に固定されるので画面の切り替えができます。

このUIのXMLは次の通りです。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;android.support.design.widget.CoordinatorLayout
    android:id="@+id/main_content"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"&gt;

    &lt;android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="@dimen/detail_backdrop_height"
        android:fitsSystemWindows="true"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"&gt;

        &lt;android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/collapsing_toolbar"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:fitsSystemWindows="true"
            app:contentScrim="?attr/colorPrimary"
            app:expandedTitleMarginEnd="64dp"
            app:expandedTitleMarginStart="48dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"&gt;

            &lt;RelativeLayout
                android:id="@+id/backdrop"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:fitsSystemWindows="true"
                android:scaleType="centerCrop"
                app:layout_collapseMode="parallax"&gt;

                &lt;android.support.v4.view.ViewPager
                    android:id="@+id/viewpager"
                    android:layout_width="wrap_content"
                    android:layout_height="206dip"
                    android:paddingBottom="16dp"
                    android:paddingTop="16dp"/&gt;

                &lt;LinearLayout
                    android:id="@+id/pagecontrol"
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:layout_alignBottom="@id/viewpager"
                    android:gravity="center_horizontal"
                    android:orientation="horizontal"/&gt;

            &lt;/RelativeLayout&gt;

        &lt;/android.support.design.widget.CollapsingToolbarLayout&gt;

        &lt;android.support.design.widget.TabLayout
            android:id="@+id/tabs"
            android:layout_width="match_parent"
            android:layout_height="50dip"
            app:layout_collapseMode="pin"/&gt;

    &lt;/android.support.design.widget.AppBarLayout&gt;

    &lt;android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"&gt;

        &lt;LinearLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="vertical"
            android:paddingTop="24dp"&gt;

            &lt;SeekBar
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:max="50"
                android:progress="10"
                android:progressDrawable="@drawable/seekbar"/&gt;

            &lt;View
                android:layout_width="match_parent"
                android:layout_height="2400dp"/&gt;
        &lt;/LinearLayout&gt;

    &lt;/android.support.v4.widget.NestedScrollView&gt;


&lt;/android.support.design.widget.CoordinatorLayout&gt;
</code></pre>

また、下タブを選択した上で、今度はアクションバー(ToolBar)を残すというUIも試しに作成しました。

<div style="width: 360px;" class="wp-video">
  <video class="wp-video-shortcode" id="video-738-2" width="360" height="640" preload="metadata" controls="controls"><source type="video/mp4" src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-213530.mp4?_=2" /><a href="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-213530.mp4">https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-21-213530.mp4</a></video>
</div>

同じくDesign Support Libraryを使っています。

アクションバーが残るので、例えば「その他」のタブをオーバーフローメニューの中に移動することで、タブ操作を簡素化することもできると思います。

ただし、ViewPagerの高さとToolBarの位置関係の設定が難しく、まだまだ調整が必要ですね。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;RelativeLayout
    android:id="@+id/main_content"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"&gt;

    &lt;android.support.design.widget.TabLayout
        android:id="@+id/tabs"
        android:layout_width="match_parent"
        android:layout_height="50dip"
        android:layout_alignParentBottom="true"
        app:layout_collapseMode="pin"/&gt;

    &lt;android.support.design.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@id/tabs"&gt;

        &lt;android.support.design.widget.AppBarLayout
            android:id="@+id/appbar"
            android:layout_width="match_parent"
            android:layout_height="@dimen/detail_backdrop_height"
            android:fitsSystemWindows="true"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"&gt;

            &lt;android.support.design.widget.CollapsingToolbarLayout
                android:id="@+id/collapsing_toolbar"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:fitsSystemWindows="true"
                app:contentScrim="?attr/colorPrimary"
                app:expandedTitleMarginEnd="8dp"
                app:expandedTitleMarginStart="16dp"
                app:layout_scrollFlags="scroll|exitUntilCollapsed"&gt;

                &lt;RelativeLayout
                    android:id="@+id/backdrop"
                    android:layout_width="match_parent"
                    android:layout_height="300dp"
                    android:fitsSystemWindows="true"
                    android:scaleType="centerCrop"
                    app:layout_collapseMode="parallax"&gt;

                    &lt;android.support.v4.view.ViewPager
                        android:id="@+id/viewpager"
                        android:layout_width="wrap_content"
                        android:layout_height="206dp"
                        android:layout_marginBottom="16dp"
                        android:paddingTop="16dp"/&gt;

                    &lt;LinearLayout
                        android:id="@+id/pagecontrol"
                        android:layout_width="match_parent"
                        android:layout_height="wrap_content"
                        android:layout_alignBottom="@id/viewpager"
                        android:gravity="center_horizontal"
                        android:orientation="horizontal"
                        android:paddingBottom="16dp"/&gt;

                &lt;/RelativeLayout&gt;

                &lt;android.support.v7.widget.Toolbar
                    android:id="@+id/toolbar"
                    android:layout_width="match_parent"
                    android:layout_height="?attr/actionBarSize"
                    app:layout_collapseMode="pin"
                    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/&gt;

            &lt;/android.support.design.widget.CollapsingToolbarLayout&gt;

        &lt;/android.support.design.widget.AppBarLayout&gt;

        &lt;android.support.v4.widget.NestedScrollView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior"&gt;

            &lt;LinearLayout
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:orientation="vertical"
                android:paddingTop="24dp"&gt;

                &lt;SeekBar
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:max="50"
                    android:progress="10"
                    android:progressDrawable="@drawable/seekbar"/&gt;

                &lt;View
                    android:layout_width="match_parent"
                    android:layout_height="2400dp"/&gt;
            &lt;/LinearLayout&gt;

        &lt;/android.support.v4.widget.NestedScrollView&gt;

    &lt;/android.support.design.widget.CoordinatorLayout&gt;

&lt;/RelativeLayout&gt;
</code></pre>

サンプル（activity\_main.xml, activity\_bottom_tabs.xml）を含めたコードを[GitHub][6]に置いておきます。参考になれば幸いです。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][6]

### 実装参考

  * https://github.com/chrisbanes/cheesesquare

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
    「みんなのコミック」にて連載始まってるそうです。よろしくね。&#10;<a href="https://t.co/sdujA9PMHk">https://t.co/sdujA9PMHk</a>&#10;<a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://t.co/722yl8oFve">pic.twitter.com/722yl8oFve</a>
  </p>
  
  <p>
    &mdash; 牡丹ぼたもち (@not_ohagi) <a href="https://twitter.com/not_ohagi/status/666224783634202624">November 16, 2015</a>
  </p>
</blockquote>

ほんわかしたお嬢様ストーリーかと思って読み始めたら……これって実はシュールコメディなんじゃないでしょうか（いや、そう思うのは僕の心が汚れているせいかもしれませんが）。

１話の最初のコマから両サイドをチョコファウンテン、正面の置かれたホールケーキに向かっている主人公が登場します。朝食のシーンだそうです。

お嬢様だけあって黒塗りの高級車で通学しますが、なんとこの車、食べられるそうです。
  
意味がわかりません。

「今日は11月11日。花ちゃんの誕生日パーティの日です。今日は早めに帰りたいです」
  
「いいでしゅねー そうしましょー」

この作品にツッコミ役は不在です。先ほどの会話は生徒と教師の会話です。
  
一応、イベントがあればキャラクターからのリアクションはありますが、次の瞬間には「当たり前のこと」として処理されています。

また、憎まれ役とは言え、女子が嘔吐しているシーンが描かれ、そのたびに「聖水です」「美容液です」と注釈が付きます。

内容は相当おかしいんですが、ひたすら可愛い絵が、それを普通と思わせる世界観を支えています。

どう話が転がっていくのか、正直、まったく先が読めません。。。

あ、担任の先生が眼鏡っ娘でした。
  
40歳だろうが可愛ければ関係ありませんね。

* * *

それでは明日23日の担当は、「こどもちぇんじ」二話の最後のページでは正直どん引きしてしまった「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: http://developer.android.com/intl/ja/design/patterns/pure-android.html
 [5]: http://android-developers.blogspot.jp/2015/05/android-design-support-library.html
 [6]: https://github.com/keiji/adventcalendar_2015_mincomi