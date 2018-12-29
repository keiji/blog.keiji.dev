---
title: 技術書典2でTensorFlowの本を頒布します
author: Keiji Ariyama
type: post
date: 2017-04-01T15:00:28+00:00
url: /2017/04/techbookfest2.html
categories:
  - TensorFlow
  - イベント

---
4月9日、秋葉原UDX アキバ・スクエアにて開催される「<a href="https://techbookfest.org/event/tbf02/circle/5705718560718848" target="_blank">技術書典2</a>」に、サークル「めがねをかけるんだ」として参加させていただくことになりました。

「めがねをかけるんだ」では、TensorFlowで超解像を題材にした冊子を頒布いたします。

<img class="aligncenter size-full wp-image-1557" src="https://blog.keiji.io/wp-content/uploads/2017/04/techbookfest2.png" alt="techbookfest2" width="50%" height="50%" />

| タイトル | TensorFlowはじめました Super Resolution ー 超解像 |
|:---- |:--------------------------------------- |
| ページ数 | 68p（本文墨刷）                               |
| 頒布価格 | 1,000円                                  |
| 発行   | 個人サークル「めがねをかけるんだ」                       |

<!--more-->

* * *

## どんな本なの？

TensorFlowがオープンソースで公開されるまで「機械学習」に触れたことがなかった筆者が、TensorFlowを通じて機械学習に挑戦して、七転八倒した成果をまとめたものです。

本書では、まず始めに TensorFlow の基本であるデータフローグラフについて解説します。
  
次に、画像の解像度を高める「超解像」を題材に「畳み込みニューラルネットワーク」 モデルの学習と評価を実装します。

## 超解像って何？

超解像(Super Resolution)とは、拡大や不可逆圧縮などで劣化した画像の解像度を高めること。それを実現する技術を言います。
  
ディープラーニングやTensorFlowに限った技術ではないのですが、今回はTensorFlow を使って、超解像を実 現する機械学習のモデルを作成しました。

## 目次

  * TensorFlowの基礎 
      * TensorFlowとは
      * データフローグラフ
      * Tensor（テンソル）
      * 変数とプレースホルダー
      * 演算子のオーバーロード
      * ブロードキャスティング
  * CNNで超解像 
      * 超解像とは
      * モデルの定義
      * 学習
      * 画像処理
      * 評価
  * 超解像奮闘記 
      * 畳み込み層とパディング
      * 画像の読み込み処理
      * 活性化関数
  * さまざまなモデル 
      * モデル（9-5-5）
      * 画質の指標（PSNR, SSIM）
      * Batch Normalizationの導入

## どんな表紙なの？

<img class="aligncenter size-full wp-image-1557" src="https://blog.keiji.io/wp-content/uploads/2017/04/techbookfest2.png" alt="techbookfest2" width="50%" height="50%" />

表紙・裏表紙のイラストは、根雪れい（<a href="https://twitter.com/neyuki_rei/" target="_blank">@neyuki_rei</a>）さんによるものです。

いつも素晴らしいイラストやまんが、本当にありがとうございます！

また、根雪さんの<a href="https://www.mincomi.jp/title/?title=328009" target="_blank">「おかあさん(10)と僕。」</a>の単行本が出ます。

現在、Amazonで予約受付中ですので、こちらも是非お買い求めください！

<iframe style="width: 120px; height: 240px;" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=keijiariyama-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4800270553&linkId=73cca0e5567140f6af19f0a3857ed504" width="300" height="150" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## どこで手に入るの？

4月9日（日）に秋葉原UDX、アキバ・スクエアで開催される「技術書典２」に、有山圭二の個人サークル「めがねをかけるんだ」として参加します。配置は『おー１２』です。

<img class="aligncenter size-full wp-image-1558" src="https://blog.keiji.io/wp-content/uploads/2017/04/7e532234bec803f783490593300730f3.png" alt="技術書典2" width="50%" height="50%" />

有償での頒布となります。
  
また、部数が限られていますので、売り切れの際はご容赦ください。

## 追伸

４月６日、技術書典２に先駆けて、リクルート住まいカンパニー様のオフィスをお借りして「機械学習の勉強会（仮）」をやります。

ご興味がある方は、こちらもよろしくお願いします。

[機械学習の勉強会（仮）][1]

※ 冊子の販売はありません

 [1]: https://connpass.com/event/53183/