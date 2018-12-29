---
title: Circle CIで新しいバージョンのbuildToolsが見えなかった話
author: Keiji Ariyama
type: post
date: 2015-11-25T08:34:21+00:00
url: /2015/11/update_build_tools_version.html
categories:
  - Android
  - メモ

---
Circle CIでビルドしているプロジェクトのBuildToolsのバージョンを`23.0.2`にしたところ、android update sdkに、「そんなバージョンはない」と言われるようになった。

`Error: Ignoring unknown package filter 'build-tools-23.0.2'`

原因はandroid update sdkの中にtoolsを含めていなかったという、完全な僕の不注意。

    echo y | android update sdk --no-ui --all --filter tools
    

を、スクリプトに足して無事解決。

`tools`を更新しないと見に行かないリポジトリがあるんだね……