---
title: スプラッシュスクリーンのキャンセル
author: Keiji Ariyama
type: post
date: 2015-12-14T15:00:29+00:00
url: /2015/12/mincomi-adventcalendar-15.html
categories:
  - Android
  - 雑記

---
## はじめに

この記事は「[みんコミ Advent Calendar][1]」の１５日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

「みんコミ」アプリは、起動時にスプラッシュスクリーンを表示します。

スプラッシュスクリーン自体、Googleはあまり推奨していないのですが、「みんコミアプリ」の場合は何らかの通信処理をしている様子なので、スプラッシュスクリーンの意味はそれなりにあるのだと認識しています。

なぜ通信をしていると思ったかというと、スプラッシュスクリーンの表示されている時間が一定ではなく、通信状態が悪いところでは長く表示されるためです。

僕は時間がかかるとバックキーでアプリを閉じるのですが、しばらくして別のアプリを使っているときに「みんコミ」アプリが立ち上がってきたりします。

Androidでは、Activityを終了してもプロセスは生きているので、バックグラウンド処理の際にはそのことを考慮しなくてはなりません。

忘れた頃に起動するとびっくりするので、スプラッシュスクリーンがキャンセルされたら、次の画面は起動しないという風にした方が良いと思います。

今回の場合、ActivityのライフサイクルのonPauseできちんとバックグラウンド処理より先に行かないように手当をする。
  
またはバックグラウンド処理が終わったときに`isFinishing()`メソッドでActivityが終了しているかを判定し、終了していなかったとき（falseのとき）だけ、次の処理に進むようにすると良いと思います。

<pre><code class="java">public class SplashActivity extends AppCompatActivity {
    private static final long DELAY_TIME = 5 * 1000;

    private final Handler mHandler = new Handler();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        ImageView imageView = new ImageView(this);
        imageView.setImageResource(R.mipmap.ic_launcher);

        setContentView(imageView);

        mHandler.postDelayed(new Runnable() {
            @Override
            public void run() {
                if (!isFinishing()) {
                    Intent intent = MainActivity.newIntent(SplashActivity.this);
                    startActivity(intent);
                }
            }
        }, DELAY_TIME);

    }
}
</code></pre>

このコードを実行すると、次のように画面全体にデフォルトのアイコンが表示され、５秒後にMainActivityが表示されます。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-15-002822.png" alt="device-2015-12-15-002822" width="50%" height="50%" class="aligncenter size-full wp-image-902" />][4]

５秒経過する前にバックキーでアプリを閉じると、次の画面は表示されません。
  
（ただし、ホームキーでバックグラウンドに回した場合は、やはり次の画面が立ち上がってくるので注意が必要です）

サンプルプロジェクトを[GitHub][5]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][5]

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

根雪さんの「おかあさん(10)と僕。」はまだ更新日（毎月第３木曜日）ではないですね。

最新話が第１話の作品は、残すところあと５作品となりました。

それでは明日、16日の担当は年末に発表予定を入れすぎたと心底後悔している「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-15-002822.png
 [5]: https://github.com/keiji/adventcalendar_2015_mincomi