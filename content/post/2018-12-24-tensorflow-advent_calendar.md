---
title: いまNN API（TensorFlow Lite）は使えるのか
author: Keiji Ariyama
type: post
date: 2018-12-24T05:30:34+00:00
url: /2018/12/tensorflow-advent_calendar.html
categories:
  - Android
  - TensorFlow

---
　この記事は[TensorFlow Advent Calendar 2018][1]の24日目の記事です。

　23日目の記事は、AtuNukaさんによる「[Design Documentから見たTensorFlow 2.0の変更点][2]」でした。

<!--more-->

## はじめに

　あいかわらず趣味でTensorFlowを使っていて、最近はAndroidで動作させるTensorFlow Liteまわりを重点的に取り組んでいます。

　アドベントカレンダーの22日目の記事、足立氏（カブク社）による「[いまMLKitカスタムモデル（TF Lite）は使えるのか][3]」に書かれているように、TensorFlowのモデルをAndroid端末で動かすことに関しては「使える」レベルになってきていると筆者も思います。

　そこで今日は、TensorFlow LiteのモデルをAndroidアプリに組み込んで動作させるとき、NN APIを使うように設定すると「動かない」という話をします。

「使えるレベルになってきている」と、言った直後にこんなことを言ってごめんね。

　でも、本当です。
## TL;DR

　TensorFlow LiteのモデルをAndroidアプリに組み込むには、TensorFlow Liteそのものの制約に加えて、量子化済みモデルの制約。そしてNN APIの制約の「３つの制約」を最大公約数的にクリアする必要がある。

　モデルの作成を進め、いざAndroidアプリに組み込もうとしたときに制約に引っかかると、大きな手戻りが発生したり、当初予定されていた性能が出ないなどの問題が起こる可能性がある。

　それらの問題を防ぐために、モデルを設計して、学習プロセスを開始する前段階で、TensorFlow Liteの量子化モデルへの変換と、NN APIを使ったときに問題なく動作するかを確認するプロセスを設けることを強く推奨する。

　今後、TensorFlow LiteモデルのAndroidアプリへの組み込みが増えていくに従い、Androidアプリエンジニアと機械学習エンジニアは、より緊密にコミュニケーションをしていく必要があると筆者は考えている。


## NN APIとは

　NN(Neural Networks) APIは、Android 8.1から追加された、モバイル端末上で機械学習の計算処理を実行するためのAPIです。

Neural Networks API

<https://developer.android.com/ndk/guides/neuralnetworks/?hl=ja>

　NN APIを通じて機械学習の計算処理をすると、端末に搭載されているGPUや機械学習の演算を専門とするプロセッサ（e.g. Pixel Visual Core）上で実行したり、それらがない場合はCPUにオフロードしたりすることで、高速な実行が期待できます。

　NN APIは、Cで提供されています。提供されている[サンプルアプリ][4]はJNIを使っています。

　JavaとKotlinでアプリ作ってるマンには厳しい世界です。

## TensorFlow Liteとは

　TensorFlow Liteは、TensorFlowで作成した機械学習のモデルをモバイル端末や組み込みデバイスで動作させる仕組みです。Google I/O 2017で発表されました。

TensorFlow Lite

<https://www.tensorflow.org/lite/>

　TensorFlow LiteをAndroidアプリ開発で使うには、[tensorflow-lite][5]ライブラリを使います。

　8月にバージョン（`1.10.0`）を出してから、TensorFlow本体が順調にバージョンアップしているにも関わらずまったく動きを見せず、12月3日に突然、`1.11.0`と`1.12.0（Breaking Changes入り）`を同時リリースするという男気あふれるライブラリです。皆さんも是非使ってみてください。

### TensorFlow Mobile

　TensorFlow Liteより前に、TensorFlow Mobileというライブラリも提供されていました。現在も提供されていて、メンテナンスもされているのですが、2019年のはじめにはdeprecated（非推奨）になることがアナウンスされています。

Optimizing for mobile

<https://www.tensorflow.org/lite/tfmobile/optimizing>

　 筆者が以前執筆に参加した「TensorFlow活用ガイド」では、AndroidアプリからTensorFlowのモデルを利用するのにTensorFlow Mobileを使っていたので、こちらが完全に死亡宣告を受けた感じです。

