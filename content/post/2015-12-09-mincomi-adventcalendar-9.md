---
title: 便利なIntent
author: Keiji Ariyama
type: post
date: 2015-12-08T15:00:27+00:00
url: /2015/12/mincomi-adventcalendar-9.html
categories:
  - Android
  - みんコミ Advent Calendar

---
## はじめに

この記事は「[みんコミ Advent Calendar][1]」の９日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.2）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

## 外部リンクのクリック

「みんコミ」は、SNSでの口コミを重視しているようです。

実際、Twitterの[#みんコミ][4]を見ると、読者がコミックの感想を寄せていたり、作者の人たちが作品を宣伝していたりします。

その際に、作品ページへのリンクも投稿されているのですが、Android端末でそれらのリンクをタップして開くWeb版の「みんコミ」は、AndroidやiOSのブラウザからは見られないように制限されています。

<div id="attachment_852" style="max-width: 1090px" class="wp-caption alignnone">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-220923.png"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-220923.png" alt="数秒後にGoogle Play Storeへリダイレクトされる" width="25%" height="25%" class="size-full wp-image-852" /></a>
  
  <p class="wp-caption-text">
    数秒後にGoogle Play Storeへリダイレクトされる
  </p>
</div>

残念なのは、Android版の「みんコミ」アプリをインストールしていても、やはりリンクをクリックするとブラウザが開いてしまうと言うことです。

Androidには`Intent`という便利な仕組みがあります。
  
Intentを使えば、リンクをクリックしたときにどのアプリで選ぶかをユーザーに選択肢として提示でき、SNSのリンクから、そのままアプリを起動してコンテンツに誘導できます。

### AndroidManifest.xml

Intentに関する情報はAndroidManifest.xmlに記載します。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;manifest
    package="io.keiji.mincomisample"
    xmlns:android="http://schemas.android.com/apk/res/android"&gt;

    &lt;application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"&gt;
        &lt;activity android:name=".MainActivity"&gt;
        &lt;/activity&gt;
        &lt;activity
            android:name=".SettingActivity"
            android:label="その他"&gt;
            &lt;intent-filter&gt;
                &lt;action android:name="android.intent.action.MAIN"/&gt;

                &lt;category android:name="android.intent.category.LAUNCHER"/&gt;
            &lt;/intent-filter&gt;
        &lt;/activity&gt;
        &lt;activity
            android:name=".TitleActivity"
            android:label="タイトルページ"&gt;
            &lt;intent-filter&gt;
                &lt;action android:name="android.intent.action.VIEW"/&gt;

                &lt;category android:name="android.intent.category.DEFAULT"/&gt;
                &lt;category android:name="android.intent.category.BROWSABLE"/&gt;

                &lt;data
                    android:host="www.mincomi.jp"
                    android:pathPattern="/?title=.*"
                    android:pathPrefix="/title"
                    android:scheme="http"/&gt;

                &lt;data
                    android:host="www.mincomi.jp"
                    android:pathPattern="/?title=.*"
                    android:pathPrefix="/title"
                    android:scheme="https"/&gt;
            &lt;/intent-filter&gt;
        &lt;/activity&gt;
        &lt;activity
            android:name=".AuthorActivity"
            android:label="著者ページ"&gt;
            &lt;intent-filter&gt;
                &lt;action android:name="android.intent.action.VIEW"/&gt;

                &lt;category android:name="android.intent.category.DEFAULT"/&gt;
                &lt;category android:name="android.intent.category.BROWSABLE"/&gt;

                &lt;data
                    android:host="www.mincomi.jp"
                    android:pathPattern="/?author=.*"
                    android:pathPrefix="/author"
                    android:scheme="http"/&gt;

                &lt;data
                    android:host="www.mincomi.jp"
                    android:pathPattern="/?author=.*"
                    android:pathPrefix="/author"
                    android:scheme="https"/&gt;
            &lt;/intent-filter&gt;
        &lt;/activity&gt;
    &lt;/application&gt;

&lt;/manifest&gt;
</code></pre>

作品タイトルを開くのは`TitleActivity`、著者ページは`AuthorActivity`が、それぞれ担当します。
  
`data`タグの`host`に、アプリが受け取るURLに関する情報を記述しています。なお、schemeは`http`と`https`両方をハンドリングできるようにしてあります。

このアプリをインストールした状態で、再びTwitterなどで「みんコミ」のリンクをクリックします。

すると、次のように起動するアプリの選択画面が表示されます。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-225455.png" alt="device-2015-12-07-225455" width="50%" height="50%" class="aligncenter size-full wp-image-844" />][5]

ここで「タイトルページ」を選択すると、サンプルアプリが起動します。
  
画面にはtitleIdやauthorIdとしてクリックしたリンクの最後にある数字が表示されます。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-225106.png" alt="device-2015-12-07-225106" width="25%" height="25%" class="wp-image-845"  style="float:left; margin-right: 16px;" />][6]

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-225040.png" alt="device-2015-12-07-225040" width="25%" height="25%" class="alignnone wp-image-846" />][7]

### Intentの処理

プログラム側を見てみましょう。ユーザーがクリックしたURLは、アプリのActivity側からはIntentの形で受け取ります。
  
今回の場合はgetQueryParameterで`title`や`author`の値を取得します。

<pre><code class="java">    private void processIntent(Intent intent, TextView textView) {

        Uri uri = intent.getData();
        String authorId = uri.getQueryParameter("author");

        textView.setText("authorId = " + authorId);
    }
</code></pre>

実際には、これらの値をもとに、タイトルの詳細や著者の詳細を表示することになるでしょう。

ただし、Intentで外部からのリンクを受け付けるときには、アプリ自体のセキュリティ設計に注意して下さい。データの更新を伴うIntentの場合、不正なリンクを送り込まれてデータを破壊されたり、ContentProviderなどで設計に不備があると、アプリの情報が不正に利用されたりする可能性があります。

サンプルプロジェクトを[GitHub][8]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][8]

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

