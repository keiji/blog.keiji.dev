---
title: 技術書典13で「コロナ禍のめがねをかけるんだ」を頒布します
author: Keiji Ariyama
type: post
date: 2022-09-10T10:00:10+09:00
url: /2022/09/techbookfest13.html
categories:
  - イベント

---
　9月11日（日）、池袋サンシャインシティにて開催される「[技術書典13](https://techbookfest.org/event/tbf13)」に、サークル「[めがねをかけるんだ](https://techbookfest.org/organization/5173176170446848)」として参加させていただくことになりました。

| タイトル | コロナ禍のめがねをかけるんだ |
|:---- |:----------------- |
| 判型   | B5                |
| ページ数 | 16p（本文墨刷）         |
| 頒布価格 | 300円（会場価格）            |
| 発行   | 個人サークル「めがねをかけるんだ」 |

## 目次

 * はじめに
 * Gitのコミットに署名を付けるようになった話
 * 30億のデバイスで動くJava
 * スマートカードを手に入れる
 * Java Card Appletを開発しよう
 * 遊び道具としては難しい
 * OpenPGP Card Applet
 * おわりに

　本文サンプルは[こちら](https://techbookfest.org/product/viaWpAsxCX0pyyKZM5CEBK?productVariantID=ihrw7XZAHgw7qpEFPcTFDp)

<!--more-->
----

# スマートカード・NFCリーダーの紹介

> 本書の取り組みの中で、いくつか手に入れたスマートカードリーダー・NFCリーダーを紹介します（リーダーという名前ですが、カードへの書き込みもできます）。

　と、言う書き出しで本に収録しようと思っていたのですが、これをページ数が中途半端になるので掲載しないことにしました。
もったいないのでここに載せます。

## 接触式スマートカードリーダー

　チップ表面にある端子に接続して通信する方式です。

　直接接続するので通信は安定しますが、抜き差しすると端子がカードの表面を擦って跡がつきます。見栄えが良くないのもありますが、端子がどの程度の挿抜に耐えられるのかが気になります。

### Alcorlink USB Smart Card Reader

　1300円くらいで買える最低価格帯のICカードリーダーです。
商品ページのブランドには「INMAN」と表記されていますが、Windowsからは「Alcorlink USB Smart Card Reader」として認識します。

<iframe sandbox="allow-popups allow-scripts allow-modals allow-forms allow-same-origin" style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=keijiariyama-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B08BCLPR29&linkId=7ea1a44aeb1b84120fda89d2bedf6f0b"></iframe>

　元々はマイナンバーカードでの確定申告用に購入したものです。本体に付いているICチップのマークはカードを挿すと赤く光ります。
安いけれどopenSCもgpg（scdaemon）からも問題なく利用できます。

　難点としては、表面がつやつやのピアノブラックなので傷が目立ちやすいこと。
あと個体差もあるとは思いますが、このリーダーにカードを入れると端子に傷が付きやすいように思います。


### Gemalto ICカードリーダ・ライタ IDBridge CT30

　4000円台で購入したICカードリーダーです。
ブランドのGemalto（ジェムアルト）は、現在はThales（タレス）社になっているそうです。

<iframe sandbox="allow-popups allow-scripts allow-modals allow-forms allow-same-origin" style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=keijiariyama-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B003XF2JJY&linkId=f56bd4e25438bd35555dc6913673aa0b"></iframe>

こちらもopenSCもgpg（scdaemon）から問題なく利用できています。

性能的にはAlcorlinkのものとほぼ変わらないように思いますが、こちらは裏面が平らになっているので、カードにアプレットを書き込んだ後、カードを抜き取らずそのままスマートフォンで読み取ってアプレットの挙動をテストするような使い方ができます。


## 非接触式スマートカードリーダー

　カード内に備えられたアンテナを通じて通信する方式です。

　カード内にバッテリーはなく、読み取り側の出している電波をカード内のコイルで受けることで発生する誘導電流で動作するので、接触式に比べて通信が安定しないことがあります（ぼくが手に入れた開発用カードは、スマートフォンなどで通信するときタッチして、認識するエリアが気持ちシビアに思います）。

### SONY RC-S300/S（PaSoRi）

　SONYが販売しているRC-S300の業務用バージョンです。
「PC/SC対応」のものが欲しくて、[Switch Science](https://www.switch-science.com/catalog/8015/)で4000円台で購入しました。

　読取り部が中央ではなく少し上（ケーブル接続側）に付いているのか、小さなトークンを読み取るときには気持ち上気味に置く必要があります。

　Macに接続するとgpgからスマートカードの読み取りができませんでした（Windowsなら使えます）。


### ACS ACR1252

　Advanced Card System社が製造しているリーダーです。Amazonで6000円台後半で購入しました。

<iframe sandbox="allow-popups allow-scripts allow-modals allow-forms allow-same-origin" style="width:120px;height:240px;" marginwidth="0" marginheight="0" scrolling="no" frameborder="0" src="//rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=keijiariyama-22&language=ja_JP&o=9&p=8&l=as4&m=amazon&f=ifr&ref=as_ss_li_til&asins=B01HM2S1NQ&linkId=2f4d641c81f824b4aec9c8e6e76e93ea"></iframe>

　サンプルのNFCカードやタグが付属して値段が高くなっているものがあるので注意が必要です。

　MacからでもWindowsからでも、openSC、gpgともに問題なく使えています。
また、APDUコマンドを送ることでリーダーの設定をいろいろ変えられるのでカスタマイズ性が高いことも魅力です。

　難点としてはPaSoRiより少し高価なこと。カードを置いたときや、外したときに「ピッ」と電子音が鳴ることです。
電子音はAPDUコマンドを送ることで消せるのですが、揮発性のメモリにしか保持されず、リーダーを外して再び接続するとまた音が鳴るので、都度設定しなければなりません。