<a href="https://www.amazon.co.jp/dp/B07921ZC62/ref=as_li_ss_il?_encoding=UTF8&#038;btkr=1&#038;linkCode=li2&#038;tag=keijiariyama-22&#038;linkId=4a00c5b2fbe48f21047a9b8d25e01e01&#038;language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&#038;ASIN=B07921ZC62&#038;Format=_SL160_&#038;ID=AsinImage&#038;MarketPlace=JP&#038;ServiceVersion=20070822&#038;WS=1&#038;tag=keijiariyama-22&#038;language=ja_JP" /></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=keijiariyama-22&#038;language=ja_JP&#038;l=li2&#038;o=9&#038;a=B07921ZC62" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />

　まぁ、足が早いものになるであろうことは承知していましたし、評判が良ければ改訂もできるので、皆さん是非買ってください。レビュー平均が1.5とか、逆に珍しいと思います。きっと興味がわいたことでしょう。

　皆さん是非買ってください。

## TensorFlow Lite用のモデル出力

　TensorFlowで作成したモデルをTensorFlow Liteで動作させるには、TensorFlow Lite用にモデルを出力する必要があります。

　TensorFlowが出力するデータ形式は[Protocol Buffers][6]が多いのですが、TensorFlow Liteでは[FlatBuffers][7]が採用されています。

　これは、FlatBuffersはデータにアクセスする際にパースを必要とせず、メモリのフットプリント（占有容量）が小さいと言う特徴があり、モバイル端末や組み込みデバイスに適しているためです。

　TensorFlow Liteモデルに変換するには、コマンドラインツールとPython APIが提供されています。

（ツールには`TOCO`と`tflite_convert`がありますが、今後は`tflite_convert`に統一されていくと聞いてます）

　コマンドラインツールを使うにせよ、Python APIを使うにせよ、基本的には、モデルとパラメーターを従来形式（Protocol Buffers）で出力した後、TensorFlow Lite用に変換する行程になります。

　ちなみに`TOCO`と`tflite_convert`、そしておそらくPython APIも、Windowsでは動作しませんが、UbuntuとかmacOSではちゃんと動くからなんの問題もない。

Toco not working on Windows #20975

<https://github.com/tensorflow/tensorflow/issues/20975>

## NN APIとTensorFlow Lite

　前に述べたとおり、Java（Kotlin）ばかりを使っている筆者にはNN APIを使うための処理を全部Cで書くのはハードルが高いのですが、TensorFlow LiteのモデルをAndroidアプリで動かすときにNN APIを使うように指定することで、計算処理をNN APIを使って実行できます。

　NN APIを使うには、TensorFlow Liteの`Interpreter`クラスをインスタンス化するときに`Options`オブジェクトを渡します。

        TfLiteModel(AssetManager am) throws IOException {
            ByteBuffer byteBuffer = loadModelFile(am);
            Interpreter.Options options = new Interpreter.Options().setUseNNAPI(true);
            tfInterpreter = new Interpreter(byteBuffer, options);
        }


　`setUseNNAPI(true)`が、NN APIを利用する設定です。

　で、NN APIを利用したら、それまで普通に使えていたモデルが動かなくなったという話です。

## 遭遇した課題

　GitHubで公開している[food\_gallery\_with_tensorflow][8]の、`tflite_quantize`ブランチが、TensorFlow Lite（tflite）の量子化モデルに対応したコードです。

　このコードをベースに`setUseNNAPI(true)`を設定して実行すると、モデルの初期化時にエラーになりました。

    E/tflite: NNAPI doesn't support tensors with rank 0 (index 88 name mul/y)
    E/tflite: Returning error since TFLite returned failure nnapi_delegate.cc:745.
    E/tflite: Failed to build graph for NNAPI


　ありがたいことに（本当にありがたいことに）、`tensorflow-lite 1.11.0`からエラーの内容がきちんと表示されるようになりました。この場合、`NN API`はrank `0`のTensorをサポートしていないとエラーが出ています。

　ランク`0`のTensorはスカラー型を意味します。

### 量子化モデルの制約

