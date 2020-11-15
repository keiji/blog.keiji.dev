---
title: ZoomとかFocusとか聞くと眼鏡っ娘しか想起しない
author: Keiji Ariyama
type: post
date: 2015-12-18T15:00:47+00:00
url: /2015/12/mincomi-adventcalendar-19.html
categories:
  - Android
  - みんコミ Advent Calendar

---

----
----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**

2020/11/09 追記:
みんコミAdvent Calendarその他の知見を元に、漫画表示用カスタムビュー「MangaView」を公開しました。

 * https://github.com/keiji/mangaview
----

[12/20追記: ズームサンプルが正常に動作しない不具合を修正しました]

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の19日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]（公開終了しています）

<!--more-->

* * *

「みんコミ」アプリのコミックビューアーで、拡大縮小したときにピンチイン・アウトの「フォーカス」のハンドリングを調整すると、もっと使いやすくなると思います。

コードを見ていないのではっきりとしたことは言えませんが、現状のコミックビューアーは、ピンチアウトで拡大操作をしたときに、１番目の指を起点に拡大縮小をしていく挙動になっているように感じました。

`ScaleGestureDetector`を使って取得した値に基づいて画像を拡大縮小するときに、`Focus`の値も使って画像の位置を調整すると、自然な拡大ができます。

サンプルプロジェクトを[GitHub][4]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][4]

サンプル「ズームサンプル(ZoomActivity)」では、起動すると標準のアイコンを画面に表示します。ピンチイン・アウトの操作で拡大縮小するようにプログラムしています。また、拡大縮小の際、二本の指の中間点（Focus）を基に位置を変えています。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-18-200216.png" alt="device-2015-12-18-200216" width="50%" height="50%" class="aligncenter size-full wp-image-973" />][5]

まず、ScaleGestureDetectorでonScaleジェスチャーを取得します。
  
使い方は簡単です。ScaleGestureDetectorのインスタンスにonTouchが受け取るMotionEventを渡すだけで、onTouchEventの状況から判定して所定のジェスチャーのコールバックが呼ばれます。

<pre><code class="java">public class ZoomView extends View {
    private static final String TAG = ZoomView.class.getSimpleName();

    private static final float CIRCLE_RADIUS = 10f;

    private static final Paint PAINT = new Paint();
    private static final Paint CIRCLE_PAINT = new Paint();

    static {
        PAINT.setAntiAlias(true);

        CIRCLE_PAINT.setColor(Color.RED);
    }

    private final Rect mSrcRect;
    private final Bitmap mBitmap;

    private final ScaleGestureDetector mScaleGestureDetector;

    public ZoomView(Context context) {
        super(context);

        mBitmap = BitmapFactory.decodeResource(context.getResources(), R.mipmap.ic_launcher);
        mSrcRect = new Rect(0, 0, mBitmap.getWidth(), mBitmap.getHeight());

        mScaleGestureDetector = new ScaleGestureDetector(context, mScaleGestureListener);

        setFocusable(true);
    }

    private float mLastScaleFactor;

    private final ScaleGestureDetector.OnScaleGestureListener mScaleGestureListener
            = new ScaleGestureDetector.OnScaleGestureListener() {

        @Override
        public boolean onScale(ScaleGestureDetector detector) {
            float scale = 1 + detector.getScaleFactor() - mLastScaleFactor;

            Log.d(TAG, "scale = " + scale);
            setScale(scale, detector.getFocusX(), detector.getFocusY());

            return true;
        }

        @Override
        public boolean onScaleBegin(ScaleGestureDetector detector) {
            mLastScaleFactor = detector.getScaleFactor();
            return true;
        }

        @Override
        public void onScaleEnd(ScaleGestureDetector detector) {
        }
    };

    private float mFocusX;
    private float mFocusY;

    public void setScale(float scale, float focusX, float focusY) {
        mFocusX = focusX;
        mFocusY = focusY;

        float newWidth = mRect.width() * scale;
        float newHeight = mRect.height() * scale;

        float scrollX = mRect.width() - newWidth;
        float scrollY = mRect.height() - newHeight;

        mRect.right = mRect.left + newWidth;
        mRect.bottom = mRect.top + newHeight;

        scrollX *= (focusX / getWidth());
        scrollY *= (focusY / getHeight());

        mRect.offset(scrollX, scrollY);

        invalidate();
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {

        boolean handled = mScaleGestureDetector.onTouchEvent(event);
        if (handled) {
            return true;
        }

        return super.onTouchEvent(event);
    }

    private RectF mRect;

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        if (mRect == null) {
            mRect = calculateRectFromBitmap(mBitmap, canvas.getWidth(), canvas.getHeight());
        }

        canvas.drawBitmap(mBitmap, mSrcRect, mRect, PAINT);

        canvas.drawCircle(mFocusX, mFocusY, CIRCLE_RADIUS, CIRCLE_PAINT);

    }

    private static RectF calculateRectFromBitmap(Bitmap bitmap, int viewWidth, int viewHeight) {
        float width = bitmap.getWidth();
        float height = bitmap.getHeight();

        float ratio = Math.min(viewWidth / width, viewHeight / height);
        width *= ratio;
        height *= ratio;

        return new RectF(0, 0, width, height);
    }
}
</code></pre>

