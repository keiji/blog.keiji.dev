---
title: 作品の更新と取得のタイミング
author: Keiji Ariyama
type: post
date: 2015-12-12T15:00:19+00:00
url: /2015/12/mincomi-adventcalendar-13.html
categories:
  - Android
  - みんコミ Advent Calendar

---
----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**
----

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の１３日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン<s>1.0.2</s> 1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

［12月13日追記］ベースにしたバージョンは1.0.3です。お詫びして訂正します。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]（公開終了しています）

<!--more-->

* * *

「平日毎日更新」ということで、僕は「みんコミアプリ」を一日一回は必ず立ち上げるのですが、その際、日付が変わって更新されているはずなのに、新しい作品が画面に出てこないと言うことがあります。

これは推測ですが、作品の更新をActivityのライフサイクル上の`onCreate`でのみ実行しているのではないでしょうか。そうであれば「みんコミアプリ」をバックグラウンドから復帰するなどのケースで、更新動作が実行されていないことの説明がつきます。

[Activity Lifecycle][4]

[以前の記事][5]でも触れたように、「みんコミ」アプリはバックキーを二度押ししないと終了しません。僕はいつもホームキーを押してランチャーに戻っているので、プロセスは生きたままになっているので、現象が発生しやすいのだと思います。

一度バックキーの二度押しでアプリを終了して再起動すると、正常に作品一覧が更新されます。

今回の場合、Activityのライフサイクルで言えばonResumeならバックグラウンドからの復帰であっても更新処理は実行されます。

ただしその場合、他のActivityから戻ったタイミングでも更新処理が実行されるので、効率が悪くなってしまいます。

そこで「最後に作品情報が更新された日付」の値を計算して保存しておき、現在の日付がその値より後であれば更新動作を行う。とすると、無駄は少なくなるかと思います。

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

根雪さんの「おかあさん(10)と僕。」はまだ更新日（毎月第３木曜日）ではないので、代わりにみんコミで気になっている作品を。

<blockquote class="twitter-tweet" lang="ja">
  <p lang="ja" dir="ltr">
    本日『みんなのコミック』創刊＆連載開始です！『√JK（るーとじぇいけー）』よろしくお願いいたします！&#10;作品ページの『おすすめ』ボタンを押すと、たつき＆海産物が喜びますｗ&#10;<a href="https://t.co/gqMiIKI2nl">https://t.co/gqMiIKI2nl</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://t.co/3kDKqPRuIq">pic.twitter.com/3kDKqPRuIq</a>
  </p>
  
  <p>
    &mdash; たつき＠新連載はじめました (@mimihane2) <a href="https://twitter.com/mimihane2/status/664369930020585472">2015, 11月 11</a>
  </p>
</blockquote>

風紀委員長が眼鏡っ娘です。
  
あと扉の見開きの左から二人目も眼鏡っ娘なので「みんコミ」に貴重な眼鏡っ娘要員を二人も擁していることになります。

素晴らしい。

あ、顔的には後者の方が好みなのですが、皆さんはどうでしょうか。

* * *

それでは明日、14日の担当は「有山圭二」さんです。

日本Androidの会 香川支部での発表資料はこちらになります。

 <iframe src="//www.slideshare.net/slideshow/embed_code/key/3hQZ48yQLaSkDm" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen></iframe>

<div style="margin-bottom:5px">
  <strong> <a href="//www.slideshare.net/keijiariyama/android-studio-56078096" title="Android Studio開発講座" target="_blank">Android Studio開発講座</a> </strong> from <strong><a href="//www.slideshare.net/keijiariyama" target="_blank">Keiji Ariyama</a></strong>
</div>

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: http://developer.android.com/intl/ja/reference/android/app/Activity.html#ActivityLifecycle
 [5]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-6.html