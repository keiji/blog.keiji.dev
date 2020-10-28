---
title: Scrollerのススメ
author: Keiji Ariyama
type: post
date: 2015-12-17T16:00:28+00:00
url: /2015/12/mincomi-adventcalendar-18.html
categories:
  - Android
  - 雑記

---
## はじめに

この記事は「[みんコミ Advent Calendar][1]」の18日目の記事です。

すみません。根雪れい（@neyuki_rei）さんの「おかあさん(10)と僕。」を読んでいたらアドベントカレンダーの更新が遅れてしまいました。

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

いやはや。まさかのお風呂回に驚きました。布団の柄とかが微妙に凝っていて細部を見ていくと楽しいです。

さて、アドベントカレンダー本編です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

「みんコミ」アプリのコミックビューアー。
  
僕はたいていタブレット（Nexus 9、Xperia Z3 Tablet Compact）で読んでいます（Nexus 5Xは画面が小さいので……）。

しかし、今日（正確には昨日）Nexus 5Xで作品を読んでいて、拡大した状態で勢いよくフリックしてもスクロールが加速しないことに気づきました。

個人的にはぴゅんと表示が跳んで欲しいと思います。

この処理は`Scroller`を使うと比較的簡単に実装できます。

サンプルプロジェクトを[GitHub][4]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][4]

サンプル「スクロールサンプル(ScrollerActivity)」では、赤い■をフリックすると移動するようにプログラムしています。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-18-004150.png" alt="device-2015-12-18-004150" width="50%" height="50%" class="aligncenter size-full wp-image-940" />][5]

まず、GestureDetectorでflingジェスチャーを取得します。
  
使い方は簡単です。GestureDetectorのインスタンスにonTouchが受け取るMotionEventを渡すだけで、onTouchEventの状況から判定して所定のジェスチャーのコールバック（今回の場合はonFling）が呼びます

<pre><code class="java">&lt;br />public class ScrollerView extends View {
    private static final String TAG = ScrollerView.class.getSimpleName();

    private static final int SIZE = 100;
    private static final Paint PAINT = new Paint();

    static {
        PAINT.setColor(Color.RED);
    }

    private Rect mRect;

    private final GestureDetectorCompat mGestureDetector;

    public ScrollerView(Context context) {
        super(context);

        mGestureDetector = new GestureDetectorCompat(context, mOnGestureListener);

        setFocusable(true);
    }

    GestureDetector.OnGestureListener mOnGestureListener = new GestureDetector.OnGestureListener() {
        @Override
        public boolean onDown(MotionEvent e) {
            return true;
        }

        @Override
        public void onShowPress(MotionEvent e) {

        }

        @Override
        public boolean onSingleTapUp(MotionEvent e) {
            return true;
        }

        @Override
        public boolean onScroll(MotionEvent e1, MotionEvent e2, float distanceX, float distanceY) {
            return true;
        }

        @Override
        public void onLongPress(MotionEvent e) {

        }

        @Override
        public boolean onFling(MotionEvent e1, MotionEvent e2, float velocityX, float velocityY) {
            return true;
        }
    };

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        Log.d(TAG, "onTouchEvent " + event.getAction());

        boolean handled = mGestureDetector.onTouchEvent(event);
        if (handled) {
            return true;
        }

        return super.onTouchEvent(event);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        if (mRect == null) {
            mRect = new Rect(0, 0, SIZE, SIZE);
            int left = (canvas.getWidth() - SIZE) / 2;
            int top = (canvas.getHeight() - SIZE) / 2;
            mRect.offset(left, top);
        }

        canvas.drawRect(mRect, PAINT);
    }
}
</code></pre>

サンプルでは、Compatibility Libraryに含まれているGestureDetectorCompatを使用しています。

簡単と言っても、OnGestureListenerの各メソッドの戻り値には気をつけて下さい。Android Studioで自動生成するとデフォルトで各メソッドは`false`を返しますが、そのままではACTION_DOWN以外のイベントを受け取れません（当然、ジェスチャも受け取れません）。
  
適宜`true`を返すようにしましょう。