[Scrollerと同様][6]、ScaleGestureDetectorの各メソッドの戻り値には気をつけて下さい。Android Studioで自動生成するとデフォルトで各メソッドは`false`を返しますが、そのままではACTION_DOWN以外のイベントを受け取れません（当然、ジェスチャも受け取れません）。
  
適宜`true`を返すようにしましょう。

### 解説

通常、onScaleから得た拡大率を普通にcanvas.setScaleやRectのwidthやheightに反映すると、左上の原点(0, 0)を基準に拡大します。

<div id="attachment_974" style="max-width: 1034px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/19th.001.jpeg"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/19th.001.jpeg" alt="通常は左上が原点" width="1024" height="768" class="size-full wp-image-974" /></a>
  
  <p class="wp-caption-text">
    通常は左上が原点
  </p>
</div>

二本指でピンチ操作をするときは、二本の指の中間点を拡大していくという挙動が自然です。この二本の指の中間点の座標を「フォーカス」と呼びます。コードではScaleGestureDetectorのgetFocusX()とgetFocusY()で取得できます。
  
（GoogleのサンプルサイトではgetCurrentSpanX()やgetCurrentSpanY()から逐次フォーカスを計算していますが、今回はFocusを使う方法を採用しています）

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/19th.002.jpeg" alt="19th.002" width="1024" height="768" class="aligncenter size-full wp-image-975" />][7]

このフォーカス（図の黒丸）を中心にすると、ユーザーにとって自然な拡大・縮小に見えます。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/19th.004.jpeg" alt="19th.004" width="1024" height="768" class="aligncenter size-full wp-image-976" />][8]

しかし、フォーカスを基準にただ拡大しても、画像はやはりずれてしまいます。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/19th.003.jpeg" alt="19th.003" width="1024" height="768" class="aligncenter size-full wp-image-977" />][9]

拡大・縮小後の大きさと、以前の大きさの差分を取り、フォーカスの量に応じて位置を調整する必要があります。
  
例えば、中心をフォーカスした状態で拡大すると、サイズが大きくなった量の半分左側に移動して描画することで、拡大してもフォーカスの位置は（見かけ上）変わりません。

もしフォーカスの位置がずれていた場合、基準となる大きさ（サンプルコードではViewのwidthやheight）に締めるフォーカスの比率を基に、位置を調整する量を計算します。

<pre><code class="java">    public void setScale(float scale, float focusX, float focusY) {
        mFocusX = focusX;
        mFocusY = focusY;

        float newWidth = mRect.width() * scale;
        float newHeight = mRect.height() * scale;

        float scrollX = mRect.width() - newWidth;
        float scrollY = mRect.height() - newHeight;

        mRect.right = mRect.left + newWidth;
        mRect.bottom = mRect.top + newHeight;

        scrollX *= (focusX / getWidth());
        scrollY *= (focusY / getHeight());

        mRect.offset(scrollX, scrollY);

        invalidate();
    }
</code></pre>

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/19th.007.jpeg" alt="19th.007" width="1024" height="768" class="aligncenter size-full wp-image-978" />][10]

ただし、今回の実装は不完全です。サンプルでは無限に拡大縮小できますし、拡大縮小を繰り返して描画の位置がずれてもまったく補正していません。

現実に実装しようとすれば、タッチによるスクロールやFling操作への対応、画像の割り当てや採用する座標系によって調整が必要になります。

あらためて、実装例を[GitHub][4]に置いておきます。参考になれば幸いです。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][4]

### 実装参照

  * http://developer.android.com/intl/ja/training/gestures/scale.html

* * *

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、ご了承下さい。
    

* * *

[みんコミ][2]といえば、僕が普段からお世話になっている根雪れい（@neyuki_rei）さんも連載していますね！

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

[先ほどのエントリ][11]のとおり、根雪さんに表紙をお願いした「Android Studio 完全移行ガイド」をコミックマーケット89で頒布します。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/c89_farewell_adt.png" alt="c89_farewell_adt" width="50%" height="50%" class="aligncenter size-full wp-image-954" />][12]

根雪さん。いつも最高の眼鏡っ娘をありがとうございます！

* * *

それでは明日20日の担当は、一日に二度、ブログを更新するだなんてこれまであっただろうかと感慨にふけっている「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://github.com/keiji/adventcalendar_2015_mincomi
 [5]: https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-18-200216.png
 [6]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-18.html
 [7]: https://blog.keiji.dev/wp-content/uploads/2015/12/19th.002.jpeg
 [8]: https://blog.keiji.dev/wp-content/uploads/2015/12/19th.004.jpeg
 [9]: https://blog.keiji.dev/wp-content/uploads/2015/12/19th.003.jpeg
 [10]: https://blog.keiji.dev/wp-content/uploads/2015/12/19th.007.jpeg
 [11]: https://blog.keiji.dev/2015/12/c89.html
 [12]: https://blog.keiji.dev/wp-content/uploads/2015/12/c89_farewell_adt.png