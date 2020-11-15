---
title: '続: Fragment使う人に最低限、知っておいて欲しいこと'
author: Keiji Ariyama
type: post
date: 2015-12-23T15:00:37+00:00
url: /2015/12/mincomi-adventcalendar-24.html
enclosure:
  - |
    |
        https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-23-230647.mp4
        1427047
        video/mp4
        
categories:
  - Android
  - みんコミ Advent Calendar

---
----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**
----

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の24日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]（公開終了しています）

［2015/12/25追記: サンプルコードのonCreateメソッド内でFragmentを初期化する処理を、Activityの再生時には実行しないように変更してあります（`savedInstanceState`で判定）］

<!--more-->

* * *

現在の「みんコミ」アプリでは画面の遷移には`Acrivity`が使われていて、`Fragment`は使われていない様子です。

昨日[みんコミに遊びに行った][4]ときに聞いた限りでは、画面の遷移を`Fragment`も使うように切り替えようとしている様子でした。

試しに下タブバージョンのUIをFragmentの切り替えができるようにしたサンプルコード（MainActivity）を[GitHub][5]作ったので置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][5]

作ってみた感想として、ランキング画面の下タブと上タブが同時に現れる部分が少しややこしい印象でした（動画です）。

<div style="width: 360px;" class="wp-video">
  <video class="wp-video-shortcode" id="video-1068-3" width="360" height="640" preload="metadata" controls="controls"><source type="video/mp4" src="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-23-230647.mp4?_=3" /><a href="https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-23-230647.mp4">https://blog.keiji.dev/wp-content/uploads/2015/12/device-2015-12-23-230647.mp4</a></video>
</div>

スクロール量に応じてToolbar周りを調整するにはNestedScrollViewに入れなければなりませんが、そのままではViewPagerの高さが0になって、きちんと操作できません。

NestedScrollViewに`android:fillViewport="true"`を設定することで、期待通りに動くようになりました。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;android.support.v4.widget.NestedScrollView
    android:id="@+id/scroll_view"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fillViewport="true"&gt;

    &lt;RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"&gt;

        &lt;android.support.design.widget.TabLayout
            android:id="@+id/ranking_tabs"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"/&gt;

        &lt;android.support.v4.view.ViewPager
            android:id="@+id/ranking_container"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_below="@id/ranking_tabs"/&gt;

    &lt;/RelativeLayout&gt;

&lt;/android.support.v4.widget.NestedScrollView&gt;
</code></pre>

あと、下タブにしているとやはりViewPagerを使った作品バナーはホーム画面だけでいいですね……すべてのページ表示されているのは、自分で作っておいて「やり過ぎだな」と思いました……

* * *

さて、Fragmentと聞いて思い出すのがこの記事です。

[Fragment使う人に最低限、知っておいて欲しいこと][6]

2014年の2月、ぼくはこんなタイトルのブログ記事をアップしました。
  
この記事はいまだにアクセス数が多く、検索で「Fragment 使い方」でたどり着く人が多いようです。

**実に申し訳ない限りです。**

記事の内容は不十分で、また今はもう古くなっている部分があります。

あれから1年と10ヶ月が経ち、現在ぼくがFragmentを使うときに気をつけていることーー「Fragment使う人に最低限、知っておいて欲しいこと」についてまとめていきます。

## Fragmentを呼び出しているActivity固有のメソッド・メンバ変数を参照してはいけない

これは今でも重要です。

呼び出し元を型チェックも無しにキャストしていたら別のActivityで使うことができません。

呼び出し元をキャストしてメソッドを呼び出すのではなく、面倒でもCallbackを経由してActivity側とのやりとりをしましょう。

ListenerやCallbackを見れば、そのFragmentがどんな情報をやり取りするのかわかるようにすると、後々の可読性も上がるというのも、今でも使えるTipsです。

## Listener/Callbackの設定タイミング

Listener/Callbackは、必ず`Fragment#onAttach`内で設定して下さい。

    Listenerの設定は、Fragment#onAttach(Activity activity)内でactivityをinstanceofし、Listenerが実装されている事が確認されたタイミングがベストです。
    FragmentをロードするActivityから明示的に`setListener`をすることも出来ますが、その場合、Fragmentのライフサイクル、再生成時の挙動には注意してください。再生成時に確実にsetListenerが実行されるようにしなければ、イベントをActivityで受けることが出来ません。
    

以前はこう書きましたが、`setListener`のようなメソッドでActivity側から設定しようにも、onAttachと同等のタイミングに該当するライフサイクル・メソッドが存在しません。

Listener/CallbackはActivityからではなく、Fragment側で受け取るようにして下さい。

### onAttach(Activity) → onAttach(Context)

以前、FragmentがActivityにattachされるタイミングは`onAttach(Activity activity)`でしたが、これは`onAttach(Context context)`に変更になりました（正確には`onAttach(Activity activity)`はdeprecated-非推奨になりました）。

`onAttach(Context context)`をオーバーライドしてください。

### Support LibraryのFragmentを使う

「onAttach(Context context)をオーバーライドしてください」と言ったものの、実は、プラットフォームに組み込まれているFragment（android.app.Fragment）で`onAttach(Context context)`が使えるのはAPI Level 23からです。

API Level 22（Lollipop）以前では、これまで通り`onAttach(Activity activity)`を使う必要があります。

このようなバージョンの違いを意識してコードを書くのは面倒なので、僕はよほど必要に迫られない限り、Support Libraryに含まれるFragment（`android.support.v4.app.Fragment`）を使っています。

Support LibraryのFragmentは、プラットフォームのバージョンアップに合わせて更新されているので、API Level 22以前でも`onAttach(Context context)`を使うように統一できます。

