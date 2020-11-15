---
title: 矩形切り出しツール「Region Cropper」を公開しました
author: Keiji Ariyama
type: post
date: 2016-09-15T07:17:36+00:00
url: /2016/09/about_region_cropper.html
categories:
  - TensorFlow
  - 習作
  - 雑記

---
----
**2020年11月15日追記:** 「Region Cropper」は非推奨となりました。Javaの実行環境に「Java FX」が標準で含まれなくなったため、導入のハードルが上がったことが非推奨とした原因です。

代替として「[MRDB-web-client](https://keiji.github.io/mrdb-web-client/)」の利用を検討してください。

----

時間を見つけてはTensorFlow関係のあれこれをちまちま続けています。

先日は「[なにわTECH道][1]」で、TensorFlowを使ったイラスト顔検出について発表しました。

<!--more-->

## データセットの作成

これまで、機械学習に使う画像（データセット）の作成にはMacの「プレビュー」を使っていました。
  
１枚ずつ画像を読み込んでクロッピング（トリミング）していたのですが、１枚の画像に複数の顔があるときや、あとで領域を微調整（特に領域を拡大）したいときが面倒でした。

良さそうなツールがないか、少しばかり探してみたのですが、僕の思い描くような操作性のものがありませんでした。

なければ作れば良い。

そんなわけで、自分で画像の切り出しツールツール「Region Cropper」を開発しました。
  
１人で使うのももったいないので、GitHubで公開します。

## Region Cropper

GitHub: <https://github.com/keiji/region_cropper>

<img src="https://blog.keiji.dev/wp-content/uploads/2016/09/set_region.png" alt="set_region" width="2048" height="1580" class="aligncenter size-full wp-image-1383" />

GUIのフレームワークに、使い慣れたSwingではなくJava FXを使いました。
  
そのため、Java 8のUpdate40以降でしかきちんと動きません。

以下、Region CropperのREADME.mdから。

## Region Cropper

### Requirements

  * Java > 1.8.0_40

### Download

Binary(Zip): <https://github.com/keiji/region_cropper>

### 使い方（Usage）

    $ java -jar region_cropper_main.jar
    

#### 処理するディレクトリの選択（Select Directory）

<img src="https://blog.keiji.dev/wp-content/uploads/2016/09/select_directory.png" alt="select_directory" width="1424" height="896" class="aligncenter size-full wp-image-1382" />

#### 領域の追加（Add)

ドラッグ&ドロップで領域を設定します。

#### 領域の選択（Select)

現在選択中の領域は赤枠で示されます。

<img src="https://blog.keiji.dev/wp-content/uploads/2016/09/set_region.png" alt="set_region" width="2048" height="1580" class="aligncenter size-full wp-image-1383" />

スペース（Space）キーを押すと、選択中の領域以外の場所がグレーアウトされます。

<img src="https://blog.keiji.dev/wp-content/uploads/2016/09/focus_region.png" alt="focus_region" width="2048" height="1580" class="aligncenter size-full wp-image-1384" />

| キー            | 動作                         |
|:------------- |:-------------------------- |
| Enter         | 次の領域へ移動。最後の領域であれば次のファイルを表示 |
| Shift + Enter | 前の領域へ移動。最初の領域であれば前のファイルを表示 |
| Tab           | 次の領域へ移動                    |
| Shift + Tab   | 前の領域へ移動                    |
| End           | 次のファイルを表示                  |
| Home          | 前のファイルを表示                  |

#### 領域の編集（Edit）

| キー                             | 動作              |
|:------------------------------ |:--------------- |
| 方向(←↑→↓)                       | 領域を移動           |
| Alt または Option + 方向(←↑→↓)      | 領域を「拡大（Expand）」 |
| Windows または Command + 方向(←↑→↓) | 領域を「縮小（Shrink）」 |
| 削除（Delete）                     | 領域を「削除（Delete）」 |

Shift を押すと、変化量が小さくなり微調整できます。

#### ラベルの設定（Label）

数字キーの「0〜9」を押すと、選択した領域にラベルを設定します。
  
追加した領域のラベルは、標準で 1 が設定されています。

<img src="https://blog.keiji.dev/wp-content/uploads/2016/09/set_label.png" alt="set_label" width="2048" height="1580" class="aligncenter size-full wp-image-1387" />

また、ラベル 0 は負例として特別な意味を持ちます。
  
負例に設定した領域は、編集や削除ができません。

#### 領域の切り出し（Crop）

領域の切り出しには「Quick crop」と「Crop to&#8230;」の２つの方法があります。

<img src="https://blog.keiji.dev/wp-content/uploads/2016/09/menu_crop.png" alt="menu_crop" width="639" height="327" class="aligncenter size-full wp-image-1386" />

「Quick crop」を選択すると、設定している領域を、処理中の画像と同じディレクトリに切り出します。
  
出力先のディレクトリを設定したい場合は「Crop to&#8230;」を選択します。

    [出力先のディレクトリ]/label-[ラベル]/[画像名]-[領域番号].png

 [1]: https://www.pasonatech.co.jp/skill_up/event/naniwa_tech.jsp