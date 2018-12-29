---
title: Archive Utilityがフリーズ
author: Keiji Ariyama
type: post
date: 2013-02-06T07:55:00+00:00
url: /2013/02/archive-utility.html
blogger_blog:
  - blog.keiji.io
blogger_author:
  - Keiji Ariyama
blogger_permalink:
  - /2013/02/archive-utility.html
categories:
  - メモ

---
<span>Macのアーカイブユーティリティで、ZIPを展開しようとすると無応答になってしまう現象の解決策。</span>

<div style="text-align: center; margin-bottom: 1em;">
  <span style="background-color: #fcfcfc;">sudo killall -KILL appleeventsd</span>
</div>

<span>月に何度も遭遇する不具合ではないけど、発生したときに検索するのが面倒なのでメモ。</span>