---
title: Moto 360 のディスプレイ焼き付きを防ぐアプリを作った話
author: Keiji Ariyama
type: post
date: 2014-09-24T13:43:47+00:00
url: /2014/09/prevent_lcd_ghosting_for_moto360.html
categories:
  - Android
  - メモ
  - 雑記

---
Google I/Oのお土産として配布されたMotorolaのAndroid Wear端末「Moto 360」が、先日ようやく届いたんですが。

<a href="https://blog.keiji.io/wp-content/uploads/2014/09/R0002605.jpg" target="_blank"><img src="https://blog.keiji.io/wp-content/uploads/2014/09/R0002605-300x198.jpg" alt="Moto 360" width="300" height="198" class="aligncenter size-medium wp-image-441" srcset="https://blog.keiji.io/wp-content/uploads/2014/09/R0002605-300x198.jpg 300w, https://blog.keiji.io/wp-content/uploads/2014/09/R0002605-1024x678.jpg 1024w, https://blog.keiji.io/wp-content/uploads/2014/09/R0002605-624x413.jpg 624w" sizes="(max-width: 300px) 100vw, 300px" /></a>

このMoto 360、充電中に必ず時計表示になるんだけど、そのせいで液晶が焼き付くという不具合が起きているらしいです。

一部のMoto 360で充電中画像のディスプレイへの焼き付きが発生している模様
  
<a href="http://juggly.cn/archives/128552.html" target="_blank">http://juggly.cn/archives/128552.html</a>

「充電中の画面が焼き付くなら、電源を落として充電すれば良いじゃない？」と、思ったのですが、電源OFFの状態でも充電台に置くと自動的に起動して、問題になっている充電中の表示をするという鬼のような仕様です。

しょうが無いので、充電中に強制的に表示をOFF（黒画面）にするというアプリを作りました。

<!--more-->

[<img src="https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-300x198.jpg" alt="インストール&サービス起動後" width="300" height="198" class="aligncenter size-medium wp-image-442" srcset="https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-300x198.jpg 300w, https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-1024x678.jpg 1024w, https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-624x413.jpg 624w" sizes="(max-width: 300px) 100vw, 300px" />][1]

<a href="https://blog.keiji.io/wp-content/uploads/2014/09/GhostBustersForMoto360-20141002.apk_.zip" target="_blank">GhostBustersForMoto360-20141002.apk.zip</a>

あ、apkはzipで圧縮されています（なに言ってるかわからねーと思うが、あとでWordPressの設定を、.apkを取り扱えるように変更するので、今は我慢してください）

アプリが起動した状態では、充電台に置くと画面がすぐに暗くなり、液晶の焼き付きを防止できます。

<a href="https://blog.keiji.io/wp-content/uploads/2014/09/R0002606.jpg" target="_blank"><img src="https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-300x198.jpg" alt="インストール&#038;サービス起動後" width="300" height="198" class="aligncenter size-medium wp-image-442" srcset="https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-300x198.jpg 300w, https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-1024x678.jpg 1024w, https://blog.keiji.io/wp-content/uploads/2014/09/R0002606-624x413.jpg 624w" sizes="(max-width: 300px) 100vw, 300px" /></a>

難点として、アプリは、コンパニオンとして開発していないので、adb経由でインストールしてなければなりません。Moto 360にはドングルも無く、USBケーブルを接続できないので、Bluetoothデバッグを有効にしたり、電波暗室に移動したりとインストール１つで大騒ぎとなります。

また、一度だけ、amコマンドを使って、Serviceを起動する必要があります(RECEIVE\_BOOT\_COMPLETEDを有効にするため)。

大変めんどくさいのですが、きっと焼き付きの不具合（充電中画面の仕様）は、次のアップデート当たりで解消すると思うので、今、動けばいいやくらいのノリで作っています。

詳しいインストールの方法は、<a href="https://github.com/keiji/GhostBustersForMoto360/blob/master/README.md" target="_blank">こちら</a>を参照してください。

ソースコードはこちら。

<a href="https://github.com/keiji/GhostBustersForMoto360/" target="_blank">keiji/GhostBustersForMoto360</a>

 [1]: https://blog.keiji.io/wp-content/uploads/2014/09/R0002606.jpg