---
title: 端末を真っ直ぐに持ってください
author: Keiji Ariyama
type: post
date: 2015-12-10T15:00:18+00:00
url: /2015/12/mincomi-adventcalendar-11.html
categories:
  - Android
  - みんコミ Advent Calendar

---
----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**
----

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の１１日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.2）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]（公開終了しています）

<!--more-->

* * *

以前の[記事][4]でも触れた「回転防止ロック」、標準ではOFFになっています。

題材がコミックなだけに、僕は寝っ転がって読むことが多いのですが、コミックビューアーの画面を開いたときに端末が横を向いていると画面が突然回転してLandscapeモードになります。

一方で、他の画面はPortrait固定になっているので、コミックビューアーを終了すると画面は再び縦向きのPortraitモードになります。

これによって何が起こるかというと、

  * コミックビューアーを起動して端末を横向きにする（Landscapeモードになる）
  * 「回転防止ロック」を有効にする（Landscape固定）
  * コミックビューアーを終了する（Portraitモードになる）
  * 再びコミックビューアーを起動すると、Portraitモードに固定された状態で表示される（回転防止ロックの誤動作？）

ただし、システム側で「画面の自動回転」をONにしていると、期待通りLandscapeモードで表示されます。

コミックビューアーはユーザーが一番使う機能なので、これら挙動は違和感がぬぐえません。

標準では画面の自動回転は無効（システム設定に準拠）にして、ユーザーが自動回転を有効にした場合にだけ回転させるという挙動が望ましいと考えています。

または、基本的には端末標準の画面の向きで表示しておいて、Activityの起動後、端末をユーザーが傾けたときだけ画面を回転するという挙動も良いかもしれません。ただし、この挙動を実現するには端末の加速度センサーを使う必要があります。

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

根雪さんの「おかあさん(10)と僕。」は……

<blockquote class="twitter-tweet" lang="ja">
  <p lang="ja" dir="ltr">
    おかあさん(10)と僕は毎月第3木曜日更新だよ…！
  </p>
  
  <p>
    &mdash; 根雪れい.おかあさん(10)と僕。連載中 (@neyuki_rei) <a href="https://twitter.com/neyuki_rei/status/674399812746264576">2015, 12月 9</a>
  </p>
</blockquote>

お、ついに更新日が確定ですか。
  
楽しみです！

* * *

それでは明日、12日の担当は香川の発表準備でごめんネタ考えている余裕ない「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/2015/12/mincomi-adventcalendar-4.html