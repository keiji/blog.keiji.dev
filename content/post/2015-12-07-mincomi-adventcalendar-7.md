---
title: 閉鎖世界 – iOS
author: Keiji Ariyama
type: post
date: 2015-12-06T15:00:05+00:00
url: /2015/12/mincomi-adventcalendar-7.html
categories:
  - Android
  - みんコミ Advent Calendar

---
----
**[「みんなのコミック」は2018年10月31日を持ちまして更新を終了いたしました。](https://twitter.com/mincomi_jp/status/1057847395889737730)**
----

## はじめに

この記事は「[みんコミ Advent Calendar][1]」の７日目の記事です。

「[みんコミ][2]」のiOSアプリ（バージョン1.0.0）をベースに執筆しています。スクリーンショットは極力控える方針ですので、本記事を読む際には、「iTunes App Atore」からアプリをインストールしておくことをお勧めします。

[<img src="https://blog.keiji.dev/wp-content/uploads/2015/12/available-on-the-app-store-1345130940.jpg" alt="en_generic_rgb_wo_60" width="172" height="60" class="aligncenter size-full wp-image-672" />][3]（公開終了しています）

<!--more-->

* * *

珍しくiOSについてです。

Webでトップ画面にあるお知らせと同じ内容が「その他」画面から見られるのですが、Android版の更新のお知らせがiOS版のアプリから見えてしまっていました。

    「Androidで作品が読書できない不具合の解消」
    

お知らせはもう消えているのですが、こんな感じの記述ですね。

これの何がまずいかというと、iOSを配信するAppStoreではAndroidなどの他のモバイル・プラットフォーム関係のお知らせを表示すると、リジェクトの対象になる可能性があるんですね。

ええ、僕も初めて知ったときは心底驚きましたが、[知り合い][4]が現実にそれが理由でリジェクトされていました。

規約にも明示されているんですね。

[リリース前にチェック！アプリのリジェクト事例まとめ][5]

なんて閉鎖的な世界だろう！　世の中のモバイルＯＳはiOSだけでいいと思っているんだ！　って思いましたが、よく考えれば事実あの会社はそう思ってそうなので、そっとしておきましょう。

さて対策ですが、これにはいくつか方法があります。

  * サーバー側で対応する 
      * クライアントのUSER-AGENTを見て、表示するお知らせを変更する
  * iOS版クライアント側で対応する 
      * お知らせの内容に&#8221;Android&#8221;等の別のプラットフォームに関する記述があれば強制的に表示しない
  * 両方で対応する 
      * サーバーは、配信するお知らせの情報に[iOS,Android,Web]といったタグを含める
      * クライアントは、自分に関係のないタグの情報を表示しない

僕が一番スマートに思えるのは「両方で対応」でしょうか。

マルチプラットフォーム対応でこういうことに工数が取られるのは誰も幸せにならないので実に残念ですね……

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
    『みんコミ』WEBトップから派手な看護婦さんの『うわようじょつよい』をご紹介！みなさんご存知の”あの”ビックサイトで、その日何が起こったのか？！ドキドキです！<a href="http://t.co/42qZc3Iegp">http://t.co/42qZc3Iegp</a> <a href="http://t.co/xJZerkNb4C">pic.twitter.com/xJZerkNb4C</a>
  </p>
  
  <p>
    &mdash; みんなのコミック（みんコミ） (@mincomi_jp) <a href="https://twitter.com/mincomi_jp/status/643636125932761088">2015, 9月 15</a>
  </p>
</blockquote>

女子小学生がなんか戦ってます。

ファンタジー風の残虐シーンがあります。**この一作品のためだけにみんコミアプリのレーティングが変わるであろうインパクト**です。

緻密な書き込みに支えられた世界観は独特すぎて、読んでいて振り落とされそうになりますが、そんなことより僕が気になっているのは「果てして女子小学生は『ようじょ』に入るのか」と言うことですね。

* * *

それでは明日、８日の担当は「ツルのない眼鏡、それは鼻眼鏡だろ？」といって周囲から顰蹙をかった「有山圭二」さんです。

よろしくお願いします。

 [1]: http://qiita.com/advent-calendar/2015/mincomi
 [2]: https://www.mincomi.jp
 [3]: https://itunes.apple.com/jp/app/minnanokomikku/id1050104822?mt=8
 [4]: http://www.railchallenge.com/?cat=9
 [5]: http://patto-cms.jp/blog/reasons_of_reject_of_ios_apps/