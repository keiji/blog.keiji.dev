---
title: AOSPで１度投稿したパッチを修正する（nobranch編）
author: Keiji Ariyama
type: post
date: 2013-01-15T13:47:00+00:00
url: /2013/01/aospnobranch.html
blogger_blog:
  - blog.keiji.dev
blogger_author:
  - Keiji Ariyama
blogger_permalink:
  - /2013/01/aospnobranch.html
categories:
  - Android
  - AOSP
  - メモ

---
この連休で<a href="https://android-review.googlesource.com/#/" target="_blank">Android Open Source Project</a>にいくつかパッチを投稿したのですが、そのときに、ちょっと詰まったのでメモ。
  
<!--more-->

<div>
  <p>
    AOSPにパッチを投げるには、
  </p>
  
  <ul>
    <li>
      トピックブランチを作成
    </li>
    <li>
      変更をローカルにコミット
    </li>
    <li>
      repo upload
    </li>
  </ul>
  
  <p>
    と言う手順でやります。詳細は<b>&#8220;<a href="http://visible-true.blogspot.jp/2012/12/aaptresaosprepo-upload.html" target="_blank">visible true</a>&#8220;</b>に良くまとまっているので、参考にしてください。
  </p>
  
  <p>
    そして、一度投稿したパッチを修正するときは、最初に作ったトピックブランチをチェックアウトして、&#8221;git commit &#8211;amend&#8221;でコミットを修正してから、もう一度&#8221;repo upload&#8221;するわけですが、今回は、<b>パッチを投稿したときのトピックブランチが失われた状態</b>だったのです。
  </p>
  
  <p>
    そのパッチを最初に投稿したのは半年以上前で、そのときは4.1が出たての頃でした。それから時は流れ、再び修正することになったのですが。。。
  </p>
  
  <p>
    いろいろ試行錯誤をしましたが、下記の手順で修正できました。
  </p>
  
  <p>
    ベストプラクティスではないかもしれませんが、参考までに。
  </p>
  
  <div style="border: solid 1px #666666; padding: 1em;">
    > git fetch https://android.googlesource.com/platform/sdk refs/changes/30/49630/3 && git checkout FETCH_HEAD<br /> > git checkout -b [BRANCH_NAME]<br /> > git add [FILE_NAME]<br /> > git commit &#8211;amend<br /> > git branch &#8211;set-upstream [BRANCH_NAME] aosp/master<br /> > repo upload
  </div>
  
  <p>
    &nbsp;
  </p>
  
  <p>
    一行目のgit fetchは、AOSPのreviewサーバーに表示されているcheckout（画像下部、赤線内）をそのまま実行します。
  </p>
  
  <div>
    <a href="https://blog.keiji.dev/wp-content/uploads/2013/01/Screen-Shot-2013-01-15-at-10.01.18-PM.jpg"><img class="aligncenter  wp-image-86" alt="Screen Shot 2013-01-15 at 10.01.18 PM" src="https://blog.keiji.dev/wp-content/uploads/2013/01/Screen-Shot-2013-01-15-at-10.01.18-PM.jpg" width="461" height="293" /></a>
  </div>
  
  <p>
    すると、該当の修正をチェックアウトした状態になります。
  </p>
  
  <p>
    その際、一旦branchが外れて(no branch)になるので、もう一度、トピックブランチを作り直します。以後、修正はこのブランチ内で行います。
  </p>
  
  <p>
    ファイルを修正して、git addで取り込み、直前のコミットを修正(amend)します。
  </p>
  
  <p>
    そして、ここがハマったところなのですが、<b>トピックブランチのupstreamを、手動で設定しなくてはなりません。</b>これをしないと、repo uploadをしたときに、投稿できるブランチがないと言われてしまいます。
  </p>
  
  <p>
    upstreamを設定したら、あとは通常通りrepo uploadでパッチを投稿できます。投稿したパッチは、AOSPのreviewサーバーの既存のエントリを上書きする形で表示されます。
  </p>
</div>