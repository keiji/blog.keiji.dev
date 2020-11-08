---
title: 「もう一度押すとアプリが終了します」とは。
author: Keiji Ariyama
type: post
date: 2015-12-05T15:00:36+00:00
url: /2015/12/mincomi-adventcalendar-6.html
categories:
  - Android
  - みんコミ Advent Calendar

---
## はじめに

この記事は「[みんコミ Advent Calendar][1]」の６日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.1）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]

<!--more-->

* * *

## バックキーの二度押し

ホーム画面を開いてバックキーを押すと、

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/toast.png" alt="toast" width="50%" height="50%" class="aligncenter size-full wp-image-799" />][4]

Toastが出て、もう一度バックキーを押さないとアプリが終了しませんが、これは、不要な処置だと思います。

### バックキーの二度押しの登場

なぜバックキーの二度押しが不要なのか。それにはまず、なぜバックキーの二度押しが検討されたのかを知る必要があるでしょう。

バックキーの二度押しで検索すると、2011年頃から言われ始めたようです。

[Close app when hitting back button on android][5]

[Android: clicking TWICE the back button to exit activity][6]

また、具体的なナビゲーションデザインとしてのバックキーの二度押しについても触れられています。

[Android UI Pattern: Back button behavior][7]

### バックキーの二度押しはなぜ生まれるか

なぜバックキーの二度押しが生まれるのか。簡単にまとめるなら「ユーザーが間違ってバックキーを押したときに、Activityを終了させないように」と、これに尽きます。

では、なぜ、ユーザーが間違ってバックキーを押してしまうのでしょうか。
  
それはユーザーが間違ったのでなく、ユーザーが期待したとおりにアプリが動いていないのだと、僕は推測しています。

バックキーを押してしまう心理として、次の二つが考えられます。

  * バックキーで前の画面に戻れると思ったのに、Activityが終了してしまった。
  * バックキーで前の画面に戻ろうとしたら画面が固まったのでバックキーを連打したら、終了したくないActivityまで終了してしまった。

前者はナビゲーション設計の問題です。
  
Activityであっても、FragmentであってもBackStackに積むことで戻れるように設計する。また、一気に上に戻りたい場合は、ActionBar(ToolBar)のUPを使うなど、ナビゲーション設計を見直せば大きな間違いは防げるでしょう。

後者はプログラム設計上の問題です。
  
アクティビティのライフサイクルの後段でメインスレッドに負荷をかけている可能性があります。重い処理は非同期にするなどにすれば、ユーザーがバックキーを連続でタップすることは防げます。

バックキーが押されてしまうのは、ユーザーが間違ったのではなく、アプリがユーザーの期待通りに動いていないのが問題なのです。

今回の「みんコミ」アプリの場合、バックキーを押したときに意図せずアプリが終了してしまうことはありません。必ずホーム画面を通ります。

ホーム画面でバックキーを押すと言うことは、ユーザーがアプリの終了を望んでいるのです。

その期待を裏切る動作はしない方がいいと僕は思います。

### 終了確認が必要なケース

もちろん、アプリの終了確認が必要なケースも存在します。それについてはGoogleのデザインガイドラインに述べられています。

[確認と通知][8]

つまり、Activityが終了することで失われるデータが、ユーザーにとって重要な意味を持つ場合は通知して確認をする。これにはバックキーの二度押しでは足りないので、ダイアログ(AlertDialog)を使うことになるでしょう。

ワープロなどのソフトで文書を作成している最中にバックキーが押された場合を考えてみましょう。作成中の文書をファイルに保存するか、ユーザーに尋ねる必要があります。

この場合、新規文書であれば確認せずに「下書き」として保存する選択肢がありますが、既存の文書を編集しているのであればそうもいきません。ユーザーは編集内容を破棄したくてバックキーを押した可能性があるからです。勝手に保存してしまっては大変です。

今回の「みんコミ」アプリの場合、ユーザーがバックキーでアプリを終了しても失うデータはありません。開き直せばいいだけで、容易にリカバーが出来ます。

僕がバックキーの二度押し確認が必要ない。というのはそう言う理由です。

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
    まじとら！連載開始しました！オススメやシェアなどで応援どうぞよろしくお願いします　 <a href="https://t.co/nhxwiqO2OD">https://t.co/nhxwiqO2OD</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://t.co/PFqc6s0A93">pic.twitter.com/PFqc6s0A93</a>
  </p>
  
  <p>
    &mdash; 香椎ゆたか@まじとら！連載中 (@yutakashii) <a href="https://twitter.com/yutakashii/status/664373356435668992">2015, 11月 11</a>
  </p>
</blockquote>

１話を読んだときはピンとこなかったんですが、２話で面白みが増したと思います。

主人公はどうやら変態ですが、こう、明るい変態と言うんでしょうか。後ろ暗さが全くないところに好感が持てますね。

* * *

それでは明日、７日の担当は「魔法少女まどか☆マギカ」は１０話が本編と言って憚らない「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/wp-content/uploads/2015/12/toast.png
 [5]: http://stackoverflow.com/questions/5902464/close-app-when-hitting-back-button-on-android
 [6]: http://stackoverflow.com/questions/8430805/android-clicking-twice-the-back-button-to-exit-activity
 [7]: http://www.androiduipatterns.com/2011/03/back-button-behavior.html
 [8]: http://developer.android.com/intl/ja/design/patterns/confirming-acknowledging.html