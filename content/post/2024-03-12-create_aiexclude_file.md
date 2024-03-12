---
title: Studio Bot用の .aiexclude ファイルを作った話
author: Keiji Ariyama
type: post
date: 2024-03-12T19:00:00+09:00
url: /2024/03/create_aiexclude_file.html
description: Studio Botで送信対象から外すファイルをプロジェクトごとに指定できる .aiexclude ファイルを作った話
images: 
  - /images/aiexclude/studiobot.png
categories:
  - 雑記
  - Android
draft: false

## 要約
Studio Botを完全に有効にしたAndroid Studioでプロジェクトを読み込むと、プロジェクトの内容が外部に送信される可能性があります。
`.aiexclude`ファイルを作成しておくことで外部へ送信しない（学習対象としない？）ファイルを指定できます。
お仕事でアプリを作っている皆さん。プロジェクトの中に（.gitignoreに追加した上であっても）機微情報を配置している人は、Studio Botを使う、使わないにかかわらず`.aiexclude`を作成しておくのが良さそうです。

https://developer.android.com/studio/preview/studio-bot/data-and-privacy#aiexclude

----

本当はもっといろいろ書こうと思ったけど、いま書きたいことはこれだけでした。

筆者は生成AIを否定する立場になく、Studio Botは便利に使わせてもらっています。Studio Botを使った感想とかは時間があったら書きます。
