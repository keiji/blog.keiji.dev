---
title: ScaleGestureとScroll
author: Keiji Ariyama
type: post
date: 2015-12-20T15:35:37+00:00
url: /2015/12/mincomi-adventcalendar-21.html
categories:
  - Android
  - 雑記

---
［12/22追記: `requestDisallowInterceptTouchEvent`のハンドリングについては、onTouchEvent内で処理するより、`onScroll`メソッド内で処理する方が適切だと考えています］

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の21日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

「みんコミ」アプリのコミックビューアーのピンチイン・アウトをしたときの拡大縮小については[前の記事][4]で述べましたが、今回はピンチイン・アウトをした後の挙動についてです。

現在のコミックビューアーは、ピンチイン・アウトをした後、ディスプレイから指を放すと、スクロールが一気に跳ぶことがあります（Scrollerを使った滑らかな移動ではなく、一瞬でスクロール値が変わる感じです）。

再現手順は次の通りです。

  * ディスプレイに指Aを触れる
  * ディスプレイに指Bを触れる
  * ピンチイン・アウトで拡大縮小する
  * ディスプレイから指Bを離す

コードを見ていないのではっきりとしたことは言えないのですが、`MotionEvent`の`getRawX`などで絶対座標をとり、`mLastTouchPositionX`などのフィールドに保持をする仕組みにしているのではないでしょうか。

続いてonScaleが始まる直前、二本目の「指B」が触れた時点で、フィールドに保持している値が「指B」の座標に書き換わってしまっている。

そして「指B」を離すと、次にonTouchEventを受け取ったときに「指B」から「指A」に一気に座標が跳び、結果としてディスプレイの表示も跳んだと考えられます。

それを裏付ける証拠として、前記の手順で指を二本触れた状態から「指A」の方を離すと、スクロールが跳ぶ現象は起きません。また、指Aと指Bの距離が遠いと、ディスプレイに指Bが触れた時点でスクロール量が変わることもあります。

このようなスクロールイベントも`GestureDetector`に任せることができます。

GestureDetectorの使い方は、[前の記事][5]で触れたので割愛します。

`OnGestureListener`の`onScroll`でスクロールイベントを受け取ることができます。得られた値をもとに`mDestRect`の位置を変えています。
  
distanceX、distanceY共に符号の逆転を忘れないで下さい。

<pre><code class="java">        @Override
        public boolean onScroll(MotionEvent e1, MotionEvent e2, float distanceX, float distanceY) {
            mDestRect.offset(-distanceX, -distanceY);
            invalidate();

            return true;
        }
</code></pre>

また、ViewPagerにイベントを渡す、渡さないの判断をしたい場合、`onScroll`メソッド内部でスクロールの可否を判定した後、結果をtrue/falseで返すことで、onTouchEvent内で受け取って処理することができます。

<pre><code class="java">    @Override
    public boolean onTouchEvent(MotionEvent event) {
        mScroller.forceFinished(true);

        boolean handled = mScaleGestureDetector.onTouchEvent(event);

        handled |= mGestureDetector.onTouchEvent(event);

        return handled | super.onTouchEvent(event);
    }
</code></pre>

サンプル（ViewerActivity）を含めたコードを[GitHub][6]に置いておきます。参考になれば幸いです。

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
    西園寺君は今日から庶民 <a href="https://t.co/IuSelfRghD">https://t.co/IuSelfRghD</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a>&#10;描いてます <a href="https://t.co/Oz091CV3fz">pic.twitter.com/Oz091CV3fz</a>
  </p>
  
  <p>
    &mdash; かやはら (@kaya7hara) <a href="https://twitter.com/kaya7hara/status/669133645026758658">November 24, 2015</a>
  </p>
</blockquote>

お金持ちのセレブリティ「西園寺君」が社会勉強のために庶民的な生活を経験しにやってくるというストーリーなのですが……お父さん、ちょっといきなりすぎやしませんか。

現金を持ち歩くことも知らない自分の子供に、いきなり庶民の生活をさせるのは、蛙を熱湯に投げ込むようなものだと思います。

しかし、連載作品は基本的に女性キャラが多く、男性キャラはかなり濃いめなラインナップが揃っている「みんコミ」の中で、「西園寺君」は、ずいぶんとまともな方に見えてしまうのが不思議です（「おかあさん(10)と僕。」の春君は、あの状況で正気を保っていられるのはすごいと思うので除外します）。

あ、眼鏡っ娘は出ません。が、前髪ぱっつんがメインストリームのようです。

我らがAndroid開発者界の誇る[前髪の神][7]の判定を待ちたいところです。

* * *

それでは明日22日の担当は、技術的な内容を書くことより、「みんコミ」の作品の感想を書くときの方がよほどものを考えている「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-19.html
 [5]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-18.html
 [6]: https://github.com/keiji/adventcalendar_2015_mincomi
 [7]: https://twitter.com/adakoda