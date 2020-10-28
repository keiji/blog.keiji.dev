---
title: 新しい"Draw 9-patch Tool"を公開しました。
author: Keiji Ariyama
type: post
date: 2013-02-10T08:22:00+00:00
url: /2013/02/newdraw9patchtool.html
blogger_blog:
  - blog.keiji.dev
blogger_author:
  - Keiji Ariyama
blogger_permalink:
  - /2013/02/newdraw9patchtool.html
categories:
  - Android
  - AOSP
  - 習作

---
最近、「有山さんはもう**Makers**の方に行っちゃったんだね。Androidはもうやってないんだよね」「いやいや、もちろんやってますよ。**Android**」と言う会話があったので。

Android SDKに同梱されている&#8221;**Draw 9-patch Tool**&#8220;を、SWTをベースにリライトして、Eclipseのプラグインにしたので公開します。

<div style="text-align: center;">
  <p>
    <span style="text-decoration: underline;">このプラグインを使えば、開発者は、Eclipseの中だけで9-patch画像の作成や編集が出来ます。</span>
  </p>
</div>

<div>
  <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.43.04-PM.jpg"><img class="wp-image-53 aligncenter" alt="Screen Shot 2013-02-10 at 3.43.04 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.43.04-PM.jpg" width="442" height="260" /></a>
</div>

これまでは、Facebook/Twitter/Google+で、知り合い向けに公開していたのですが、もう少し公開範囲を広げることにしました。

 <em id="__mceDel">開発してまだ日が浅いので、Windows/Mac/Linuxの違い、Eclipseのバージョンの違いなどによって、不具合がある可能性があります。<br /> 不具合を見つけた方は、是非ともフィードバックしてください。</em>

フィードバックは、<a href="http://www.facebook.com/keiji.ariyama" target="_blank">Facebook</a>/Twitter/<a href="https://plus.google.com/101075618639637989227?authuser=0" target="_blank">Google+</a>、その他どんな手段でも構いません。宜しくお願いいたします。

## ダウンロード＆利用方法

**draw9patch tool for Eclipse plugin <span>(MD5: 09b952ceea11d9d59984ad5d004b422d)</span>**
  
<https://github.com/keiji/draw9patch2/blob/master/bin/Draw9PatchPlugin.zip>

zipを展開して、pluginsの中の&#8221;Draw9PatchEditor_*.jar&#8221;を、お使いのEclipseのpluginsディレクトリにコピーして下さい（Eclipseの再起動が必要です）。

Eclipseの再起動後、パッケージエクスプローラーから、pngファイル(_e.g. &#8220;res/drawable-hdpi/ic_launcher.png&#8221;_)をダブルクリックすると、&#8221;Draw 9-patch Tool&#8221;が起動します。

<table cellspacing="0" cellpadding="0" align="center">
  <tr>
    <td>
      <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-4.00.32-PM.jpg"><img class="wp-image-60 aligncenter" alt="Screen Shot 2013-02-10 at 4.00.32 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-4.00.32-PM.jpg" width="450" height="273" /></a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <span>この画像中のモザイクは、<br /> Pixelmatorで処理しました。</span>
    </td>
  </tr>
</table>

<!--more-->

## 9-patchとは？

皆さんご存じの通り、Androidは、様々な解像度の端末にアプリを配信することになります。したがって、アプリで表示する画像は、解像度を決めうちで作ることは推奨されていません（参考: **<a href="http://developer.android.com/tools/help/draw9patch.html" target="_blank">Android Developers: Draw 9-patch</a>)**

しかし、小さな画像を拡大して表示するときに、全体を引き延ばすと、画像自体がぼやけてしまいます。

<div>
  <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.53.34-PM.png"><img class="aligncenter  wp-image-61" alt="Screen Shot 2013-02-10 at 3.53.34 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.53.34-PM.png" width="139" height="316" /></a>
</div>

そのため、引き延ばす範囲をあらかじめ設定しておくと、Androidは、指定した範囲を引き延ばして表示します。
  
この、引き延ばす範囲を指定する作業を「パッチを当てる」と、言い、&#8221;Draw 9-patch Tool&#8221;はそのためにあります。

<div>
  <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.47.17-PM.jpg"><img class="aligncenter  wp-image-62" alt="Screen Shot 2013-02-10 at 3.47.17 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.47.17-PM.jpg" width="346" height="203" /></a>
</div>