## Fragmentのインスタンス生成はクラスメソッドに任せるのがおすすめ

Activity同様、Fragmentも再生成される場合があります。
  
再生成時には引数無しのコンストラクタが使われます。引数無しのコンストラクタを用意していなかっったり、Fragmentのパラメーターをコンストラクタの引数を使って与えているとエラーが発生する可能性があります。

<pre><code class="java">public class HomeFragment extends Fragment {

    private int mProgress;

    // 再生成時にmProgressの値が与えられない
    public HomeFragment(int progress) {
        mProgress = progress;
    }

    // 引数のない空のコンストラクタがない
}
</code></pre>

僕は、Fragmentのインスタンス生成はActivityで行わず、それぞれのFragmentに`newInstance`メソッドを用意して、そちらに任せるようにしています。

<pre><code class="java">&lt;br />public class HomeFragment extends Fragment {
    private static final String KEY_DEFAULT_PROGRESS = "key_default_progress";

    public static HomeFragment newInstance(int progress) {
        HomeFragment fragment = new HomeFragment();

        Bundle args = new Bundle();
        args.putInt(KEY_DEFAULT_PROGRESS, progress);
        fragment.setArguments(args);

        return fragment;
    }

    public HomeFragment() {
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_home, container, false);

        int defaultProgress = getArguments().getInt(KEY_DEFAULT_PROGRESS);

        mSeekBar = (SeekBar) view.findViewById(R.id.seekbar);
        mSeekBar.setOnSeekBarChangeListener(mOnSeekBarChangeListener);
        mSeekBar.setProgress(defaultProgress);

        return view;
    }
}
</code></pre>

`Bundle`にパラメーターを設定して、`setArguments`で設定しておくと、Fragmentの再生成時にパラメータを確実に渡すことができます。

もちろん`Bundle`を使ったパラメーターの受け渡しは、Activityなど外側のクラスからもできます。しかし、内部（この場合`onCreateView`）でどのようなキーやパラメーターの型を意識する必要があります。

`newInstance`メソッドを用意すれば、引数の型に合った値を設定するだけで、キーに指定するについては意識する必要がないというメリットがあります（オプショナルなパラメーターについてもメソッドのオーバーロードで対応可能です）。

* * *

あらためて、Fragmentのサンプル（MainActivity）を含めたコードを[GitHub][5]に置いておきます。参考になれば幸いです。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][5]

* * *

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、ご了承下さい。
    

* * *

[みんコミ][2]には、僕が普段からお世話になっている根雪れい（@neyuki_rei）さんも連載していますね！

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    おかあさん(10)と僕。という10歳のおかあさんマンガが2話まで読めますよー…読んでくださいませ～　<a href="https://t.co/UfxCfaOYRZ">https://t.co/UfxCfaOYRZ</a> <a href="https://t.co/M7ioKciVZX">pic.twitter.com/M7ioKciVZX</a>
  </p>
  
  <p>
    &mdash; 根雪れい.おかあさん(10)と僕。連載中 (@neyuki_rei) <a href="https://twitter.com/neyuki_rei/status/678186480552968192">December 19, 2015</a>
  </p>
</blockquote>

根雪さんには、[コミックマーケット89で頒布する冊子][7]の表紙絵を描いてもらっていますが、次の話は残念ながら来年になるので(ry

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    【みんコミ連載紹介】若杉佳さんが連載する『まりもな日々』の舞台はなんと女子寮！かわいくてまったりな時間が流れる女子寮ライフを描きます♡　<a href="https://t.co/gKio9ShKkE">https://t.co/gKio9ShKkE</a> <a href="https://t.co/SBV5HQhaFG">pic.twitter.com/SBV5HQhaFG</a>
  </p>
  
  <p>
    &mdash; みんなのコミック（みんコミ） (@mincomi_jp) <a href="https://twitter.com/mincomi_jp/status/656289783128322048">October 20, 2015</a>
  </p>
</blockquote>

古いアパート「よつば荘」の新しい入居者「まりも」を待っていたのは、個性溢れる４人の女の子たち。これから楽しい生活が始まると思いきや、実はアパートには大きな秘密があって……

どんな秘密かは、是非作品を見ていただきたいのですが

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    【本日更新！】若杉佳先生の『まりもな日々』2話更新しました！幽霊が出ないように自力でアパートをなんとかしようとするまりも。でも個性的な住人たちに翻弄されてしまい… <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://t.co/vDUre3GjjA">https://t.co/vDUre3GjjA</a> <a href="https://t.co/TqIgPk6DZ8">pic.twitter.com/TqIgPk6DZ8</a>
  </p>
  
  <p>
    &mdash; みんなのコミック（みんコミ） (@mincomi_jp) <a href="https://twitter.com/mincomi_jp/status/676571780425756674">December 15, 2015</a>
  </p>
</blockquote>

……。

気を取り直していきましょう。

他作品に比べて登場人物が格段に多いのが特徴です。
  
主人公の「まりも」に入居者４人の総勢５人、人間じゃないのを入れると６人です。

この人数を取り回すのは相当大変だと思いますが、一話からきちんと個性や人間関係を伝えられているのはすごいですね。

残念ながら眼鏡っ娘の存在は確認できていませんが……今後が楽しみな作品です！

* * *

それでは明日25日の担当は、１年前の自分のコードや設計を見ると、酷すぎて思わず焼き捨てたくなってしまう「有山圭二」さんです。

次で最後です。
  
よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-23.html
 [5]: https://github.com/keiji/adventcalendar_2015_mincomi
 [6]: https://blog.keiji.dev/2014/02/about_fragment.html
 [7]: https://blog.keiji.dev/2015/12/c89.html