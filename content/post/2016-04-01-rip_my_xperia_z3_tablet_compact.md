---
title: 壊れかけのXperia Z3 Tablet Compactに止めを刺した話
author: Keiji Ariyama
type: post
date: 2016-04-01T07:20:15+00:00
url: /2016/04/rip_my_xperia_z3_tablet_compact.html
categories:
  - Android
  - メモ
  - 雑記

---
　昨年の後半のことでしょうか。Xperia Z3 Tablet Compactを落としてしまい、前面ガラスが破損してしまいました。

<div id="attachment_1155" style="max-width: 2170px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_201258.jpg" rel="attachment wp-att-1155"><img src="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_201258.jpg" alt="赤枠で囲った部分が破損箇所" width="50%" height="50%" class="size-full wp-image-1155" /></a>
  
  <p class="wp-caption-text">
    赤枠で囲った部分が破損箇所
  </p>
</div>

<!--more-->

　幸いなことに液晶は割れていなかったので普通に使えていたのですが、見栄えも悪いし、お風呂で表面に水滴が付くとずっとタッチしたままの判定になって操作できなくなるので、思い切ってSONYに見積をお願いしたところ、3万円台の見積が返ってきたので、あっさりとメーカー修理を断念しました。

「Xperia Z3 Tablet CompactがなければNexus 9を使えばいいじゃない（防水じゃないけど）」

　それが今年に入って「液晶は破損してないのだから、最前面のパネル（デジタイザ）だけ交換すれば修理ができるんじゃね？」そんな色気を出してネットを検索、Xperia Z3 Tablet Comact用のデジタイザを見つけて注文。２週間ほどで到着しました。

<div id="attachment_1161" style="max-width: 2170px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_2004412.jpg" rel="attachment wp-att-1161"><img src="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_2004412.jpg" alt="取り寄せたデジタイザ。 送料込みで3500円ほど" width="50%" height="50" class="size-full wp-image-1161" /></a>
  
  <p class="wp-caption-text">
    取り寄せたデジタイザ。<br />送料込みで3500円ほど
  </p>
</div>

　期末で忙しく届いてからしばらく寝かせていたのですが、期末処理もだいたい終えたし、そろそろ交換するかと重い腰を上げたのが三日ほど前のこと。

　しかし、いざ作業を始めて、大きな問題が持ち上がります。Xperia Z3 Tablet Compactの分解方法は、知る限りどこにもない。iFixItにも、Xperia Z3 Compatの分解を解説する記事はあるけど、Tablet Compactはない。

<a href="https://jp.ifixit.com/Teardown/Sony+Xperia+Z3+Compact+Teardown/35454" target="_blank">IFIXIT: Sony Xperia Z3 Compact</a>

　しょうが無いので手探りで分解を始めました。

　結論から言えば、Xperia Z3 Tablet Compactは前面（Front）から分解します。Xperia Z3 Compactのようにバックパネルはないので、後ろをいくら温めても無駄です。

<div id="attachment_1163" style="max-width: 2170px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_2025402.jpg" rel="attachment wp-att-1163"><img src="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_2025402.jpg" alt="ディスプレイを外したところ。銀色の部品がバックライト" width="50%" height="50%" class="size-full wp-image-1163" /></a>
  
  <p class="wp-caption-text">
    ディスプレイを外したところ。銀色の部品がバックライト
  </p>
</div>

　フレキケーブルが90度湾曲しているので、破損にはくれぐれも注意が必要です。

<div id="attachment_1165" style="max-width: 2170px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_202818.jpg" rel="attachment wp-att-1165"><img src="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_202818.jpg" alt="直角に曲がっている＋切れやすそうな切れ目入りのフレキケーブル" width="50%" height="50%" class="size-full wp-image-1165" /></a>
  
  <p class="wp-caption-text">
    直角に曲がっている＋切れやすそうな切れ目入りのフレキケーブル
  </p>
</div>

　分解を進めてわかったのですが、デジタイザと液晶、バックライトの三つのコンポーネントは強力に接着されています。デジタイザと液晶を分離して、デジタイザのみを交換することは至難（実質的に不可能）と思われます。

　僕の場合、バックライトを分離した時点で液晶パネルが破損しました。

<div id="attachment_1164" style="max-width: 2170px" class="wp-caption aligncenter">
  <a href="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_205054.jpg" rel="attachment wp-att-1164"><img src="https://blog.keiji.dev/wp-content/uploads/2016/04/IMG_20160330_205054.jpg" alt="バックライトを剥がした後、液晶をみると細かいひびが……" width="50%" height="50%" class="size-full wp-image-1164" /></a>
  
  <p class="wp-caption-text">
    バックライトを剥がした後、液晶をみると細かいひびが……
  </p>
</div>

　剥がしてる最中、パキパキ音がするからおかしいなとは思ったんですよ。

　デジタイザのみの破損であっても、修理には「デジタイザ＋液晶＋バックライト」のコンポーネントが必要です。けど、それをするくらいなら国内で修理した方がいいです。

　壊れかけでも使えていたXperia Z3 Tablet Compactを僕は、自分の手で、完全にとどめを刺してしまいました。

　同じ状況の人がいてこの記事に行き着いたのなら、僕と同じ間違いを繰り返さぬよう。ここに書き記します。

<a href="http://www.amazon.co.jp/registry/wishlist/3U4RVD7RNXYQK/ref=cm_sw_r_tw_ws_AJH.wbR1DRVPA" target="_blank">ウィッシュリスト: この先生き残るには</a>