根雪さんの「おかあさん(10)と僕。」は……[TODO: 更新されたら感想を書く]

そしてもう一つ、「みんコミ」で気になる作品を紹介します。

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    「みんコミ」で連載始まりましたヽ(ﾟ∀ﾟ)ﾉ ﾊﾟｯ☆&#10;<a href="https://t.co/U3yj63pXQk">https://t.co/U3yj63pXQk</a>&#10;応援よろしくお願いします～！
  </p>
  
  <p>
    &mdash; 明川 真弓@ﾃｨｱ114『と11a』』 (@mayumiasugawa) <a href="https://twitter.com/mayumiasugawa/status/664697932545785856">November 12, 2015</a>
  </p>
</blockquote>

ロリコンの神職（社会職業的な意味で）が、ケモミミ幼女とキスしたら子供に戻ってしまって……という話のようです。
  
何を言っているのか自分でもよくわかりませんが、みんコミってそんな感じの作品が多いです。

ただ、残念なのはこの作者さん、[Twitterアカウント][9]を見ると、大変良い眼鏡っ娘を描く人だとわかります。

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    明日のティア、新刊はありませんが少し原稿の猶予を頂けたので 無配のペーパー漫画作ってます！なんてことない水野さんの仕事風景ですがよろしければ受け取りに来てください(ﾉ´∀｀*) <a href="https://twitter.com/hashtag/COMITIA114?src=hash">#COMITIA114</a> <a href="https://twitter.com/hashtag/%E3%82%B3%E3%83%9F%E3%83%86%E3%82%A3%E3%82%A2114?src=hash">#コミティア114</a> <a href="https://t.co/bBA2u13KXL">pic.twitter.com/bBA2u13KXL</a>
  </p>
  
  <p>
    &mdash; 明川 真弓@ﾃｨｱ114『と11a』』 (@mayumiasugawa) <a href="https://twitter.com/mayumiasugawa/status/665535870552793088">November 14, 2015</a>
  </p>
</blockquote>

にも関わらず、この「こどもチェンジ」では第１話に眼鏡っ娘が出てこないという……一体どこで何を間違ったらそうなるのかと問い詰めたくなります。

しかし、カラーの見開き扉絵にはきちんと眼鏡っ娘が出ることが予告されているので、大いに期待したいところです。

* * *

それでは明日１０日の担当は、眼鏡はどんな格好にも似合うけど、ケモミミ眼鏡だけはどうしても認められない「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash
 [5]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-225455.png
 [6]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-225106.png
 [7]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-07-225040.png
 [8]: https://github.com/keiji/adventcalendar_2015_mincomi
 [9]: https://twitter.com/mayumiasugawa