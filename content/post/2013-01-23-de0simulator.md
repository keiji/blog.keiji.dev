---
title: DE0メモ – シミュレーター(ModelSim)
author: Keiji Ariyama
type: post
date: 2013-01-22T16:31:00+00:00
url: /2013/01/de0simulator.html
blogger_blog:
  - blog.keiji.io
blogger_author:
  - Keiji Ariyama
blogger_permalink:
  - /2013/01/de0simulator.html
categories:
  - DE0
  - FPGA
  - メモ

---
前回の『<a href="https://blog.keiji.io/2013/01/de0.html" target="_blank">DE0メモ</a>』で、

> その他、シミュレータなどでいろいろ検証したりもあるようですが、僕はまだその段階にありません。

と、書いたのですが、

**「普通はシミュレータの方が先だよ」**との意見を戴きました。

また、DE0を使って実験をしていると、なんだかよくわからない挙動をするときがあるんですね。そういう時に中をひょいと覗けないのは、思いのほか辛いものです。

さらに、DE0を持ち歩いているわけではないので、ちょっと試したいときにはシミュレーターが使えれば、ある程度の実験は出来るでしょう。

そんなわけで、使ってみました。シミュレーター。

<!--more-->


  
使用したのは、「**ModelSim ALTERA STARTER EDITION 10.1b**」です。Quatus IIとは別にインストールする必要がありますが、無償で配布されているのでダウンロードしてくるだけです。

## Quartus II

シミュレーターを使う前にまず、Quartus IIでVerilog HDLの簡単なモジュールを書いて、実際にDE0で動かしてみます。

<div>
  <pre>module ToggleLed(btn, led);

 input btn;
 output led;
 reg state = 1'b0;

 assign led = state;

 always @(negedge btn) begin
  state = !state;
 end

endmodule</pre>
</div>

ピンアサインは、

<table>
  <tr>
    <td>
      btn:
    </td>
    
    <td>
      F1 (BUTTON2)
    </td>
  </tr>
  
  <tr>
    <td>
      led:
    </td>
    
    <td>
      J1 (LED0)
    </td>
  </tr>
</table>

これをDE0に書き込むと、タクトスイッチ(BUTTON2)を1回押すと、LED0が点灯、もう一度押すと消灯します。**今回、チャタリング防止は組み込んでいません。**

<div>
  <a href="https://blog.keiji.io/wp-content/uploads/2013/01/Photo-22-01-13-08-20-47-0.jpg"><img class="aligncenter  wp-image-159" alt="Photo-22-01-13-08-20-47-0" src="https://blog.keiji.io/wp-content/uploads/2013/01/Photo-22-01-13-08-20-47-0.jpg" width="576" height="432" /></a>
</div>

## ModelSim

実機での確認が終わったら、いよいよModelSimを起動して、新しいテスト・プロジェクトを作成します。

<div>
  Quartus IIで記述したVerilog HDLファイルをインポートします。<br /> （インポートしたファイルの編集は、ModelSimでも出来ます。）
</div>

<div>
  <a href="https://blog.keiji.io/wp-content/uploads/2013/01/Parallels-Picture.jpg"><img class="aligncenter  wp-image-160" alt="Parallels Picture" src="https://blog.keiji.io/wp-content/uploads/2013/01/Parallels-Picture.jpg" width="576" height="360" /></a>
</div>

<div>
</div>

<div>
</div>

### テスト・ベンチの記述

テスト・ベンチは、<span>ToggleLedモジュールを記述したファイルにそのまま</span>記述できます。どのモジュールをテストするのかをあとから指定するので、名前は関係ないようです。

<div>
  <pre>module ToggleLedTest();

  reg btn = 1'b0;
  wire led;

  reg [3:0] i;

  ToggleLed toggleLed(btn, led);

  initial begin
    $monitor("button:%b, led:%b", btn, led);
    for(i=0; i &lt; 15; i=i+1) begin
      #1
      btn = !btn;
    end
  end
endmodule</pre>
</div>

### シミュレーションの実行

テスト・ベンチを追記したVerilog HDLをコンパイルして、&#8221;Start Simulation&#8221;を実行すると、どのモジュールをテストするのか指定する画面になるので、ここでテスト・ベンチのモジュール名（ToggleLedTest）を選択します。

最後に、&#8221;Run&#8221; -> &#8220;Run -all&#8221;を実行すれば、シミュレーターは、テストベンチを実行して、monitorで指定している内容をコンソール(transcript)に出力します。

<div>
  <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span><br /> <span># button:1, led:0</span><br /> <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span><br /> <span># button:1, led:0</span><br /> <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span><br /> <span># button:1, led:0</span><br /> <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span><br /> <span># button:1, led:0</span>
</div>

&nbsp;

**しかし、これは期待している結果ではありません。**
  
この結果では、テストを開始した最初の時点で、ledが1(HIGH)になっています。ボタンを最初に押すまでは0であることを想定しています（実際、DE0の動作もそうなっています）。

よくよく見てみると、テスト・ベンチに記述している**btnの初期値が1&#8217;b0**になっています（**DE0のタクトスイッチは押していない状態が1で、押した状態が0**）。
  
btnの初期値を1&#8217;b1に修正して、再度シミュレーションを実行すると、

<div>
  <span># button:1, led:0</span><br /> <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span><br /> <span># button:1, led:0</span><br /> <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span><br /> <span># button:1, led:0</span><br /> <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span><br /> <span># button:1, led:0</span><br /> <span># button:0, led:1</span><br /> <span># button:1, led:1</span><br /> <span># button:0, led:0</span>
</div>

&nbsp;

期待したとおりの動作となりました。

### 波形の観察

あらかじめ指定した各レジスタや入出力の波形を表示する機能もあります。
  
これで、挙動がおかしいときにも、内部の状態を視覚的に確認することが出来そうです。

<div>
  <a href="https://blog.keiji.io/wp-content/uploads/2013/01/Screen-Shot-2013-01-23-at-1.12.31-AM.png"><img class="aligncenter  wp-image-161" alt="Screen Shot 2013-01-23 at 1.12.31 AM" src="https://blog.keiji.io/wp-content/uploads/2013/01/Screen-Shot-2013-01-23-at-1.12.31-AM.png" width="545" height="231" /></a>
</div>