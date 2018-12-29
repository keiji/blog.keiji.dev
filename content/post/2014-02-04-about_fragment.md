---
title: Fragment使う人に最低限、知っておいて欲しいこと
author: Keiji Ariyama
type: post
date: 2014-02-04T09:32:17+00:00
url: /2014/02/about_fragment.html
categories:
  - Android
  - メモ

---
* * *

※ 2015/12/24追記: この記事は古くなっています。最新の情報は[こちら][1]

* * *

`Fragment`は、使いこなすと深い概念です。僕自身、使いこなせているとは言えません。

ただ、昔なら`Activity`だけで乗り切れていたのですが、最近ではAndroidのプロジェクトを作成すると標準で`Fragment`を使ったコードが生成されますし、避けて通ることは難しくなっています。

そしてやっかいなことに、`Fragment`は使いこなすまで恩恵がわかりにくいばかりか、ちょっと変な使い方をすると保守性が急降下します。

「`Fragment`よくわからないな」と、僕と同じ考えをお持ちの皆さん、`Fragment`を使いこなせるようになるまでの間は、最低限、無難な使い方をしましょう。 <!--more-->

## いきなりの結論

**Fragment内部から、そのFragmentを呼び出しているActivity固有のメソッド・メンバ変数を参照してはいけません。**

    public class YYYYFragment extends Fragment implements OnClickListener {
    
        public void onClick(View v) {
            XXXXActivity caller = (XXXXActivity) getActivity();
            caller.submit(); // XXXXActivity固有のメソッドを実行
        }
    }
    

**`instanceof`で型チェックもしないでいきなりキャストとか、ダメ！絶対！**

そのActivity以外でロードされると速攻で落ちます。

こう言う場合、必ず、`Listener`を介してActivity側で処理しましょう。

    public class XXXXActivity extends FragmentActivity implements YYYYFragment.Listener {
        private void submit() {
            // 処理
        }
    
        @Override
        public void onSubmitClicked() {
            submit();
        }
    }
    

　

    public class YYYYFragment extends Fragment implements OnClickListener {
        // リスナー
        public interface Listener {
            public void onSubmitClicked();
        }
    
        private Listener mListener;
        public void setListener(Listener l) {
            mListener = l;
        }
    
        public void onClick(View v) {
            if (mListener != null) {
                mListener.onSubmitClicked();
            }
        }
    }
    

Listenerの設定は、Fragment#onAttach(Activity activity)内でactivityをinstanceofし、Listenerが実装されている事が確認されたタイミングがベストです。

FragmentをロードするActivityから明示的に`setListener`をすることも出来ますが、その場合、Fragmentのライフサイクル、再生成時の挙動には注意してください。再生成時に確実にsetListenerが実行されるようにしなければ、イベントをActivityで受けることが出来ません。

<s>もし、万が一、そのFragmentが、将来にわたってそのActivityからしかロードされないことが確定しているのであれば、そのActivityの内部クラスとして定義しましょう。</s>

**この場合、PlaceholderFragmentはstaticにしておかないと再生成時にエラーが発生するという指摘を<a href="https://twitter.com/zaki50" target="_blank">zaki50</a>さんから頂きました。ありがとうざきさん！**

<s>

    public class XXXXActivity extends FragmentActivity {
    
        private void submit() {
            // 処理
        }
    
        private class PlaceholderFragment extends Fragment implements OnClickListener {
    
            public void onClick(View v) {
                submit(); // XXXXActivity固有のメソッドを実行
            }
        }
    }
    

</s>

FragmentにいちいちListenerを作るのは、めんどくさいように見えても、将来的には得をします。 他の開発者がプログラムを見たときにそのFragmentがどんなイベントをActivityに投げるのか一目瞭然になるからです。

「自分のコードは自分しか見ないよ」と考えている皆さん。

**3ヶ月前のコードを見直す自分は、他の開発者と同じであることを忘れないでください。**

自戒を込めて。

* * *

PS. 他にもいろいろ落とし穴を避ける方法を指摘してもらったのですが、記事がどんどん長くなるので割愛します。Fragmentには落とし穴がいっぱいだけど、今回挙げたのはそれ以前の問題と言うことで。

 [1]: https://blog.keiji.io/2015/12/mincomi-adventcalendar-24.html