つぎは`Scroller`です。
  
インスタンス化し、onFlingで受け取った各種の値をそのままflingメソッドに渡します。これで、flingが始まってからの時間に応じて自動的にスクロール量を計算してくれます。

なおGoogleは、OverScrollerを使用するように推奨しています。

<pre><code class="java">&lt;br />public class ScrollerView extends View {
    private static final String TAG = ScrollerView.class.getSimpleName();

    private static final int SIZE = 100;
    private static final Paint PAINT = new Paint();

    static {
        PAINT.setColor(Color.RED);
    }

    private Rect mRect;

    private final OverScroller mScroller;
    private final GestureDetectorCompat mGestureDetector;

    public ScrollerView(Context context) {
        super(context);

        mScroller = new OverScroller(context);
        mGestureDetector = new GestureDetectorCompat(context, mOnGestureListener);

        setFocusable(true);
    }

    GestureDetector.OnGestureListener mOnGestureListener = new GestureDetector.OnGestureListener() {
        @Override
        public boolean onDown(MotionEvent e) {
            return true;
        }

        @Override
        public void onShowPress(MotionEvent e) {

        }

        @Override
        public boolean onSingleTapUp(MotionEvent e) {
            return true;
        }

        @Override
        public boolean onScroll(MotionEvent e1, MotionEvent e2, float distanceX, float distanceY) {
            return true;
        }

        @Override
        public void onLongPress(MotionEvent e) {

        }

        @Override
        public boolean onFling(MotionEvent e1, MotionEvent e2, float velocityX, float velocityY) {

            mScroller.fling(mRect.centerX(), mRect.centerY(),
                    Math.round(velocityX), Math.round(velocityY),
                    0, getWidth() - SIZE, 0, getHeight() - SIZE,
                    SIZE / 2, SIZE / 2);

            ViewCompat.postInvalidateOnAnimation(ScrollerView.this);

            return true;
        }
    };

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        Log.d(TAG, "onTouchEvent " + event.getAction());

        boolean handled = mGestureDetector.onTouchEvent(event);
        if (handled) {
            return true;
        }

        return super.onTouchEvent(event);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        if (mRect == null) {
            mRect = new Rect(0, 0, SIZE, SIZE);
            int left = (canvas.getWidth() - SIZE) / 2;
            int top = (canvas.getHeight() - SIZE) / 2;
            mRect.offset(left, top);
        }

        canvas.drawRect(mRect, PAINT);
    }

    @Override
    public void computeScroll() {
        super.computeScroll();

        if (mScroller.computeScrollOffset()) {
            mRect.offsetTo(mScroller.getCurrX(), mScroller.getCurrY());

            ViewCompat.postInvalidateOnAnimation(this);
        }
    }
}
</code></pre>

`fling`から`ViewCompat.postInvalidateOnAnimation(this);`を実行すると`computeScroll()`が呼び出されます。

`mScroller.computeScrollOffset()`でスクロールの状態を計算し、（flingが続いていれば）その結果を`Rect`の位置として設定しています（ここで得られる値は、Scrollerのflingメソッドに与えた値を基にした絶対座標です）。

`mScroller.computeScrollOffset()`がtrueである限り`postInvalidateOnAnimation`と`computeScroll`の連鎖が続き、やがてScrollerが完了すると移動も止まります。

なお、OverScrollerはその名の通り、最後の値を設定すればオーバースクロールを有効にして、バウンスさせることができます。例では赤い■のサイズの半分の量だけオーバースクロールさせるように設定しています。

あらためて、実装例をGitHubに置いておきます。

サンプルプロジェクトを[GitHub][4]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][4]

### 実装参照

  * http://developer.android.com/intl/ja/training/gestures/detector.html
  * http://developer.android.com/intl/ja/training/gestures/scroll.html

* * *

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、ご了承下さい。
    

* * *

それでは明日19日の担当は、この記事を書くに当たってGestureDetectorCompatの存在を知ったけど、Compat系のクラスには嫌な思い出しかないという「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://github.com/keiji/adventcalendar_2015_mincomi
 [5]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-18-004150.png