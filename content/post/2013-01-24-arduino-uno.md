---
title: 雨の日のArduino – UNO
author: Keiji Ariyama
type: post
date: 2013-01-23T15:09:00+00:00
url: /2013/01/arduino-uno.html
blogger_blog:
  - blog.keiji.io
blogger_author:
  - Keiji Ariyama
blogger_permalink:
  - /2013/01/arduino-uno.html
categories:
  - Arduino
  - 習作

---
## はじめに

僕の会社が入居しているビルは、主要道路に面している、いわゆる表店（おもてだな）にあります。
  
また、駅からもそれほど離れておらず、比較的良い立地と言えるでしょう。

僕はここを気に入っていて、かれこれ６年程、ここでお仕事をしているのですが、もちろん、不満がないわけではありません。今回はそんな話です。
  
<!--more-->

## オフィスへの不満

前述の通り、ビル自体は主要道路に面しているのですが、入居している部屋は道路とは反対側です。なので、窓を開けると隣のビルが見えます。

<div>
  <a href="http://blog.c-lis.co.jp/wp-content/uploads/2013/01/Photo-23-01-13-02-40-21-0.jpg"><img class="aligncenter  wp-image-75" alt="Photo-23-01-13-02-40-21-0" src="http://blog.c-lis.co.jp/wp-content/uploads/2013/01/Photo-23-01-13-02-40-21-0.jpg" width="466" height="350" /></a> <a href="http://blog.c-lis.co.jp/wp-content/uploads/2013/01/Photo-23-01-13-02-10-49-6.jpg"><br /> </a>
</div>

曇りガラスなので、窓を閉めると外の景色は見えません。また、大きな窓は、巨大な熱交換機の働きをして冷暖房効率を極端に下げるので、基本、ブラインドは下げたままになっています。

そんな状態なので、**窓を開けない限り、外に雨が降っているか、わかりません。**
  
（ある程度以上の雨であれば、音で解る程度です）

もちろん、外の天気そのものは、オフィスに居るときはあまり関係ありません。問題は外出するときです。

エレベーターに乗って１階まで降りて、正面ドアを開ける直前に、外を歩く人たちが傘を差しているのを見て、雨が降っていることに気づきます。

しょうがなくもう一度エレベーターで上がり、オフィスの鍵を開け、傘をとってもう一度1階に降りる。
  
余分に一往復しているのです。

こんなことを、何度繰り返せば良いのでしょうか。この節電の時代に、**非効率甚だしいと思います。**

## 解決策の検討

この不満点を誰かに相談すると良く「外に出る前に窓から確認するようにすれば良い」と言われます。
  
また、外出前にネットでお天気情報を確認して&#8230;と言う意見ももらいます。

根本的に誤解があるようなのですが、僕は外を確認したり、ネットで天気情報を確認できないと言っているのではありません。

僕にはそもそも、**外出時に天気を確認しようと言う発想そのものが抜け落ちているのです。**

中学くらいの時に「能動的に何かを知ろうとする姿勢が欠けている」と、先生の誰かに言われたような覚えがありますが、あれから２０年経ってもその性質は変わっていないようです。柏木先生、南先生ごめんなさい。

先日、スマートフォンのアプリで、雨が降り出したら教えてくれるものがあると知りました。
  
「<a href="https://play.google.com/store/apps/details?id=jp.or.jwa.furidasu.android" target="_blank">フリダス</a>」と言うそのAndroid用アプリは、日本気象協会が、Google Playで公開しています。

日本気象協会といえば、「<a href="https://play.google.com/store/apps/details?id=jp.or.jwa.sora_annai.android" target="_blank">そら案内</a>」と言うアプリも出していますが、そら案内のAndroid版を開発したのはうちの会社です（そら案内のiPhone版は、株式会社フィードテイラーが開発しています）。

何という偶然でしょう。うちのお客様のリリースしているアプリに、お世話になるだなんて！
  
運命的な出会いを感じながら、早速「フリダス」を、ダウンロードして使ってみました。

&nbsp;

&nbsp;

<div style="text-align: center;">
  <b>発想は素晴らしいのに、アプリの実装がウンコでした。</b>
</div>

<div style="text-align: center;">
</div>

## 雨の日のArduino

フリダスが残念な出来なので(2013年１月現在)、自分で作ってみようと思いました。いや、Androidアプリではなくて「雨が降ったら知らせてくれるような何か」の方をです。

ちょうど、手元に使う予定のない**Arduino UNO**があります。これにセンサーをつなげれば、検出器はすぐに作れそうです。

雨を検出する手段ですが、適当な雨センサーがすぐに見つからなかったので、一先ず**湿度センサー**を使うことにしました。

湿度センサは、スイッチサイエンスで「<a href="http://www.switch-science.com/products/detail.php?product_id=350" target="_blank">HIH-4030</a>」が売っていたので、それを使います。内部抵抗も込みで、一つの部品になっているので、非常に簡単に繋ぐことが出来ます。

<div>
  <a href="http://blog.c-lis.co.jp/wp-content/uploads/2013/01/Photo-23-01-13-02-10-48-0.jpg"><img class="aligncenter  wp-image-77" alt="Photo-23-01-13-02-10-48-0" src="http://blog.c-lis.co.jp/wp-content/uploads/2013/01/Photo-23-01-13-02-10-48-0.jpg" width="389" height="518" /></a>
</div>

本当は、温度センサもつけて、温度もとりたかったのですが、手持ちのサーミスタは、先日の<a href="https://blog.keiji.io/2013/01/adk_21.html" target="_blank">ADKハンズオン</a>で配布して品切れなので、今回は省略です。

<div>
</div>

<div>
  <a href="http://blog.c-lis.co.jp/wp-content/uploads/2013/01/Photo-23-01-13-02-10-49-6.jpg"><img class="aligncenter  wp-image-76" alt="Photo-23-01-13-02-10-49-6" src="http://blog.c-lis.co.jp/wp-content/uploads/2013/01/Photo-23-01-13-02-10-49-6.jpg" width="406" height="305" /></a>
</div>

湿度センサーで検知した湿度をArduinoのアナログピンで受けます。湿度の閾値などの判定は、現在はスケッチ側で処理しています。

<div>
  <pre>#define LED_PIN 11
#define HUM_PIN A0

#define HUMIDITY_TOP_THRESHOLD 700

void setup() {
  Serial.begin(115200);
  pinMode(LED_PIN, OUTPUT);
  pinMode(HUM_PIN, INPUT);
}

void loop() {
  int hum = analogRead(HUM_PIN);
  Serial.println(hum);
  digitalWrite(LED_PIN, hum &gt; HUMIDITY_TOP_THRESHOLD ? HIGH : LOW);
  delay(1000);
}</pre>
</div>

これをパッケージングして、オフィスの入り口の辺りにおいて光らせれば、外の湿度が一定以上であると解るでしょう。閾値を調整すれば、雨の検知にもそれなりの精度が期待出来るかしれません。

最終的には、Arduinoはデータをサーバーに送信するだけにして、サーバー側からAndroid端末に通知が出来るようになればスマートでしょう。その際、サーバーとの通信を何で行うかについては検討の余地がありますが。

直近の一番の課題としては、**湿度センサーをどうやれば安全に窓の外に配置できるのか。**
  
外の天気を知りたいという性質上、窓の外に配置しないと意味がありません。配線を長く延ばすか、無線で飛ばすか。。。

また、時間が出来たら、いろいろ試してみます。