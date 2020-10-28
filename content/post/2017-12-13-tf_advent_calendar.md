---
title: ココが辛いよ！ TensorFlow
author: Keiji Ariyama
type: post
date: 2017-12-13T00:52:09+00:00
url: /2017/12/tf_advent_calendar.html
categories:
  - TensorFlow
  - 雑記

---
## この投稿は、[TensorFlow Advent Calendar][1]の12日目の記事です。

みなさん。TensorFlow使ってますか？
  
ぼくはと言えば、気がつけば毎日なにがしかのTensorFlowのコードを書いています。

TensorFlowが発表されてからすぐに使い始めて早二年、さまざまな機械学習向けのフレームワークが登場する中、TensorFlowは高い人気を保っていますね。

さて今回の記事は、そんな人気のTensorFlowを使っていて、ぼくが「つらい」と感じる（感じた）ことを書きます。

同じ気持ちの方には共感していただき、「こうすればもっと便利に使えるのに」という方法を知っている方が居られたら是非教えていただきたいと考えています。
  
<!--more-->

## 多すぎる選択肢

TensorFlowにはさまざまなAPIが用意されています。
  
たとえば畳み込み層であれば`tf.nn.conv2d`がもっとも基本的なオペレーションとして挙げることができますが、プリミティブすぎて最近はあまり使っていません。

他にも`tf.layers`モジュールの`tf.layers.conv2d`があります。
  
`tf.layers.conv2d`の引数は、つぎの通りです。

<pre><code class="py">conv2d(
    inputs,
    filters,
    kernel_size,
    strides=(1, 1),
    padding='valid',
    data_format='channels_last',
    dilation_rate=(1, 1),
    activation=None,
    use_bias=True,
    kernel_initializer=None,
    bias_initializer=tf.zeros_initializer(),
    kernel_regularizer=None,
    bias_regularizer=None,
    activity_regularizer=None,
    kernel_constraint=None,
    bias_constraint=None,
    trainable=True,
    name=None,
    reuse=None
)
</code></pre>

さらに`tf.contrib.layers`モジュールにも`tf.contrib.layers.conv2d`があります。

結局のところ、どれを使えばいいのでしょう？

[tf.contrib][2]は本来は変更されたり、消えるかもしれない。将来的にTensorFlowに取り込まれるかもしれないという実験的なコードという位置づけになっています。

では、今は安定版である`tf.layers.conv2d`を使っておいて、将来、contribが外れたときに対応すれば……と思うのですが、`tf.contrib.layers.conv2d`からcontribを外すと、ただの`tf.layers.conv2d`になるので、名前が衝突してしまい、既存のコードに影響が出ます。

現在の`tf.contrib.layers.conv2d`の引数を見てみましょう。

    conv2d(
        inputs,
        num_outputs,
        kernel_size,
        stride=1,
        padding='SAME',
        data_format=None,
        rate=1,
        activation_fn=tf.nn.relu,
        normalizer_fn=None,
        normalizer_params=None,
        weights_initializer=initializers.xavier_initializer(),
        weights_regularizer=None,
        biases_initializer=tf.zeros_initializer(),
        biases_regularizer=None,
        reuse=None,
        variables_collections=None,
        outputs_collections=None,
        trainable=True,
        scope=None
    )
    

活性化関数の指定が`activation_fn`となっています。これでは`contrib`が外れた瞬間にbreaking changeが発生しそうです。
  
また、活性化関数のデフォルト値も`tf.layers.conv2d`は`None`ですが、`tf.contrib.layers.conv2d`は`relu`がデフォルトです。

将来のことを考えると、contribが外れるタイミングで`tf.contrib.layers.conv2d`の方を現行のAPIに寄せるのが妥当と思います。そうなると今`tf.contrib.layers.conv2d`を使ったところは書き直す必要が出てますが、`contrib`は、それがあり得ると説明があるので、しょうがないと言えばしょうがないのでしょう。

そんなわけで、現時点では`tf.contrib.layers.conv2d`は怖くて使えないので、`tf.layers.conv2d`を使っておこうと思っていたら、TensorFlow 1.4.0で`tf.layers.Conv2D`クラスが追加されていました。

コンストラクタを見ると引数やデフォルト値は`tf.contrib.layers.conv2d`に合わせてあるようですが、これは標準のAPIとして提供する必要はあるのでしょうか……？

このようなことが方々であります。

全結合層の名前も`tf.layers.dense`だったり`tf.contrib.layers.fully_connected`だったりと（こちらは名前が違っているのでまだ救いがありますが）、関わっている人たちが自分が慣れている名前や引数をめいめい追加しているだけなんじゃないかと思えてきます。

また、TensorFlowは、Python 2系と3系をサポートしています。
  
サポートするのは良いのですが、その影響でTensorFlowを利用したサンプルコードがPython 2系に向けたものしか用意されていないときがあります。

[Project Magenta][3]など先進的な取り組みも、コードを見に行くとPython 2.7系でしか動かないと書かれています。
  
個人的には仕事以外でPython 2を使うことはしたくないので「あーあ」と思いながら画面を閉じます。

直近の事例では、TensorFlow ServingのPython APIがPython 2向けにしか提供されていないということがありました。

なぜPython 2の方が優先されて実装されるのかより、Python 2しか対応していないなら、それならそうとドキュメントに書いておいて欲しいわけです。

https://www.tensorflow.org/serving/setup

<img src="https://blog.keiji.dev/wp-content/uploads/2017/12/Screen-Shot-2017-12-13-at-8.49.08.png" alt="Screen Shot 2017-12-13 at 8.49.08" width="1706" height="372" class="aligncenter size-full wp-image-1645" />

apt-getでTensorFlow Serving　Serverをインストールして、TensorFlow Serving向けにモデルを出力したりといろいろした後に、pipでPython用のクライアントAPIをインストールしようとしたら「バージョンが違うよ」と表示されるのは、あまりにも残酷な仕打ちです。

そんなわけで、PRを送ってみるなどしました。

https://github.com/tensorflow/serving/pull/685

この一行で救われる原稿があります。

* * *

そんなわけで、[TensorFlow Advent Calendar][1] 12日目の記事は終わりです。
  
明日の担当はLewuatheさんです。

 [1]: https://qiita.com/advent-calendar/2017/tensorflow
 [2]: https://www.tensorflow.org/api_docs/python/tf/contrib
 [3]: https://github.com/tensorflow/magenta