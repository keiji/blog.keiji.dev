---
title: 有山家の買い物メモ
author: Keiji Ariyama
type: post
date: 2014-01-13T17:11:11+00:00
url: /2014/01/happynewyear.html
categories:
  - 習作

---
あけましておめでとうございます。本年もよろしくお願い致します。

僕は年末から正月にかけて、実家に引きこもっていました。

## 冬休みの自由研究

引きこもってる間、何か出来ることはないかと考えていました。

普段はAndroidアプリばかり作ってるし、かと言って執筆とかミスをできない仕事をする気にもなれず、なにか気軽に、新しいことをしようと思い、久しぶりにWebシステムに手を出してみました。

そうして作ったのが**「有山家の買い物メモ」**です。

<div align="center">
  <a href="https://i.keiji.io" target="_blank">https://i.keiji.io/</a>
</div>

<a href="http://blog.keiji.io/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-12.38.00-PM.png" target="_blank"><img src="http://blog.keiji.io/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-12.38.00-PM-300x239.png" alt="Screen Shot 2014-01-13 at 12.38.00 PM" width="300" height="239" class="aligncenter size-medium wp-image-397" srcset="https://blog.keiji.io/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-12.38.00-PM-300x239.png 300w, https://blog.keiji.io/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-12.38.00-PM-1024x817.png 1024w, https://blog.keiji.io/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-12.38.00-PM-624x498.png 624w" sizes="(max-width: 300px) 100vw, 300px" /></a>

## 機能

このシステムは、事前に登録したAmazonの商品を定期的に確認。価格が変動して、かつ所定の金額以下（または以上）になれば、メールで知らせます。

<a href="http://blog.keiji.io/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-12.38.48-PM.png" target="_blank"><img src="http://blog.keiji.io/wp-content/uploads/2014/01/Screen-Shot-2014-01-13-at-12.38.48-PM-300x239.png" alt="Screen Shot 2014-01-13 at 12.38.48 PM" width="300" height="239" class="aligncenter size-medium wp-image-395" /></a>

その名の通り、我が家の家族（主に母）に向けて作ったシステムなので、他の人が使うことを想定していませんが、せっかくなので公開します。

もちろん、何か不都合があればすぐに公開を止めます。あっさりです。

<!--more-->

## 開発動機

僕の実家では結構な数の犬猫と暮らしていて、彼らのためにトイレ用品やご飯などを定期的に購入する必要があります。分かる人はわかると思いますが、猫砂や犬のご飯は、かさばる上に結構な重さになります。

僕としてはAmazonで購入すれば送料無料だったり、労力や時間の節約になると思うのですが、母は、近所のスーパーの特売がAmazonの価格より安いと言って買い出しに行っています。

このシステムを使って、Amazonの価格が近所のスーパーの特売価格より安い時にメールで知らせるように設定すれば、安いタイミングを逃すことはなくなると思います。

システムの利用にはAmazonアカウントを使います。なぜAmazonアカウントなのかというと、母はAmazonアカウントは持っているからです（新しいIDとパスワードを覚えてもらうのは大変です）。

## 開発の構成

今回のシステム開発にあたって言語はPythonを使い、<a href="https://www.djangoproject.com/" target="_blank">Django</a>フレームワークで開発しました。

Pythonを本格的に使うのは初めてだったのですが「<a href="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&#038;bc1=000000&#038;IS2=1&#038;bg1=FFFFFF&#038;fc1=000000&#038;lc1=0000FF&#038;t=keijiio-22&#038;o=9&#038;p=8&#038;l=as4&#038;m=amazon&#038;f=ifr&#038;ref=ss_til&#038;asins=4797371595" target="_blank">みんなのPython</a>」を読みながら勉強しました。インデントの世界もなかなかいいものです。

Djangoは非常に良く出来たフレームワークだと思います。ただ、日本語での情報が少ない上に、バージョンアップで少しずつ仕様が変わる。検索すると古い情報が引っかかる。というAndroid現象が起きているのが残念です。

Amazonとの連携は<a href="https://github.com/omab/python-social-auth" target="_blank"><strong>python-social-auth</strong></a>を使いました。対応プロバイダーが多いのが素敵ですね。

今回のシステムも、GoogleアカウントやTwitter, Facebookアカウントでもログインができるようにできたのですが、まずは母の持っているAmazonアカウントに限定しました。

そしてAmazonアカウントで認証する際のRedirect URLが、localhostのときはHTTPでいいのですが、本番運用のアドレスを指定するとHTTPSのアドレスしか受け付けてくれず、泣く泣くSSL証明書を取得したりしました（本当は、自己署名の証明書でも問題なく認証できるのですが、アクセスするときにブラウザが警告を出すのは、さすがにみっともないので）。

Amazon APIから取得するXMLは<a href="http://www.crummy.com/software/BeautifulSoup/" target="_blank"><strong>Beautiful Soup 4</strong></a>を使ってパースしました。このライブラリは直感的に使えてとても便利です。

ただ、SQLiteでテストしていたときは良かったのですが、MySQLにつなぎ替えたら突然**UnicodeDecodeError**が発生したりしました。

マルチバイト文字の時に正しくUnicode型として返さないとかで、結局、Beautiful Soup 4からマルチバイト文字を取得する部分でunicode()を使うことで解決したのですが、この方法で良かったのかは未だに不明です。

デザインは<a href="http://getbootstrap.com/" target="_blank"><strong>Bootstrap</strong></a>を使ってあっさり仕上げました。

サーバーは、商品などのデータをJSONで返すようにしてあります。このJSONをHTMLに整形するテンプレートエンジンとして<a href="http://handlebarsjs.com/" target="_blank"><strong>Handlebars</strong></a>を使いました。

ページの挙動はBootstrapと一緒にインポートした<a href="http://jquery.com/" target="_blank"><strong>jQuery</strong></a>を使い、スクリプトはすべて<a href="http://coffeescript.org/" target="_blank"><strong>Coffeescript</strong></a>でコーディングしました。

## おわりに

久しぶりにWebで、のびのびシステムを開発しました。

PerlでCGIを作っていた頃、と、比べるのは少しやり過ぎかもしれませんが、数年前、Cake PHPでシステム開発をした頃と比べても、作りたいものがある程度形になるまでの時間は格段に短くなっていますね。

もちろん、ある程度形になったものを「きちんとする」には、やはりそれなりの時間がかかるものですが、、、今回の場合、メインターゲットが母なので、母からの要望を受けて改良をしていくのが一番かなと思っています。

人間万事塞翁が馬。本年もよろしくお願い致します。