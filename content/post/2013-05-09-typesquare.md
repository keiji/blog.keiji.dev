---
title: TypeSquareのクラウドフォント
author: Keiji Ariyama
type: post
date: 2013-05-09T06:26:13+00:00
url: /2013/05/typesquare.html
categories:
  - メモ

---
モリサワ社が提供している[クラウドフォント][1]を導入しました。今、皆さんが見ているこの文字は、クラウドフォントが出力した字体です。

クラウドフォントを導入するきっかけは[会社のサイト][2]。何年か前に作成して以来、ほとんど放置していたのだけど、Google I/Oの前に思い切って更新することにしました。

新しいサイトのデザインは、標準テンプレートのデザインほぼそのままで、内容も最小限です。

あまりにも貧相なので、何か出来ないものかと考えていたところに、TypeSquareが[クラウドフォント0円キャンペーン][3]をしていたので、渡りに船とばかりに申し込んだというわけです。

このクラウドフォント、今回初めて導入してみたのですが、想像以上に簡単だったので、利用手順を紹介します。面白そうと思ったら是非使ってみて下さい。<!--more-->

## クラウドフォントの利用方法

クラウドフォントは、次の4つのステップで利用します。

  1. ユーザー登録
  2. 利用するウェブサイトのURLを登録
  3. Scriptタグの埋め込み
  4. CSSでフォントを指定

### ユーザー登録

クラウドフォントを利用するために、まずはユーザー登録をします。

登録は、個人または法人を選択できます。登録内容を送信すると、メールアドレス宛に確認のメールが届き、そこに記載されているアドレスにアクセスすると登録完了です。

以後、入力したメールアドレスとパスワードで、TypeSquareにログインします。

### 利用するウェブサイトのURLを登録

フォントを利用するウェブサイトのURLを登録します。

ウェブサイト毎にページビューが計測されていて、現在は全て無料ですが、正式入会後(正式入会の受付は、2013年12月開始予定)は、一定のページビューを超えるとお金がかかったりするようです。

### Scriptタグの埋め込み

「導入方法」のページを開いて、表示されるScriptタグを、登録したウェブサイトのheadに埋め込みます。参照するスクリプトは、ユーザー毎に異なります。

### CSSでフォントを指定

ページのCSSに、適用したいフォント名を指定します。

    .wrapper {
        font-family: UDTypos58;
    }
    

これだけで、指定した部分にクラウドフォント（この場合は&#8217;UDタイポス&#8217;）を適用できます。

### 使ってみた感想

これまでのウェブデザインでは、表示するフォントは受け手側に依存していました。

どうしてもフォントを固定したい場合は、画像にしたりといろいろと手間がかかった訳ですが、[クラウドフォント][1]を使うと、そのような心配は無くなります。

気になる本サービス開始後の価格も、120万(1年間)PVまでは¥16,800。また、月に1000PVを超えなければ無料と、モリサワ社の販売しているフォントセットと比べると、驚くほど低廉な価格だと思います。

一方、現時点ではTypeSquareのサイトが重いのが気になります。ページの切り替えの度に大体１秒程度、特に、フォントの検索と一覧表示で、検索ボタンを押してから数秒待たされることもあります。

とは言えば、クラウドフォントの表示が遅いと言うことは無いので、この技術がビジネスになり、サーバーリソースの割り振りが増えれば、簡単に改善できるでしょう。

初めて使ってみた[クラウドフォント][1]。これまでの不便を一気に解決すると言う意味で、とても大きな可能性を感じました。

 [1]: https://typesquare.com
 [2]: http://www.c-lis.co.jp
 [3]: http://typesquare.com/news/view/VEOS0kf0fcE=