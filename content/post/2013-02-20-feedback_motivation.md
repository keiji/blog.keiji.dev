---
title: フィードバックとモチベーション
author: Keiji Ariyama
type: post
date: 2013-02-19T19:18:51+00:00
url: /2013/02/feedback_motivation.html
categories:
  - Android
  - AOSP
  - 雑記

---
昨日、知り合いが、[新しい&#8221;Draw 9-Patch Tool&#8221;][1]に関するフィードバックを、Twitterを通じてくれたのだけど、やりとりする上で、なんだかとても苛ついていたのを、自分でも不思議に思っていました。

<div align="center">
  <blockquote class="twitter-tweet">
    <p>
      Windowsうざい
    </p>
    
    <p>
      &mdash; Keiji Ariyama (@keiji_ariyama) <a href="https://twitter.com/keiji_ariyama/status/303789291740475392">February 19, 2013</a>
    </p>
  </blockquote>
  
  <p>
    </div> 
    
    <p>
      &nbsp;<br /> あれからしばらく考えて、何となく自分の中で納得ができたので、まとめておこうと思います。<br /> <!--more-->
    </p>
    
    <h2>
      機能要件と非機能要件
    </h2>
    
    <p>
      要するに、その人のフィードバックの意図するところが、ソフトウェアが本来の機能要件を満たさないという指摘であるのか、それとも非機能要件に関わる、例えば動作が遅いとか、ユーザビリティに関するものなのか。<br /> まず始めにその点を明確にして、両者で共有できていなかったのが、一番の原因だと思います。
    </p>
    
    <p>
      <span style="font-size: 1rem; line-height: 1.714285714;">特に、非機能要件については、フィードバックをくれる当人の主観にもとづく割合が非常に大きいので、</span><span style="text-decoration: underline;"><span style="font-size: 1rem; line-height: 1.714285714;">僕が共感できない場合は、全く必要性が理解できずに「それは僕はいらないと思うんで」としか答えられなかったりします。</span></span>
    </p>
    
    <p>
      &nbsp;
    </p>
    
    <div align="center">
      <blockquote class="twitter-tweet">
        <p>
          @<a href="https://twitter.com/yokmama">yokmama</a> ドット表示にしてしまうと、ドットを打つというインタラクションをユーザーに与えてしまいますが、本来のdraw9patchの用途は範囲を指定するのであって、コチコチドットを打つような使い方はしませんしするべきではありません。なので僕はいらないと思います。
        </p>
        
        <p>
          — Keiji Ariyama (@keiji_ariyama) <a href="https://twitter.com/keiji_ariyama/status/303766253384921088">February 19, 2013</a>
        </p>
      </blockquote>
      
      <p>
        &nbsp;
      </p>
    </div>
    
    <h2>
      開発コスト
    </h2>
    
    <p>
      また、新しい&#8221;Draw 9-Patch Tool&#8221;は、AOSPに取り込まれれば、僕の手を離れることになる。言わば、<strong>嫁に出すのが前提のソフトウェア</strong>です。
    </p>
    
    <p>
      非機能要件上の課題は、将来、他のコントリビューターが改善の提案をしてくれるんじゃないかと期待しているので、現時点では、開発（メンテナンス）コストの増大を招く方向に舵を切るつもりはないのです。
    </p>
    
    <p>
      なので、自分が必要性を感じない、しかしフィードバックをくれる人が望んでいる非機能要件の実現に、メンテナンスコストがスカイ・ハイな「OpenGL」の利用を提案されたりすると、その瞬間に<strong>ストレスがマッハ</strong>になるわけで。。。
    </p>
    
    <div align="center">
      <blockquote class="twitter-tweet">
        <p>
          @<a href="https://twitter.com/yokmama">yokmama</a> 知らん
        </p>
        
        <p>
          — Keiji Ariyama (@keiji_ariyama) <a href="https://twitter.com/keiji_ariyama/status/303770058818220032">February 19, 2013</a>
        </p>
      </blockquote>
      
      <p>
        &nbsp;
      </p>
    </div>
    
    <h2>
      反省と改善
    </h2>
    
    <p>
      <span style="line-height: 1.714285714; font-size: 1rem;">しかし、フィードバックをくれる人も、純粋な善意でやってくれているので、それを無碍に扱うというのは絶対に宜しくない。<br /> </span><span style="line-height: 1.714285714; font-size: 1rem;">その点は反省しつつ、今後、このようなことを繰り返さないためにどのような改善をすれば良いかというと、<strong>フィードバックを受け付ける際、それが&#8221;Bug Report&#8221;か&#8221;Feature Request&#8221;なのかをラベル</strong>することが考えられます。</span>
    </p>
    
    <p>
      &#8220;<strong>Bug Report</strong>&#8220;については、動かないという第一報からエラーの確認・修正まで、SNSを通じて対応出来るかも知れません。
    </p>
    
    <p>
      <strong>だが、&#8221;Feature Request&#8221;。テメーはダメだ。</strong><br /> Githubで管理して、他の人の賛成・反対意見も聞きながら、判断する基準を作る必要があるでしょう。
    </p>
    
    <p>
      ってことで、まだまだ改善の余地はありそうですね。
    </p>
    
    <p>
      &nbsp;
    </p>

 [1]: https://blog.keiji.dev/2013/02/newdraw9patchtool.html