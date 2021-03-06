---
title: Chromebook（Lenovo IdeaPad Duet）でAndroidアプリ開発
author: Keiji Ariyama
type: post
date: 2020-12-15T10:00:00+09:00
url: /2020/12/android_app_development_on_chromebook.html
description: 「Lenovo IdeaPad Duet」でAndroidアプリ開発するときは、`adb connect`するのが一番早いという話。
images:
- /images/androiddev_on_chromebook/my_new_gear.jpg
categories:
  - 習作
  - Android
  - Chromebook

---

　アプリ開発の仕事で比較的画面が大きい端末に対応する必要があり、いい感じのAndroidタブレットが見当たらなかったので、最近はAndroidアプリも普通に動く「Chromebook」を購入しました。

![My new gear...](/images/androiddev_on_chromebook/my_new_gear.jpg)

<!--more-->

　僕が購入したのは、「Lenovo IdeaPad Duet Chromebook」。Chromebookは他のメーカーからも発売されていますが、Twitterで[@eaglesakura](https://twitter.com/eaglesakura)が完全に理解していたので、同じ機種にしておけば、いざとなれば聞けるなという算段です。

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">Chromebook完全に理解した <a href="https://t.co/prgBy6BcHc">pic.twitter.com/prgBy6BcHc</a></p>&mdash; 川峠@Andriders (@eaglesakura) <a href="https://twitter.com/eaglesakura/status/1300752310725869570?ref_src=twsrc%5Etfw">September 1, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">Chromebook完全に理解した</p>&mdash; 川峠@Andriders (@eaglesakura) <a href="https://twitter.com/eaglesakura/status/1300772791944228866?ref_src=twsrc%5Etfw">September 1, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## 外部のPCからChromebookにadb接続する

　本稿で説明する筆者の作業環境は次の通りです。

 * Google Chrome OS バージョン87.0.4280.109(Official Build)　（32ビット）

　ChromebookでAndroidアプリを開発しようと思った時、最初に目を通すのが次のサイトでしょう。
Linux（ベータ版）の有効化、Androidアプリの開発（ADBデバッグを有効にする）、Chrome OS上 **で** 起動したAndroid Studioからアプリをインストールする方法などが紹介されています。

 * [Chrome OSデバイス - 開発環境の準備](https://developer.android.com/topic/arc/development-environment?hl=ja)

　筆者の実務的にはChrome OS上でAndroidアプリ開発が完結するということはないので、外部のPCからテスト端末（Chromebook）にapkをインストールする方法として「[Chrome OSデバイス - 別のデバイスからデプロイする](https://developer.android.com/topic/arc/development-environment?hl=ja#deploy_from_another_device)」を参考に設定しました。

　結論から言うと **「USB経由での接続（デバッグ）」は諦めて「ネットワーク経由での接続」で運用しましょう。**

　また、ネットワーク経由での接続についても「Androidアプリの開発」を有効にしたら、外部PCからChromebookのIPアドレスに対して、いきなり`adb connect`して問題ありません（外部のPCを信頼するか尋ねるダイアログが表示される）。

```
$ adb connect [ChromebookのIPアドレス]
connected to [ChromebookのIPアドレス]:5555
```

　`shell` `crossystem` `dev_features_ssh`と、ドキュメントにはいろいろ手順が書いてありますが、手順通りに作業しようとしても、筆者の環境には`crosssystem`がなく、そもそも`shell`の時点でシステムがなかったりします。ドキュメントが古いのかと思って英語版を見てみましたが、内容に大きな違いはなさそうです。

　adbを通じて接続するとAndroid Studioは「Lenovo IdeaPad Duet Chromebook」を「Google kukui」として認識します。

![google_kukui](/images/androiddev_on_chromebook/google_kukui.png)

----

　実際に使ってみて、使い勝手はなかなか良いと思いました。液晶もきれいでタッチパネルの精度も良好です。少し重いのが玉に瑕ですが、Androidタブレット不遇の時代にChromebookという選択肢は十分に有りだと思います。付属のキーボードが日本語というのが不満なので、時間ができたら英語キーボードが取り寄せられるかやってみます。

<iframe style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=keijiariyama-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B08CCWGJDY&linkId=25d7ebff0a600f37ce654c7a6d2eff0f"></iframe>

### 追記
　その後、いろいろ調べてみたところ、Lenovo Japanはおろか、Lenovo USでも交換用のキーボードを部品として持っていないらしいことがわかりました。

 * Lenovo Duet -- be careful with the keyboard, it's hard to replace<br>https://www.reddit.com/r/chromeos/comments/htlel4/lenovo_duet_be_careful_with_the_keyboard_its_hard/

　どうしようかな……

### 追記2（2020/12/18）

<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">Lenovo, 時間ができたからサポートセンターに問い合わせたんだけど、サポート体験はあまり良くはなかった。<br>あと、IdeaPad Duetのキーボードは本当に部品として販売していない。壊れた時はサポート（スマートセンター）に連絡して。こっちではわからないってことだった。<a href="https://t.co/f4gQMQcqXy">https://t.co/f4gQMQcqXy</a></p>&mdash; ARIYAMA Keiji (@keiji_ariyama) <a href="https://twitter.com/keiji_ariyama/status/1339812702890254338?ref_src=twsrc%5Etfw">December 18, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
