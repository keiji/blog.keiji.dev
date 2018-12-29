---
title: VPNの”切断中”が終わらない
author: Keiji Ariyama
type: post
date: 2013-02-18T14:55:39+00:00
url: /2013/02/forever-vpn-disconnecting.html
categories:
  - メモ

---
<span>MacのVPN接続で、切断時にいつまでも&#8221;Disconnecting&#8221;が表示され続ける現象の解決策。</span>

<div style="text-align: center; margin-bottom: 1em;">
  <span style="background-color: #fcfcfc;">sudo killall -9 pppd</span>
</div>

<span>Macでトラブルに遭遇したら、とりあえずなんかのプロセスを殺せば良いらしい。けど、覚えている自信が無いのでメモ。</span>