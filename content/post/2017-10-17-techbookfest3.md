---
title: 技術書典3でTensorFlowの本を頒布します。
author: Keiji Ariyama
type: post
date: 2017-10-17T11:18:06+00:00
url: /2017/10/techbookfest3.html
categories:
  - TensorFlow
  - イベント

---
10月22日、秋葉原UDX アキバ・スクエアにて開催される「<a href="https://techbookfest.org/event/tbf03/circle/5686003050217472" target="_blank">技術書典３</a>」に、サークル「めがねをかけるんだ」として参加させていただくことになりました。

「めがねをかけるんだ」では、TensorFlowで物体検出を題材にした冊子を頒布いたします。

<img class="aligncenter size-full" src="https://blog.keiji.dev/wp-content/uploads/2017/10/tfbook3-hyo1.png" alt="tfbook3" width="50%" height="50%" />

| タイトル | TensorFlowはじめました Object Detection ー 物体検出 |
|:---- |:---------------------------------------- |
| ページ数 | 68p（本文墨刷）                                |
| 頒布価格 | 1,000円                                   |
| 発行   | 個人サークル「めがねをかけるんだ」                        |

<!--more-->

* * *

## どんな本なの？

TensorFlowがオープンソースで公開されるまで「機械学習」に触れたことがなかった筆者が、TensorFlowを通じて機械学習に挑戦して、七転八倒した成果をまとめたものです。

本書では、まず始めにTensorFlowの基本であるデータフローグラフについて解説します。
  
次に、画像から物体を見つける「物体検出」を題材に「畳み込みニューラルネットワーク」モデルの学習と評価（検証）を実装します。

## 物体検出って何？

物体検出（Object Detection）は、画像の中に含まれる「物体」を検出するタスクです。

以前は、画像から物体らしいものが写っている候補領域（Region Proposal）を見つけて、それらについて分類（Classification）を実行するのが一般的でした。

現在はYOLO（You Only Look Once）やSSD（Single Shot MultiBox Detector）といった手法が発表されていて、検出と分類を1つのネットワークで行うことができます。

## 目次

  * TensorFlowの基礎 
      * TensorFlowとは
      * データフローグラフ
      * Tensor（テンソル）
      * 変数とプレースホルダー
      * 演算子のオーバーロード
      * ブロードキャスティング
  * 　グリッドベースの物体検出 
      * 物体検出とは
      * モデルの定義
      * データセットの作成
      * 学習（訓練）
      * 検証
  * 物体認識奮闘記 
      * 確信度と座標
      * 畳み込み層
      * モデルの定義
      * データセットの再作成
      * 学習・訓練
      * 検証

詳細な目次と本文サンプルは<a href="https://techbookfest.org/event/tbf03/circle/5686003050217472" target="_blank">こちら</a>

## どんな表紙なの？

<img class="aligncenter size-full wp-image-1557" src="https://blog.keiji.dev/wp-content/uploads/2017/10/tfbook3-hyo1.png" alt="tfbook3" width="50%" height="50%" />

表紙・裏表紙のイラストは、根雪れい（<a href="https://twitter.com/neyuki_rei/" target="_blank">@neyuki_rei</a>）さんによるものです。

ぼくのふわふわした要望をまとめて、素晴らしい絵に仕上げてくれました。
  
本当にありがとうございます！

また、根雪さんの<a href="https://www.mincomi.jp/title/?title=328009" target="_blank">「おかあさん(10)と僕。」</a>の単行本が好評発売中です。
  
こちらも是非お買い求めください！

<iframe style="width: 120px; height: 240px;" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=keijiariyama-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=4800270553&linkId=73cca0e5567140f6af19f0a3857ed504" width="300" height="150" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## どこで手に入るの？

10月22日（日）に秋葉原UDX、アキバ・スクエアで開催される「技術書典３」に、有山圭二の個人サークル「めがねをかけるんだ」として参加します。配置は『か２２』です。

<a href="https://techbookfest.org/event/tbf03/circle/5686003050217472" target="_blank"><br /> <img class="aligncenter size-full" src="https://blog.keiji.dev/wp-content/uploads/2017/10/circlecut.png" alt="技術書典3" width="50%" height="50%" /></a>

有償での頒布となります。
  
また、部数が限られていますので、売り切れの際はご容赦ください。