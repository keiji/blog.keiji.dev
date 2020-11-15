---
title: 軽すぎる退会処理
author: Keiji Ariyama
type: post
date: 2015-12-13T15:00:24+00:00
url: /2015/12/mincomi-adventcalendar-14.html
categories:
  - Android
  - みんコミ Advent Calendar

---
----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**
----

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の１４日目の記事です。

「[みんコミ][2]」のAndroidアプリ（バージョン1.0.3）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「Google Play Store」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/en_generic_rgb_wo_60.png" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]（公開終了しています）

<!--more-->

* * *

ログインまたは外部サービス（Twitter, Facebook）と連携した状態で[その他] → [会員情報確認・変更]を選択すると現在の会員情報の確認ができます。

僕はTwitterと連携しているので、ログイン情報（メールアドレス・パスワード）は空白で、「外部サービス連携」のTwitterの行に「連携中」と表示されています。

この状態でTwitterの行をタップすると……退会確認の画面が表示されます。

Twitterとの連係を解除するという操作を期待していたので、いきなり退会確認が出てくるのには驚きました。Twitterとしか連携していないので、連係解除＝退会という扱いになるのも理解はできるのですが……

会員情報確認・変更の画面では、FacebookとTwitterの行が同時に表示されているので、複数サービスと連携できるのだろうと想像しています（実際「Facebook」の行をタップするとFacebookの認証画面が表示されます）。

退会確認画面で「退会する」のボタンを押せば、退会が完了してしまうのでしょう。

さらに怖いのは、前の画面の「Twitterの行」と、退会確認画面の「退会ボタン」が、ディスプレイ上のほとんど同じ場所にあることです。

<div id="attachment_887" style="max-width: 1034px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2015/12/mincomi_adventcal_14.002.jpeg"><img src="https://blog.keiji.dev/wp-content/uploads/2015/12/mincomi_adventcal_14.002.jpeg" alt="タップしたのと非常に近い場所に「退会」ボタンが待ち受けている" width="1024" height="768" class="size-full wp-image-887" /></a>
  
  <p class="wp-caption-text">
    タップしたのと非常に近い場所に「退会」ボタンが待ち受けている
  </p>
</div>

退会の手順が難読化されている昨今のソーシャルゲームやサービスと比べて、「トン・トン」と、同じ場所を２回タップするだけで退会が完了する（かどうかは試してないのでわかりませんが）という設計は、いっそ男らしいと思います。

しかし、人間は過ちを犯す生き物です。

「退会するとすべての会員情報、これまでご利用になられたサービス利用情報が削除されます。一度退会されると会員情報は復旧できませんご了承下さい」という注意文にもあるように、リカバリーの効かない退会処理については、もう少し慎重になってもいいと僕は思います。

この場合は確認ダイアログを出すなどの安全策を設けるなど、操作に少し変化をつけるのはどうでしょうか。

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

根雪さんの「おかあさん(10)と僕。」はまだ更新日（毎月第３木曜日）ではないので、代わりに僕がやっているサークル「めがねをかけるんだ」が、コミックマーケット(C89)に委託する冊子の宣伝を。

［12/20 Android Studio完全移行ガイド詳細を追記］

C89で、「[Android Studio完全移行ガイド][4]」を頒布します。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/c89_farewell_adt.png" alt="c89_farewell_adt" width="50%" height="50%" class="aligncenter size-full wp-image-954" />][5]

### どんな内容？

ADT（Android Developer Tools）のプロジェクトをAndroid Studioに移行するための決定版です。

ADTのプロジェクトをAndroid Studioに移行について、準備から実際の手順を、また、Antなどのビルドシステムで実現していた処理をAndroid Studio(Gradle)で実現するにはどのようにすればいいかについて、ユースケース毎にまとめてあります。

表紙・裏表紙は、根雪れい（[@neyuki_rei][6]）さんにお願いしました（なお、中身は普通の技術本です）。

### どこで手に入るの？

コミックマーケット89で頒布します。
  
現在、委託先として決定しているのは、

  * [三日目 東シ-58a TechBooster][7]

です。

そのほかページ数などの詳細については、別エントリでフォローしていく予定です。

また、TechBoosterの頒布する「Sweet Android」にも、Android 6.0の「Fingerprint API」について記事を書いているので、そちらもよろしくお願いします！

* * *

それでは明日、15日の担当は入稿日まであと４日も残されていない「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://play.google.com/store/apps/details?id=jp.ebookjapan.mincomi&hl=ja
 [4]: https://blog.keiji.dev/2015/12/c89.html
 [5]: https://blog.keiji.dev/wp-content/uploads/2015/12/c89_farewell_adt.png
 [6]: https://twitter.com/neyuki_rei
 [7]: https://techbooster.github.io/c89/