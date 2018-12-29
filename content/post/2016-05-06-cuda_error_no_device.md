---
title: TensorFlowでGPUが使えない
author: Keiji Ariyama
type: post
date: 2016-05-06T09:27:03+00:00
url: /2016/05/cuda_error_no_device.html
categories:
  - TensorFlow

---
## あわせて読みたい

  * [TensorFlow on DockerでGPUを使えるようにする方法 | 株式会社カブク][1]
  * [NVIDIA Dockerで簡単にGPU対応のTensorFlow入りコンテナを作る方法 | muo-notes][2]

## 概要

　GPUマシンでTensorFlowを実行する際、CUDAもcuDNNも正常にセットアップできているのに、TensorFlowがGPUを認識しない（`CUDA_ERROR_NO_DEVICE`や`No GPU devices available on machine.`が表示される）場合、正しいディスプレイカード・ドライバがインストールされていない可能性があります。

　筆者の環境では、既存のドライバーをアンインストール（<tt>sudo apt-get remove --purge nvidia*</tt>）して、<a href="http://www.nvidia.co.jp/Download/index.aspx?lang=jp" target="_blank">NVIDIAのサイト</a>からドライバーをダウンロード・インストールすることでGPUを認識するようになりました。

<!--more-->

### 環境

  * ビデオカード GeForce GTX 960
  * OS Ubuntu 14.10 
      * CUDA 7.5
      * cuDNN 7
      * TensorFlow 0.8.0

### 発生した問題

  * TensorFlowの起動直後に<tt>failed call to cuInit: CUDA_ERROR_NO_DEVICE</tt>と<tt>No GPU devices available on machine.</tt>が表示され、CPUでしか演算ができない
  * <tt>nvidia-smi</tt>を実行すると&#8221;No devices were found&#8221;になる
  * <tt>/usr/local/cuda/samples/1_Utilities/deviceQuery</tt>を実行すると<tt>38</tt>で失敗する

### （筆者の環境での）解決法

　まず既存のドライバをアンインストールします。

    $ sudo apt-get remove --purge nvidia*
    

　次に<a href="http://www.nvidia.co.jp/Download/index.aspx?lang=jp" target="_blank">NVIDIAのサイト</a>からドライバーをダウンロード、インストールします。

　筆者の場合、使用しているGTX 960用の最新ドライバ（_NVIDIA-Linux-x86_64-361.42.run_）をインストールしました。
  
（インストール後には再起動が必要）

　きちんと認識すると、TensorFlowの画面に認識したビデオカードに関する情報が表示されます。

    I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:900] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
    I tensorflow/core/common_runtime/gpu/gpu_init.cc:102] Found device 0 with properties: 
    name: GeForce GTX 960
    major: 5 minor: 2 memoryClockRate (GHz) 1.1775
    pciBusID 0000:01:00.0
    Total memory: 2.00GiB
    Free memory: 1.91GiB
    I tensorflow/core/common_runtime/gpu/gpu_init.cc:126] DMA: 0 
    I tensorflow/core/common_runtime/gpu/gpu_init.cc:136] 0:   Y 
    I tensorflow/core/common_runtime/gpu/gpu_device.cc:755] Creating TensorFlow device (/gpu:0) -> (device: 0, name: GeForce GTX 960, pci bus id: 0000:01:00.0)
    

## ここに至った経緯

　「[TensorFlow勉強会（３）][3]」の発表準備中のことです。東京から大阪にあるGPUマシンを操作していたら突然、計算速度ががくんと低下しました。

　TensorFlowを動かしたり止めたりを繰り返していたのでプロセスが残っているのかなと、プロセスをkillしたりしながら様子を見ていると、速度は多少ましになったものの、TensorFlowを起動した時の次の表示が気になりました。

    E tensorflow/stream_executor/cuda/cuda_driver.cc:481] failed call to cuInit: CUDA_ERROR_NO_DEVICE
    I tensorflow/core/common_runtime/gpu/gpu_init.cc:81] No GPU devices available on machine.
    

