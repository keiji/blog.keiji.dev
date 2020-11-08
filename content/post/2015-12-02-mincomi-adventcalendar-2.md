---
title: アプリのパーミッション 〜 M comes after L
author: Keiji Ariyama
type: post
date: 2015-12-01T15:00:19+00:00
url: /2015/12/mincomi-adventcalendar-2.html
categories:
  - Android
  - みんコミ Advent Calendar

---
* * *

※ 2015/12/24追記: バージョン1.0.3で、GET_ACCOUNTSパーミッションが取り除かれているのを確認しました。

* * *

「[みんコミ Advent Calendar][1]」２日目を担当する有山圭二です。 ２日目の今日は、アプリのパーミッションについて解説します。

「みんコミ」のAndroidアプリは、「Google Play Store」からインストールできます。さっそくインストールしてみましょう。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][2]

<!--more-->

インストールの確認画面に表示される権限を見て、まず気になるのが「連絡先」というアクセス権限を要求していることでしょう。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/Screenshot_20151128-145117.png" alt="Screenshot_20151128-145117" width="270" height="480" class="aligncenter size-full wp-image-673" />][3]

よく見ると「この端末上のアカウントの検索」という補助項目になっているので、連絡帳へのアクセス権限である「READ|WRITE\_CONTACTS」ではなく、端末に登録してあるGoogleアカウントやdocomoアカウントの情報を取得する「GET\_ACCOUNTS」であるとわかります。

わかりますと言うのは、アプリ開発者など知っている人であればという話で、一般のユーザーで二つの違いがわかる人は非常に少ないのではないでしょうか。

一般に「連絡先へのアクセス権限」といわれれば、電話帳の中身が見られるのではないか。インターネットを通じて送信されるのではないかと不安になり、結果としてせっかくのユーザーを逃してしまうかもしれません。

（これについては、GET_ACCOUNTSを「連絡先」にまとめてしまうAndroidもどうかと思うのですが……）

アプリ内でこの権限が必要となる処理がどれほど重要なのか。十分に検討した上で、不必要な権限をつけないという選択をする必要があります。

また、Android 6.0（Marshmallow）では、新しくRuntimeパーミッションの仕組みが導入されており、アプリは権限が必要な処理を実行する前にユーザーに許可を求めるという流れになっています。

「みんコミ」のアプリは5.x系（Lolipop）向けに作られているので権限はインストール時に付与されますが、後から個別の権限を停止することもできるので、説明が不足しているとユーザーが個別に権限をＯＦＦにしてしまうことも考えられます。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/Screenshot_20151128-213101.png" alt="Screenshot_20151128-213101" width="270" height="480" class="aligncenter size-full wp-image-674" />][4]

今回の場合、権限がなぜ必要なのかをきちんとユーザーに説明すれば、安心してインストールできるようになると考えます。

例えば「連絡先情報の読み込みはしないこと」を明確にしてから「なぜ権限が必要なのか（例えばメールアドレスの取得する処理や、ebookjapanアカウントがあった場合はそれと連携する処理をするなどが考えられます）」を説明文中で解説するのが良いと思います。

権限関係では他に「端末のスリープの無効化（WAKE\_LOCK）」は要らないのでは（FLAG\_KEEP\_SCREEN\_ONで足りる？）とも思いました。

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、あらかじめご了承下さい。
    

* * *

蛇足になりますが、[みんコミ][3]といえば、僕が普段からお世話になっている根雪れい（[@neyuki_rei][4]）さんも連載陣に名を連ねていますね。

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

「おかあさん」より早く登場した「クラス委員の日高さん」は、春君のことが好きなのか。それともただ世話を焼いているだけなのか……

優しくしてくれるクラスメイト……うっ！　頭痛がっ！

* * *

３日の担当は「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [3]: https://blog.keiji.dev/wp-content/uploads/2015/12/Screenshot_20151128-145117.png
 [4]: https://blog.keiji.dev/wp-content/uploads/2015/12/Screenshot_20151128-213101.png