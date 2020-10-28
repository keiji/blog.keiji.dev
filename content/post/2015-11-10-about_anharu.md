---
title: Webサイトを作った話
author: Keiji Ariyama
type: post
date: 2015-11-10T06:49:06+00:00
url: /2015/11/about_anharu.html
categories:
  - Android

---
新しいWebサイトを作りました。

<div align="center">
  <a href="https://anharu.keiji.io">はじめてのAndroidアプリ開発レッスン<br /> <img src="https://blog.keiji.io/wp-content/uploads/2015/11/Android_Studio_icon.png" alt="Android_Studio_icon" width="25%" height="25%" class="aligncenter size-full wp-image-595" /> </a>
</div>

以前、技術評論社より[Androidアプリ開発の初心者本][1]を出版したのですが、その本のAmazonレビューに「Javaのことをまったく解説していないのでこれだけではわかりません。初心者向きではない」というような指摘が多かったことから「やりましょう」とばかりに取り組んだこの企画。 実は、９月の[名古屋つ部][2]ではちらりと発表していたのですが、ようやくのお目見えとなりました。

公開が遅くなった理由？

Android Studioのアップデートに聞いて下さい！

今回もまたサイト掲載のコミックとイラストは[根雪れい][3]さんにお願いしました。 [みんコミ][4]の新連載準備でお忙しい中、最高の眼鏡っ娘を生み出していただき、本当にありがとうございます！

<blockquote class="twitter-tweet" lang="en">
  <p lang="ja" dir="ltr">
    >RT 有山さんの「はじめてのAndroidアプリ開発レッスン」に絵とマンガを描きましたよ（描くにあたってメガネ外さないでとしか要求されなかった…
  </p>
  
  <p>
    — 根雪れい.ティア不参加です… (@neyuki_rei)
  </p>
  
  <p>
    <a href="https://twitter.com/neyuki_rei/status/662623560578412544">November 6, 2015</a>
  </p>
</blockquote>

<!--more-->

* * *

## 使っている技術

僕はAndroidアプリ開発がメインの仕事なので、Webについてはリアルタイムではフォローしてないのですが、大体1年に一回くらいはサイトとかサービスを作っていたりします。

以前は、Amazonの価格の変化をチェックするサービス[有山家の買い物メモ][5]とか作りました。

たまにWebの世界に触れると、技術やトレンドが変化していて面白いんですね。 数ヶ月ぶりに見る、みのもんたの顔が全然違って見えるのと同じ現象です。

なので、今回のサイト構築にあたっても、なるべく面白そうな技術やプラットフォームを採用するようにしました。

### Hugo

[<img src="https://blog.keiji.io/wp-content/uploads/2015/11/hugo.png" alt="hugo" width="25%" height="25%" class="aligncenter size-full wp-image-590" />][6]

サイトの構築にはHugoを採用しました。 HugoはSteve Franci氏がオープンソースで開発している&#8221;Static Website Engine&#8221;です。Markdownとテーマ・テンプレートから静的なHTMLを出力します。

同じSWEとしてはJekyllや<s>Octpass</s>Octopressが有名ですが、HugoはGolangで開発されているので変換のスピードが高速という利点があります。

今回はコンテンツサイトということもあり、さくっとWordPressで作るかなとも思いました。しかし、最近アップデートのためだけにコントロールパネル開いてる気がするし、記事に少しでもアクセスが集中するとサーバーが死んじゃうくらい重くなるし、もうWordPressはこりごりだ！

ということでHugoにしました。 （正確には、CMSを使わない決定をしたというのが正しいでしょう）

それだとJekyllでいいんじゃない？　という話になるのですが、Jekyllはgemの依存パッケージがガンガンはいりそうなので、コンパクトにまとまっているHugoを選びました。あと、使ったことがある人が少なそうだったので、楽しそうだなと思ったのも理由の一つですね。

結果的には大変苦労をしました。

その辺りのアレコレについては、また機会があれば詳しくお話しできればと思います。

### Material Design Lite

* * *

「世の中は[マテリアルデザイン][7]ですって。奥さん」

「そうは言っても一からあんなデザインを作ろうなんて、正気の沙汰じゃないよな。AndroidだとDesign Support Libraryがあるけど、Webではなにかないの？」

「Polymer?」

「Polymer、アイツはダメだ！ デザインを適用したいだけにしては重量級が過ぎる！」

「そこでMaterial Design Liteですよ！」

* * *

ということで[Material Design Lite][8]です。

MDL(Material Design Lite)は、Googleが開発しているデザインライブリ（こう言う言い方は大丈夫なんでしょうか）です。

MDLを使えばCSSのクラス指定をするだけでマテリアルデザインっぽい見た目と動きのサイトが結構、簡単に作れます。

サンプルも充実していて、 [はじめてのAndroidアプリ開発レッスン][9]のデザインは、ほとんどがサンプルを組み合わせて出来ています。

MDLはとても便利です。しかしMDLのクラスの命名規約として採用されている[BEM][10]は、人類が習得するにはまだ精神レベルが足りてないと思いました。

### TypeSquare

TypeSquareはモリサワが提供しているWebフォント配信サービスです。 最近だとAdobeのTypekitにモリサワフォントが追加されたと話題になりましたがTypeSquareなら元から使い放題です。

ただ、たくさんのフォントが使えるのと、実際にたくさんのフォントを違和感なく組み合わせるのは別の話で、この辺りはまだ試行錯誤中です。

モリサワさんにおかれましては「フォント活用術」みたいな講習をやってもらえれば是非とも受けたいと思っているのでご検討の程よろしくお願いいたします。

### Let&#8217;s Encrypt

SSL証明書は[Let&#8217;s Encrypt][11]のクローズドベータプログラムで取得しました。

ＰＣにインストールしたクライアントソフトを使って生成したトークンを、サーバー側でアクセス可能な状態にしてサーバー所有を認証します。

SSL証明書を取得できたのでnginxのSPDYを有効にしてあります。

### その他

その他、オープンソースソフトウェアについては&#8221;Open Source License&#8221;をご覧ください。

## おわりに

せっかくサイト作ったんだから、３回は続けたいなぁと思っています。

あと、[書籍][1]の方は絶賛改訂作業中です。

改訂版は来年の１月上旬頃発売予定なので、そちらの方もよろしくお願いいたします。

 [1]: http://gihyo.jp/book/2014/978-4-7741-6998-9
 [2]: http://www.zusaar.com/event/15227003
 [3]: https://twitter.com/neyuki_rei
 [4]: http://www.mincomi.jp/
 [5]: https://i.keiji.io/
 [6]: https://blog.keiji.io/wp-content/uploads/2015/11/hugo.png
 [7]: http://www.google.com/design/spec/material-design/introduction.html
 [8]: http://www.getmdl.io/
 [9]: https://anharu.keiji.io
 [10]: https://en.bem.info/
 [11]: https://letsencrypt.org/