　あれ？　GPUが見えなくなってる？

　もしかすると停止したプロセスがGPUをつかんだままなのかもしれません。通常であれば再起動するのが手っ取り早いのですが、今回はそうできない事情があります（デュアルブート構成なので、リモートで再起動するとWindowsが立ち上がってしまう）。

　しょうがないので、勉強会で発表する内容は、CPUでできるところまでやりました。そして、大阪に戻ってから再起動をしたのですが、やはりGPUを見つけられません。

　それどころか、Ubuntuを再インストールしても、やはり「GPUがない」と言われるのです。

　TensorFlowのGitHub Issueを眺め、環境変数（CUDA\_VISIBLE\_DEVICES）を設定するなど試行錯誤を続けても、問題は解決しませんでした。

## 解決。そして

　結論として前述の通り、NVIDIAの配布しているドライバーをインストールすると、正常にGPUで演算ができるようになりました。

　普通ならここでめでたしめでたしなんですけど、気になるのは、じゃあいつからGPUを使えてなかったのかって話です。

　NVIDIAのドライバーなんてこれまでインストールはおろかダウンロードもした記憶ないです。ずっとUbuntu付属のドライバー使ってきました。つまり。最初っからGPUは使えてなかったってことじゃないでしょうか。認めたくはありませんが……CUDAのライブラリを正常にロードできていることに安心して、エラー表記を見落としていた可能性が非常に高いと思います。

　僕、今年に入ってから、TensorFlowを題材にけっこう発表とかしてます。「GPUを導入したら速かった！　GPUすげー！」とか、みんなの前で言ってるんです。

　何回も。何回も。

　GPUマシンを導入して以来、TensorFlowの演算をするときはGPUが頑張ってくれてるものだと思ってました。けれど、実際に働いていたのはCPUで、GPUはずっと寝ていた。そして僕は、そのことを知らずに勉強会で「GPUはやっぱり速いですよ」って、みんなの前で発表していたわけです。

　まったく、とんだピエロですよ！

　そんなわけで、これまで発表を聞いてくれた皆さんごめんなさい。ぼくはGPUを使っていませんでした！

　今、僕の目の前ではGPUを使った演算の経過が表示さています。

    2016-05-06 17:15:51.129638: step 12570, loss = 14.01 (143.9 examples/sec; 0.889 sec/batch)
    2016-05-06 17:16:00.183510: step 12580, loss = 14.00 (144.0 examples/sec; 0.889 sec/batch)
    2016-05-06 17:16:09.199027: step 12590, loss = 14.01 (147.2 examples/sec; 0.870 sec/batch)
    

　いやぁ、GPUはやっぱり速いわ。

## 技術書典

　TensorFlowに関しては失敗続きの僕が、TensorFlowを題材にした同人誌を出します。

　６月２５日に秋葉原の通運会館で開かれる技術系同人誌オンリーイベント「[技術書典][4]」にて頒布します（今回は有償頒布の予定）。

<div id="attachment_1186" style="max-width: 645px" class="wp-caption aligncenter">
  <a href="https://techbookfest.org/#A-28" rel="attachment wp-att-1186"><img src="https://blog.keiji.io/wp-content/uploads/2016/05/5690091590647808.png" alt="6/25 技術書典 A-28" width="50%" height="50%" class="size-full wp-image-1186" /></a>
  
  <p class="wp-caption-text">
    6/25 技術書典 A-28
  </p>
</div>

　皆様、技術書典のA-28のブースでお目にかかりましょう！

## 更新履歴

2016/5/8: あわせて読みたい追記・インストールしたドライバーのバージョンについて追記
  
2016/5/13: 技術書典のブースリンクを更新

 [1]: http://www.kabuku.co.jp/developers/errors-with-tensorflow-on-gpu
 [2]: http://www.muo.jp/2016/05/nvidia-docker-tensorflow.html
 [3]: http://connpass.com/event/27081/
 [4]: https://techbookfest.org/