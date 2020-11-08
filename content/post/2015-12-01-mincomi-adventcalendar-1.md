---
title: みんコミ Advent Calendarはじめます
author: Keiji Ariyama
type: post
date: 2015-11-30T15:00:36+00:00
url: /2015/12/mincomi-adventcalendar-1.html
categories:
  - Android
  - みんコミ Advent Calendar

---

----
2020/11/09 追記:
みんコミAdvent Calendarその他の知見を元に、漫画表示用カスタムビュー「MangaView」を公開しました。

 * https://github.com/keiji/mangaview
----

* * *

2015/12/01 14:48: Fix typo.

* * *

11月11日にWebコミック配信サービス「[みんなのコミック][1]」がサービスを開始しました。

Webでモバイル(Android/iOS)アプリでWebコミックを配信、一日一回更新（一作品の新作が読める）というコンセプトです。

<blockquote class="twitter-tweet" lang="ja">
  <p lang="ja" dir="ltr">
    お待たせいたしました！ 本日、無料マンガアプリ『みんなのコミック』を創刊いたしました！ 2015年11月11日が『みんなのコミック』の誕生日となります。皆さま、どうかよろしくお願いいたします。 <a href="https://t.co/KgI1nqpXzz">https://t.co/KgI1nqpXzz</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%82%B3%E3%83%9F?src=hash">#みんコミ</a> <a href="https://twitter.com/hashtag/%E3%81%BF%E3%82%93%E3%81%AA%E3%81%AE%E3%82%B3%E3%83%9F%E3%83%83%E3%82%AF?src=hash">#みんなのコミック</a>
  </p>
  
  <p>
    — みんなのコミック（みんコミ） (@mincomi_jp)
  </p>
  
  <p>
    <a href="https://twitter.com/mincomi_jp/status/664367432648146945">2015, 11月 11</a>
  </p>
</blockquote>



* * *

さて、この「みんなのコミック」。サービス開始直後にAndroid版のアプリをダウンロードしたのですが、やはりアプリ開発を本職にしているといろいろなところが気になってしまうわけで。

不遜な言い方になるかもしれませんが、「もっとこうした方が良くなる」という点がたくさん見つかるんですね。

気づいたところ、気になったところは逐一ウェブの「お問い合わせ」から送信しているのですが、気がつけば結構な分量になっていました。

で、これまで送った要望や指摘事項を眺めていて思ったんです。

「あれ？　これってきちんとまとめれば、アプリ開発者（発注者）の役に立つんじゃない？」

と。

そんなわけで、これまで送ったアレコレをもとに「みんコミ Advent Calendar」をすることにしました。

<blockquote class="twitter-tweet" lang="ja">
  <p lang="ja" dir="ltr">
    ありがたいことに全日程の参加者が決まりました &#8211; みんコミ Advent Calendar 2015 <a href="https://twitter.com/hashtag/Qiita?src=hash">#Qiita</a> <a href="https://t.co/cgWGZg30rX">https://t.co/cgWGZg30rX</a>
  </p>
  
  <p>
    — Keiji ARIYAMA (@keiji_ariyama)
  </p>
  
  <p>
    <a href="https://twitter.com/keiji_ariyama/status/670466011800276992">2015, 11月 28</a>
  </p>
</blockquote>

「なに言ってるんだコイツ」という声が聞こえてきそうですが、本気です。 （あ、御社の原稿は今やっているので。期限には必ず。はい）

<!--more-->

## 執筆ルール

さて、WEBやiOS/Androidアプリを見ていく前に、いくつかルールを定めておきます。

### アプリのリバースエンジニアリングはやらない

APKを抜き出して分解したり、内部の処理を見ると言うことはしません。みんコミの規約でも「リバースエンジニアリング」は明確に禁止されています。

### ログを見るのはＯＫ

開発環境につないでLogCatを見たり、例外発生時にGoogle Playへのバグレポートで確認できるスタックトレースは「公開された情報」という認識なのでありです。

### 著作権には十分に配慮する

コミック配信サービスなので、コンテンツは命です。コンテンツプロバイダーである「みんコミ」だけでなく、執筆した作者さんの著作権にも十分な配慮をする必要があります。不要な画像の掲載は極力避け、どうしても引用が避けられない場合は作品名・作者名などを明示します。

### スクリーンショットは配慮する

開発者にとっては画面設計もアイデアの塊であると認識しています。画面のスクリーンショットの掲載は、改善に必要十分と考える分量に留めます。

### 改善の提案に努める

要するにアプリやサービスについて必要のないディスりはしない。ということですね。 「こんなアプリは駄目だぁ！」というのは簡単ですが、具体的な改善策を提案する方が良いと考えています。

### 個人の見解であることを明確にします

    「有山圭二」は「みんなのコミック」及び運営の「株式会社イーブックイニシアティブジャパン」とは一切関係がありません。
    また、本アドベントカレンダーの内容はあくまで参加者個人の見解です。
    「みんなのコミック」の評価を目的とするものではありませんので、あらかじめご了承下さい。
    

そんな感じで、これからクリスマスまでよろしくお願いします。

* * *

蛇足になりますが、[みんコミ][2]といえば、僕が普段からお世話になっている根雪れい（[@neyuki_rei][3]）さんも連載陣に名を連ねています。眼鏡っ娘分はありませんが、１０才のおかあさんと主人公の春くんとの仲良しぶりには期待しています！

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

それでは明日、２日の担当は「有山圭二」さんです。

よろしくお願いします。

 [1]: https://www.mincomi.jp/
 [2]: (https://www.mincomi.jp/)
 [3]: https://twitter.com/neyuki_rei