　ここで、モデルを出力したPythonスクリプトを見てみます。

    INPUT_CHANNEL = 4

    FLAGS = tf.flags.FLAGS
    tf.flags.DEFINE_string('tag_name', None, "タグ名")
    tf.flags.DEFINE_string('train_path', None, "学習結果のパス")
    tf.flags.DEFINE_string('graph_dir', './', "学習結果のパス")


    def _export_graph(sess):
      constant_graph_def = tf.graph_util.convert_variables_to_constants(
        sess, sess.graph_def, ['result'])

      tf.train.write_graph(constant_graph_def, FLAGS.graph_dir,
                           'model_4ch_flot32.pb', as_text=False)


    def main(args=None):
      assert FLAGS.tag_name, 'tag_name is not set.'
      assert FLAGS.train_path, 'train_path is not set.'

      with tf.Graph().as_default() as g:
        image_ph = tf.placeholder(
          tf.float32,
          [model.IMAGE_SIZE * model.IMAGE_SIZE * INPUT_CHANNEL],
          name='input')
        normalized_image = image_ph * (1.0 / 255.0)

        image = tf.reshape(
          normalized_image,
          [1, model.IMAGE_SIZE, model.IMAGE_SIZE, INPUT_CHANNEL])
        image = image[:, :, :, :3]

        logits = model.layers(FLAGS.tag_name, image, 1, training=False)
        result = tf.nn.sigmoid(logits)
        result = tf.fake_quant_with_min_max_args(
          result,
          name='result'
        )

        saver = tf.train.Saver(tf.global_variables())
        with tf.Session() as sess:
          saver.restore(sess, FLAGS.train_path)
          _export_graph(sess)


    if __name__ == '__main__':
      tf.app.run()


　`Placeholder(name: input)`に`(1.0 / 255.0)`を乗算しています。

        image_ph = tf.placeholder(
          tf.float32,
          [model.IMAGE_SIZE * model.IMAGE_SIZE * INPUT_CHANNEL],
          name='input')
        normalized_image = image_ph * (1.0 / 255.0)