また、画像の上に文字を表示する場合など、どの部分に文字を載せたいのか（パディング）を設定することも出来ます（右側青色部分）。

パッチとパディングは、画像の一番外側に黒いドットを置いて設定します。
  
パッチをパディングを設定したファイルは、実体はpng画像のデータです。拡張子を&#8221;.9.png&#8221;とすることで、Androidは、その画像が&#8221;9-patched&#8221;であると認識します。

## 現在のDraw 9-patch Tool

現在、Android SDKに同梱のDraw 9-patchツールは、スタンドアローンのアプリケーションとして動作します。

<div>
  <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.48.51-PM.jpg"><img class="aligncenter  wp-image-63" alt="Screen Shot 2013-02-10 at 3.48.51 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.48.51-PM.jpg" width="346" height="203" /></a>
</div>

AWT&Swingベースで開発されており、現在のEclipseプラグイン&#8221;ADT(Android Development Tools)&#8221;には取り込まれていません。そのため、開発者は、Eclipseだけで作業を完結することが出来ませんでした。

また、このツールには、いくつかの不具合が報告されていますが、Android登場から6年間、ほとんど改善されていません。

<table cellspacing="0" cellpadding="0" align="center">
  <tr>
    <td>
      <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.34.01-PM.jpg"><img class="aligncenter  wp-image-64" alt="Screen Shot 2013-02-10 at 3.34.01 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.34.01-PM.jpg" width="346" height="203" /></a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      不具合の1つ。<br /> 画像の読み込み直後に中央に表示されない
    </td>
  </tr>
</table>

僕もいくつか不具合修正のパッチを作成して、AOSP(Android Open Source Project)にサブミットしたことがあります。

しかし、現在の設計は、クラスの分割が十分でなく、またテストケースが用意されていないために大きな変更に踏み切れず。根本的な解決には至りませんでした。

Google I/Oで、開発ツールの担当であるTor Norbyeと、Xavier Ducrohetに会ったときに、&#8221;**Draw 9-patch toolは誰がメンテナンスしているの？**&#8220;と、聞いたことがあります。

その答えは、&#8221;**Nobody knows&#8230;**&#8220;でした。

## 新しいDraw 9-patch Tool

今回、新しい&#8221;Draw 9-patch Tool&#8221;は、SWT(Standard Widget Toolkit)ベースで開発しています。

<div>
  <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.43.54-PM.png"><img class="aligncenter  wp-image-65" alt="Screen Shot 2013-02-10 at 3.43.54 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-3.43.54-PM.png" width="346" height="202" /></a>
</div>

これまで と同様、スタンドアローンで動作するのに加えて、SWTベースなので、Eclipseのプラグインにも比較的容易に移植できました。

<table cellspacing="0" cellpadding="0" align="center">
  <tr>
    <td>
      <a href="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-4.00.32-PM-1.jpg"><img class="aligncenter  wp-image-66" alt="Screen Shot 2013-02-10 at 4.00.32 PM (1)" src="https://blog.keiji.dev/wp-content/uploads/2013/02/Screen-Shot-2013-02-10-at-4.00.32-PM-1.jpg" width="346" height="210" /></a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center;">
      <span>この画像中のモザイクは、<br /> Pixelmatorで処理しました。</span>
    </td>
  </tr>
</table>

プラグイン化することで、開発者は、Eclipseの中だけでパッチやパディングを設定した画像の作成や編集をすることが出来ます。

現在の&#8221;Draw 9-patch Tool&#8221;の不具合は、知りうる限り、全て修正してあります。
  
また、クラス分割の徹底と、JUnitのテストケースを用意することで、コード変更への強度を高めました。

新しいDraw 9-patchのコードは、現在、AOSPへサブミットしてレビュー待ちの状態です。これがもしAOSPに取り込まれれば、正式なAndroid開発ツールとして、開発者の皆さんのお手元に届くかもしれません。

しかし、繰り返しになりますが、開発してまだ日が浅いツールなので、Windows/Mac/Linuxの違い、Eclipseのバージョンの違いなどによって、不具合がある可能性があります。

不具合を見つけた方は、是非ともフィードバックしてください。

フィードバックは、<a href="http://www.facebook.com/keiji.ariyama" target="_blank">Facebook</a>/Twitter/<a href="https://plus.google.com/101075618639637989227?authuser=0" target="_blank">Google+</a>、その他どんな手段でも構いません。宜しくお願いいたします。

<div>
</div>