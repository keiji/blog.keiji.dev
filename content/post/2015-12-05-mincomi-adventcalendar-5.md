---
title: 設定画面のあれやこれや
author: Keiji Ariyama
type: post
date: 2015-12-04T15:00:00+00:00
url: /2015/12/mincomi-adventcalendar-5.html
categories:
  - Android
  - みんコミ Advent Calendar

---
この記事は「[みんコミ Advent Calendar][1]」の５日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.1）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

ホーム画面からメニューの「その他」をタップすると、設定画面が表示されますが、この画面が独自の画面として実装されているのが気になりました。

この内容だとPreferenceを使った方が実装もメンテナンスも簡単で、見た目もAndroid標準に則っていてユーザーに混乱を与えないと思います。

<http://developer.android.com/intl/ja/guide/topics/ui/settings.html>

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/mincomi_pref1.png" alt="mincomi_pref1" width="25%" height="25%" style="float:left;" />][4]

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/mincomi_pref2.png" alt="mincomi_pref2" width="25%" height="25%" />][5]

メニューの初期画面「pref_general.xml」は次の通りです。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"&gt;

    &lt;PreferenceCategory
        android:title="サービスにログイン"&gt;
        &lt;Preference
            android:title="ログイン"/&gt;
        &lt;Preference
            android:title="会員登録"/&gt;

    &lt;/PreferenceCategory&gt;

    &lt;PreferenceCategory
        android:title="その他"&gt;
        &lt;Preference
            android:title="お知らせ"/&gt;
        &lt;Preference
            android:key="various"
            android:title="設定"/&gt;
        &lt;Preference
            android:title="ヘルプ"/&gt;
        &lt;Preference
            android:title="お問い合わせ"/&gt;
        &lt;Preference
            android:title="サービスについて"/&gt;
        &lt;Preference
            android:title="利用規約"/&gt;
        &lt;Preference
            android:title="個人情報保護指針"/&gt;

    &lt;/PreferenceCategory&gt;

&lt;/PreferenceScreen&gt;
</code></pre>

実際にはそれぞれのPreferenceから別の画面に遷移することになります。
  
本来であれば、Preferenceの`android:fragment`属性でPreferenceをタップした際に遷移するFragmentを指定できるのですが、それが有効なのは読み込むActivityがPreferenceActivityを継承している場合です。

今回、`SettingActivity`は、Design Support Libraryとの兼ね合いでAppCompatActivityを継承しているため、OnPreferenceClickListenerで処理しています。

<pre><code class="java">    public static class GeneralSettingFragment extends PreferenceFragment
            implements Preference.OnPreferenceClickListener {

        @Override
        public void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);

            addPreferencesFromResource(R.xml.pref_general);

            findPreference("various").setOnPreferenceClickListener(this);
        }

        public static GeneralSettingFragment newInstance() {
            return new GeneralSettingFragment();
        }

        @Override
        public boolean onPreferenceClick(Preference preference) {

            switch (preference.getKey()) {
                case "various":
                    getFragmentManager()
                            .beginTransaction()
                            .addToBackStack(OtherSettingFragment.class.getSimpleName())
                            .replace(android.R.id.content, OtherSettingFragment.newInstance(),
                                    OtherSettingFragment.class.getSimpleName())
                            .commit();
                    break;
            }

            return false;
        }
    }
</code></pre>

「その他 &#8211; 設定」をタップしたときに別の画面に遷移するようにしています。
  
表示する「pref_other.xml」は次の通りです。

<pre><code class="xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android"&gt;

    &lt;Preference
        android:summary="一時保存のデータを削除して空き容量を増やします"
        android:title="キャッシュのクリア"/&gt;

    &lt;SwitchPreference
        android:key="notification"
        android:summary="お気に入りの作品が更新されたときに通知でお知らせします"
        android:title="通知"/&gt;

    &lt;Preference
        android:key="version"
        android:title="バージョン情報"/&gt;

&lt;/PreferenceScreen&gt;
</code></pre>

さて、Preferenceで作った画面に「最終閲覧」がないことにお気づきかと思います。

「最終閲覧」は、Preferenceを頑張ってカスタマイズすれば（おそらく）表示できます。今回、僕がそれをしていない理由は、一つはPreferenceのカスタマイズは作業の負荷が高くなりがちなこと、また、個人的に「最終閲覧」の表示は「その他」ではなく、ホーム画面にあった方が良いと思うからです。

Huluなどでも視聴中の番組は、まずトップ画面に出てきます。アプリを起動してすぐに表示されるホーム画面に表示した方がユーザーの目に留まりやすいと考えます。

サンプルプロジェクトを[GitHub][6]に置いておきます。

[https://github.com/keiji/adventcalendar\_2015\_mincomi][6]

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

根雪さんの「おかあさん(10)と僕。」は……まだ更新されていませんか。そうですか。

それでは代わりにもう一つ、「みんコミ」で気になっている作品を紹介します。

<blockquote class="twitter-tweet" lang="ja">
  <p lang="ja" dir="ltr">
    【次回更新予定】12月1日火曜日は爽田するめ先生の『育児しんどいマンガ-もうママって呼ばないで』2話が公開！育児の様々な面を本音で伝えます。子育て…難しい(^_^;) <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://t.co/4FRvZFqq9G">https://t.co/4FRvZFqq9G</a> <a href="https://t.co/WpcVcj3DAI">pic.twitter.com/WpcVcj3DAI</a>
  </p>
  
  <p>
    &mdash; みんなのコミック（みんコミ） (@mincomi_jp) <a href="https://twitter.com/mincomi_jp/status/670193289677529088">2015, 11月 27</a>
  </p>
</blockquote>



「子育てもの」というジャンルは、自分がその立場になってみないと、なかなか面白さがわからないものですね……。

わかります。ええ、わかりますとも。

* * *

それでは明日、６日の担当は、今月で１才の誕生日を迎える息子さんが、最近、アニメの「ゆるゆり」にハマっていて、さっそく将来が心配になっている「有山圭二」さんです。

よろしくお願いいたします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/wp-content/uploads/2015/12/mincomi_pref1.png
 [5]: https://blog.keiji.dev/wp-content/uploads/2015/12/mincomi_pref2.png
 [6]: https://github.com/keiji/adventcalendar_2015_mincomi