　これは入力画像の値（`0-255`）を`0.0-1.0`に正規化する処理です。なぜ素直に`image_ph / 255.0`としないかというと、TensorFlow Lite形式に変換する際、パラメーターを量子化しようとすると「`Div`をサポートしていない」エラーが発生するためです。

    $ tflite_convert \
        --graph_def_file food_model_4ch.pb  \
        --output_file food_model_quant_4ch.tflite \
        --inference_type QUANTIZED_UINT8  \
        --inference_input_type QUANTIZED_UINT8  \
        --input_arrays input  --output_arrays result  \
        --std_dev_value 1 --mean_value 0 \
        --default_ranges_min=-6 --default_ranges_max=6
    （略）
    F tensorflow/contrib/lite/toco/graph_transformations/quantize.cc:474] Unimplemented: this graph contains an operator of type Div for which the quantized form is not yet implemented. Sorry, and patches welcome (that's a relatively fun patch to write, mostly providing the actual quantized arithmetic code for this op).\n"


　量子化モデルでは単純な除算（`Div`）が使えません（量子化しなければ使えます）。そこで`(1.0 / 255.0)`の結果を乗算することで、`255.0`で除算するのと同じ結果にしています。

　そもそもなんで`255.0`で除算するのか。`tf.image.per_image_standardization`という便利なものがあるだろうと思ったあなた。安心してください。

　TensorFlow Liteの量子化モデルでは`tf.image.per_image_standardization`は使えません。

     F tensorflow/contrib/lite/toco/graph_transformations/quantize.cc:474] Unimplemented: this graph contains an operator of type ReduceProd for which the quantized form is not yet implemented. Sorry, and patches welcome (that's a relatively fun patch to write, mostly providing the actual quantized arithmetic code for this op).\n"


　あと筆者の知るかぎりでは`tf.expand_dims`が使えなかったりと、普通に`tflite_convert`を使っていて問題なく変換できるモデルでも、量子化するときに制約に引っかかるということは珍しくありません。大変ですね。

### NN APIの制約

#### スカラーをサポートしない

　除算をサポートしていない件については乗算に置き換えて解決しました。しかし今度は値がスカラーなのでNN APIでエラーが発生します。

また、量子化していないTensorFlow Liteのモデルでも、同様のエラーが発生します。

（ 厄介なのは、NN APIを使わなければ普通に動いてしまうということです）

　スカラーをサポートしていないなら、要素数`1`の配列として計算するように調整します。

        image_ph = tf.placeholder(
          tf.float32,
          [model.IMAGE_SIZE * model.IMAGE_SIZE * INPUT_CHANNEL],
          name='input')
        normalized_image = image_ph * [1 / 255.0]


#### Op code 45（スライス処理）をサポートしない

　調整済みのスクリプトでモデルを出力して、さらに変換したTensorFlow Lite形式のモデルを、NN APIを使う設定で実行した結果が次の通りです。

    E/tflite: Op code 45 is currently not delegated to NNAPI
    E/tflite: Returning error since TFLite returned failure nnapi_delegate.cc:745.
    E/tflite: Failed to build graph for NNAPI


`Op code 45`は現状は使えないと言ったエラーになりました。`Op code 45`といわれても、かいもく見当が付きませんね。

　TensorFlow Liteのオペレーションは、[TensorFlowのGitHubリポジトリ][9]に定義されています。これを見ると`45`に対応するオペレーション名が`kTfLiteBuiltinStridedSlice`だとわかります。

　前述のスクリプトにはスライス処理をしている部分がありました。

        image = tf.reshape(
          normalized_image,
          [1, model.IMAGE_SIZE, model.IMAGE_SIZE, INPUT_CHANNEL])
        image = image[:, :, :, :3]


　これは、RGBAの4chの画像のアルファチャンネルを捨てる処理です。

　もともとAndroidアプリ側で画像を`scaledBitmap`を使ってモデルの入力に適したサイズに縮小しているのですが、サイズ変更処理後に`copyPixelsToBuffer`でバイト列を取ると、得られるバイト列が`RGBA8888`となります。

　この処理で得られるTensorのShapeは`[1, model.IMAGE_SIZE, model.IMAGE_SIZE, 3]`。つまり`RGB888`のデータです（最初の足はミニバッチサイズを表しています）。

　[公式のサンプル][10]では、アプリ側で縦横2重のfor文と、ビットシフトを使ってRGBを取り出しています。

      /** Writes Image data into a {@code ByteBuffer}. */
      private void convertBitmapToByteBuffer(Bitmap bitmap) {
        if (imgData == null) {
          return;
        }
        imgData.rewind();
        bitmap.getPixels(intValues, 0, bitmap.getWidth(), 0, 0, bitmap.getWidth(), bitmap.getHeight());
        // Convert the image to floating point.
        int pixel = 0;
        long startTime = SystemClock.uptimeMillis();
        for (int i = 0; i < DIM_IMG_SIZE_X; ++i) {
          for (int j = 0; j < DIM_IMG_SIZE_Y; ++j) {
            final int val = intValues[pixel++];
            imgData.putFloat((((val >> 16) & 0xFF)-IMAGE_MEAN)/IMAGE_STD);
            imgData.putFloat((((val >> 8) & 0xFF)-IMAGE_MEAN)/IMAGE_STD);
            imgData.putFloat((((val) & 0xFF)-IMAGE_MEAN)/IMAGE_STD);
          }
        }
        long endTime = SystemClock.uptimeMillis();
        Log.d(TAG, "Timecost to put values into ByteBuffer: " + Long.toString(endTime - startTime));
      }


　筆者は、以前この処理を見て「モデルにはRGBAで入力して、上述のように内部でアルファチャンネルを捨てればいいのに、なんでこんな無駄なことをするのだろう」と思っていましたし、方々でドヤ顔をして話をしていたりしたのですが、おそらく、NN APIの制約に対応するためにこのような処理にしているのだとわかりました。がっかりだ。

　そこで、非常に不本意ではありますが、モデルに入力するデータを`RGB`の3チャンネルと定義して、スライスの処理を取り除きました。

　また、アプリ側でアルファチャンネルを除去するように調整しました。

        val inputByteBuffer: ByteBuffer = ByteBuffer
                .allocateDirect(IMAGE_WIDTH * IMAGE_HEIGHT * 3)
                            .order(ByteOrder.nativeOrder())
        val resultArray = Array(1, { ByteArray(1) })

        fun recognize(imageByteBuffer: ByteBuffer): Float {
            for (index in 0 until IMAGE_BYTES_LENGTH) {
                if ((index % 4) != 3) {
                    inputByteBuffer.put(imageByteBuffer[index])
                }
            }
            inputByteBuffer.rewind()

            val start = Debug.threadCpuTimeNanos()
            tfInference.run(inputByteBuffer, resultArray)
            val elapsed = Debug.threadCpuTimeNanos() - start
            Log.d(TAG, elapsed.toString())

            inputByteBuffer.clear()

            return resultArray[0][0].toInt().and(0xFF) / 255.0F
        }


　この調整により、無事、NN APIを有効にした状態でモデルが動作するようになりました。

#### そのほかの制約

　NN APIでは、このほかにも`tf.tanh`など、重要な処理（と、筆者は考えている）をサポートしていなかったりします。

　ちなみに`tf.tanh`は、TensorFlow Liteそのものがサポートしていないと記述がありますが、実際にはNN APIを使わなければ動作します。

TensorFlow Lite & TensorFlow Compatibility Guide

[https://www.tensorflow.org/lite/tf\_ops\_compatibility#unsupported_operations][11]

## 評価

[food\_gallery\_with_tensorflow][8]で利用しているTensorFlow Liteの量子化済みモデルを、Essential PH-1、Pixel 2、Pixel 3で動作させて、実行時間を評価しました（搭載しているバージョンは3機種ともAndroid 9.0）。

　評価プログラムを次に示します。小さなモデルを使っているため、ナノ秒単位で計測しています。

            val start = Debug.threadCpuTimeNanos()
            tfInference.run(inputByteBuffer, resultArray)
            val elapsed = Debug.threadCpuTimeNanos() - start
            Log.d(TAG, elapsed.toString())


### 評価結果

　それぞれで推論を11回実行し、初回を除いた（理由は後述）10回分の推論にかかった時間の平均を性能として表に示します。

| 機種名            | NN APIあり  | NN APIなし      |
| -------------- | --------- | ------------- |
| Essential PH-1 | 556,323ns | 185,372,624ns |
| Pixel 2        | 450,807ns | 187,395,465ns |
| Pixel 3        | 477,489ns | 129,994,563ns |

　今回、量子化済みのモデルで評価した結果、NN APIを使うと実行時間が大幅に短縮されるという結果になりました。

　以前、量子化をしていないTensorFlow Liteモデル（[TensorFlow for Poets 2][12]: Inception-v3モデル）を使ってPixel 2で計測したときは、NN APIを使うと逆に遅くなる結果を得たことがあります（[Liteな期待が重すぎる！ （茶色いトイプーは食べ物じゃないっ!?）][13]）。

　今回の結果を見てしまうと、TensorFlow Liteの量子化モデル＋NN API以外の選択は考えられません。

　なお、Pixel 3がPixel 2より性能が低いという結果になっていますが、実際にはそうではなく、十分な性能のGPUで小さなモデルを動かすときに性能が生かし切れないのと同様のことが起こっているのだと筆者は考えています。

### NN API初期化のタイミング

　NN APIを使った場合、初回の推論に時間がかかります。Essential PH-1で計測した生データを次に示します。

    3189426
    499012
    396042
    395209
    433593
    516042
    627552
    625572
    580572
    887918
    601719


　この結果から、NN APIを使う場合、モデルをロードするタイミングではなく、実際に`run`するタイミングでグラフを構築していると推測できます。

　今後のバージョンアップなどで挙動が変わる可能性は多分にありますが、現時点では、ダミーデータなどを使って初回の`run`を実行しておくことで、初回の実行時間を短縮する手法が考えられるでしょう。

## おわりに: われわれはどうすべきか

　モデルを設計して、学習プロセスを開始する前に、TensorFlow Liteの量子化モデルへの変換と、NN APIを使ったときに問題なく動作するかを確認するプロセスを設けることを強くオススメします。

　学習を進めて、いざ組み込もうとしたときにいずれかの制約に引っかかれば、前処理やモデルの変更などによる学習プロセスのやり直しなど、大きな手戻りが発生したり、当初予定されていた性能（実行時間）が出ないなどの問題の引き金になる可能性があります。

　パラメーターは初期値でいいのです。モデルの精度はひとまず問題ではありません。制約をクリアしているか、性能は十分に出るかの評価を優先しましょう。

　そのためには今後、Androidアプリエンジニアと機械学習エンジニアは、より緊密にコミュニケーションをしていく必要があると筆者は考えています。

* * *

　この記事は[TensorFlow Advent Calendar 2018][1]の24日目の記事です。

　25日目の記事は「natsutan」さんによる「[GCPでTPU動かしました][14]」です。

 [1]: https://qiita.com/advent-calendar/2018/tensorflow
 [2]: https://qiita.com/AtuNuka/items/6966efeddceb96012819
 [3]: https://www.kabuku.co.jp/developers/how-is-tflite-useful
 [4]: https://github.com/googlesamples/android-ndk/tree/master/nn_sample
 [5]: https://bintray.com/google/tensorflow/tensorflow-lite
 [6]: https://developers.google.com/protocol-buffers/
 [7]: https://google.github.io/flatbuffers/
 [8]: https://github.com/keiji/food_gallery_with_tensorflow
 [9]: https://github.com/tensorflow/tensorflow/blob/master/tensorflow/lite/builtin_ops.h
 [10]: https://github.com/googlecodelabs/tensorflow-for-poets-2/blob/master/android/tflite/app/src/main/java/com/example/android/tflitecamerademo/ImageClassifier.java
 [11]: https://www.tensorflow.org/lite/tf_ops_compatibility#unsupported_operations
 [12]: https://github.com/googlecodelabs/tensorflow-for-poets-2
 [13]: https://booth.pm/ja/items/1043447
 [14]: http://natsutan.hatenablog.com/entry/2018/12/25/120450
