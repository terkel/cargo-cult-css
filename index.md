# カーゴ・カルト CSS

CSS を書いたり管理したりするにはなんらかの方法論があった方が良い、と広く考えられている。しかし実際に取り入れられている手法の中には、セマンティクス上の品質や、長期にわたるメンテナンス性に悪影響を与えるものもある。ここでは、CSS の「フレームワーク方法論」として提唱されているテクニックの問題点や、その問題を僕たちウェブ・ディベロッパーがどうすれば解決できるかについて論じてみようと思う。

現在、CSS 開発におけるフレームワーク方法論として、[BEM][1] など類似のテクニックがいくつかあるが、もっとも有名なのは [OOCSS][2] だろう。これらの方法論は CSS にオブジェクト指向プログラミングの原則を適用しようと試みる。しかしながら、両者の間にはそもそも*宣言型スタイル言語*と*オブジェクト指向ソフトウェア設計原則*というコンセプト上の不一致がある。その結果、経験の浅いディベロッパーが気づきにくいような複雑な問題を持ち込んでしまう。たいへん困ったことに、著名なブロガーたちが「ベスト・プラクティス」として喧伝しているおかげで、これらの方法論の採用が広く見られるようになってしまった。ごく一部の高いトラフィックを持つサイトを除いて、これら方法論によって得られるとされる利点を証明する根拠はない。このことからもわかるとおり、CSS のフレームワーク方法論は、人を惑わせ害を及ぼす [カーゴ・カルト][3] の典型であると僕は考える。

## セマンティクス {#semantics}

> コンピューター・サイエンスで難しいことは 2 つしかない。キャッシュの無効化と、命名だ。
> <footer>— フィル・カールトン[^Karlton]

ウェブは基本的に*セマンティックなメディア*だ。様々な言語、文化、性、世代、肉体的および認知的能力を持った人々が、あらゆる種類の技術を通じて利用できることを目指すプラットフォームとして、この点はきわめて重要だ。ウェブがどうあるべきか、またはどう見えるべきかを示す唯一のビジョンはない。明らかに限られた領域で開発しているのでない限り、セマンティックなアプローチこそがウェブ・ディベロッパーのあらゆる行動の中心であるべきだ。

HTML を書くとき、ウェブサイトやアプリケーションのインターフェースやコンテンツのセマンティクスを表現するにはおもに 3 つの方法がある。コンテンツをマークアップする*要素タイプ*、*ほかとは異なる個別の要素*を特定する *ID*、そして要素がどの*集合*に属すかカテゴライズする*クラス名*だ。

この 3 つの中で、HTML ドキュメントにプレゼンテーションを結びつけるにはクラス名がもっとも一般的に使われている。クラス名について論じるにあたって、W3C の最新の HTML5 勧告候補が次のように記している点に注意してほしい。

> `class` 属性で用いることのできるトークンにとくに制限はないが、その内容にどのような体裁を持たせたいかではなく、その内容の性質を表現する値が推奨される。[^HTML5-DOM]

`class` 属性の仕様のどこを見ても、この属性の果たす*目的*として、コンテンツのセマンティクスを表現するということ*以外*は書いていない。にもかかわらず、じつに多くのウェブ・ディベロッパーが `class` 属性について「CSS クラス」などといった間違った呼び方をしている。タンテク・チェリクはこう書いている。

> 「CSS クラス」や「CSS クラス名」といった言い回しを使うということは、その言い回しが不正確である (あるいはただ単に間違っている) というだけではない。それは「CSS」のプレゼンテーショナルな文脈と概念をクラス名と結びつけ、プレゼンテーショナルなクラス名を使うというバッド・プラクティスをほのめかしたり、あまつさえ奨励したりすることになる。[^Celik]

フレームワーク方法論は勧告と完全に食い違っているが、彼らは勧告を軽視する道を選んだ。その手のプロジェクトでのクラス名はプレゼンテーションを表現している*必要があり*、非セマンティックにならざるを得ない。[そうではないと主張しているにもかかわらず][4]。それにしてもなぜ、セマンティックなクラス名がそこまで重要なのか? もっとも基本的なレベルで言うと、ティム・バーナーズ＝リーが「[ウェブの普遍性][5]」と表現したところの、プラットフォームに依存しない、包括的デザイン原則の維持が目的だ。また、いま僕たちが作っているものが、将来どんな技術によって利用されることになるかは予測できないからでもある。

> HTML がセマンティックかどうか簡単にテストするには、その HTML がパブリック API として利用できるかどうかを考えてみるといい。
> <footer>— アレックス・ゲイナー[^Gaynor]

この件についての例として [Microformats][6] が挙げられる。Microformats が生まれたのは、ドキュメントの製作者が住所やカレンダーといったよくある仕組みを表現するのに*セマンティックなクラス名*を使ってきたからにほかならない。このようにデータを表現する上での慣例を体系化できれば、マークアップはウェブサイトの利用者だけではなく、それをまったく新しいやり方で利用するソフトウェアにとっても有益なものになる。Microformats がセマンティックなクラス名は有用だと証明しているのに、それが否定されるのを見ると残念な気持ちになる。

> クラス名は、機械にも人間にも、セマンティックな情報を伝えることはほとんどない。それが Microformats という、合意を得た (かつマシーン・リーダブルな) 名前の小さな集まりでもない限り。
> <footer>— ニコラス・ギャラガー[^Gallagher]

これは恣意的な表現だ。今日作られ、発表されているコンテンツの、将来にわたる利用のすべての可能性に対して、「合意を得た名前の集まり」とは。この表現は CSS フレームワークのイデオロギーを正当化したいがためのものだ。非セマンティックなクラス名を許すと、ドキュメントの構造とコンテンツがプレゼンテーションの影響を受けるようになってしまう。それは明らかに [関心の分離][7] に反する。

## メンテナンス性 {#maintainability}

ウェブサイトを作るとき、僕たちはつねに、そのサイトがいつまでも簡単にメンテナンスや変更ができるよう、充分に将来を見すえて考えなければならない。今日のウェブサイトやアプリケーションは、ますます大きなフロントエンド開発チームを必要とするようになってきている。そういった環境では、CSS を書くための方法論がチーム全体にうまく取り入れられることがとても重要だ。チームの何人かはプロダクトの一生のうち一時期しか関わらないだろうし、
何人かはプロダクトの初期には参加していなかっただろう。そこではまた、CSS の実装に関して多くの難題が持ち上がる。たとえば…

- コードの繰り返し・重複
- プロダクトを通じた一貫性
- パフォーマンス

CSS フレームワーク方法論の背後にあるもっとも重要な目的は、このうち最初の、サイト中の CSS コードの重複の問題を解決することだろう。クラス名による短いセレクターは、実用的で、あらゆる要素間でルールを共有できる。次の例を見てほしい。このクラスはどんな要素にも同じボーダー、パディング、タイポグラフィを適用する。

```css
.box-standard {
    color: blue;
    border: 2px solid blue;
    border-radius: 5px;
    padding: 20px;
    font-family: Helvetica, Arial, sans-serif;
    font-weight: normal;
    font-size: 1rem;
    line-height: 1.4;
}
```

このクラスをどのような要素に適用してもそのスタイルになる。もしそのスタイルの全部は必要なく、かわりに特定のプロパティを上書きしたいなら、次のソース例のように別のクラスを宣言すればいい。これらのクラスの特定度は同じなので、後者の宣言が優先される。

```css
.box-special {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

```html
<div class="box-standard box-special">
    Here is a special box!
</div>
```

やりたかったのはコードの重複を減らすことだったはずだが、この方法でクラスを実装すると、マークアップはこの*両方*のクラスの存在に永遠に縛られることになる。どちらのクラスも特定のセマンティクスを伝えていないにもかかわらず。

こんなときこそ求められるのが、[LESS][8] や [Sass][9] などの CSS プリプロセッサーにある*ミックスイン*や*継承*の組み合わせだ。必要なプロパティは[ミックスイン][10]や[プレースホルダー][11]でひとまとめにできるし、もろにプレゼンテーショナルな名前をつけても (デバッグのためにあえてそうしない限り) 生成されるコードには現れないし、そのルールはどんなセレクターにも使い回せる。以下は [SCSS 構文][12]でのミックスインの簡単な例。

```scss
@mixin news-item($color) {
    border: 2px solid $color;
    border-radius: 5px;
    padding: 20px;
    font-family: Helvetica, Arial, sans-serif;
    font-weight: normal;
    font-size: 1rem;
    line-height: 1.4;
}

div.news {
    @include news-item(blue);
}

div.breaking {
    @include news-item(red);
    font-weight: bold;
}
```

```html
<div class="news">
    Here is a news item.
</div>

<div class="breaking">
    Here is a breaking news item!
</div>
```

ミックスインのスタイル宣言に変更を加えるとつねに `div.breaking` セレクターも連動する。マークアップとはこれ以上ないくらい疎結合の状態だ。こういったミックスインをネイティブな CSS で定義できないのはとても残念だが (ありがたいことにようやく[正式に検討されはじめた][13])、プロとして CSS を書くなら CSS プリプロセッサーを使わない理由はどこにもない。

疎結合や*[関心の分離][14]*といったものは、プロジェクトの将来にわたるメンテナンスにおいて極めて重要だ。マークアップと CSS を密結合にしすぎると修正のコストが増大してしまう。CSS がまだ生まれて間もない頃には、Dreamweaver や Frontpage といった WYSIWYG エディターの増殖が多くのウェブサイトを次のようなコードへと導いたものだった。

```html
<span style="display: block; font-family: Arial, sans-serif; font-size: 11px; color: blue; border-style: solid; border-color: blue; border-width: 2px; padding: 20px;">Here’s a box.</span>
<span style="display: block; font-family: Arial, sans-serif; font-size: 11px; color: blue; border-style: solid; border-color: blue; border-width: 2px; padding: 20px;">Here’s another box.</span>
<span style="display: block; font-family: Arial, sans-serif; font-size: 11px; color: blue; border-style: solid; border-color: blue; border-width: 2px; padding: 20px;">Here’s yet another box. Noticing a trend yet…?</span>
```

こういったインラインのスタイルに基づいたマークアップは、90 年代後半にウェブをおとしめていたグチャグチャのタグの山に比べれば多少はまし、というものでしかない。デザインが変更されるたび、これらすべてのインライン・スタイルを追跡して書き換える必要がある。僕らはこの手の地獄のような開発からは逃げ出したが、しかし悲しいことに、これまでの教訓も忘れ去られてしまったらしく、いまだに次のようなコードに遭遇することがある。

```html
<span class="display-block blue-box font-arial color-blue solid-blue-border padding-20">Party like it’s 1999!</span>
<span class="display-block blue-box font-arial color-blue solid-blue-border padding-20">Hey, have you checked K10K.net lately?</span>
<span class="display-block blue-box font-arial color-blue solid-blue-border padding-20">Wassuuuuuuup!</span>
```

This might seem like an extreme example, but it’s the logical conclusion of a CSS framework methodology that abandons semantic class names and selectors in the pursuit of ‘modularisation’. While avoiding duplication of code within the CSS, it is simply transferred to the markup, incurring close-coupling side-effects in the process.

極端な例に見えるかもしれないが、これは CSS フレームワーク方法論が「モジュラー化」を追い求めてセマンティックなクラス名とセレクターを捨てた当然の帰結だ。CSS 内でのコードの重複を避ける一方、その重複はただ単にマークアップに転移し、その過程で密結合という副作用を招いている。

また CSS フレームワーク方法論の別の目標として、高いパフォーマンスの実現というものもある。パフォーマンスは多くの要因に依存するものだ。ウェブページを利用しているクライアントの種類や、接続速度、コンテンツのキャッシュなど。基本的に、CSS のパフォーマンスに明らかに影響するのは次の 3 つだ。

- HTTP リクエストの数
- キャッシュの状態
- ドキュメントのサイズ

このうち最初の 2 つは CSS フレームワーク方法論の範囲ではないが、3 つめにはクラス名の再利用というかたちで取り組んでいる。その意図は、スタイルのプロパティの宣言を可能な限り少なくすれば、CSS はおのずと小さくなるだろう、というものだ。

しかしながら、そういった CSS の配信は誤った方向に向かう恐れがある。セレクターを短く、ルールを少なくし、多くの束縛を (たくさんの長いクラス名とともに) マークアップに持ち込んでも、転送されるデータ量は必ずしも小さくならない。この方法は多くのものを CSS ファイルから HTML ドキュメントに運び出すが、CSS はたいてい CDN による積極的なキャッシュと配信がおこなわれるのに対し、HTML はまったくキャッシュされないかもしれないのだ。

CSS セレクターを短く保つことはパフォーマンス上の利点があると、CSS フレームワーク方法論の支持者はしばしば引き合いに出す。しかしこのことは [すでに反証されており][15]、モダンなブラウザー・エンジンはごくまれな場合を除いて充分に手際が良く、セレクターを書き換えることによるスピードの向上は取るに足りない程度のものだ。一方で、フロントエンドのパフォーマンスには追求する価値があるのは間違いない。

> 多くの人が気づいていないと思うんだけど (それとも話そうとしないだけ?)、パフォーマンスの実現には現実的かつ具体的なコストがかかることがよくあって、メンテナンス性を食いつぶす結果にもなる。
> <footer>— ニールス・マティス[^Matthijs]

CSS フレームワーク方法論のもっとも深刻な問題のいくつかは、古いコードを見返したときや、新しいチーム・メンバーがプロジェクトに加わったときに露わになる。これはおもに「探しやすさ」をほとんど考慮していないことが原因だ。ニコラス・ギャラガーはフロントエンド・アーキテクチャーについての記事で「クラス名はディベロッパーに有益な情報を伝えるものであるべきだ」と言っているが、僕はこれをもう一歩推し進めたい。*セレクター*はディベロッパーに有益な情報を伝えるものであるべきだ。両者の目的は同じで、*そのルールがどこで使われているのか知りたい*というものだが、セレクターは*コンテキスト*を伝えるという点でずっと有益だ。もしセレクターが*決められた範囲内*の要素にだけ適用されるということがわかれば、そのルールの背後にある意図を理解するのはずっと簡単だ。次に挙げる、名簿のリンクをスタイリングする 2 つの方法について考えてみてほしい。

```css
.list-link {
    font-weight: bold;
    text-decoration: none;
}
```

またはこう。

```css
ul.members li a {
    font-weight: bold;
    text-decoration: none;
}
```

後者は、そのコードベースにはじめて触れたり、しばらく離れていたりして馴染みが薄かったとしても、難なく理解できるはずだ。そしてまた「探しやすい」という性質も持っている。新入りのデベロッパーの場合を考えてみよう。彼らからすると、その CSS コードベースを丸ごと読んだり (あるいは覚えたり) しない限り、そこに一貫したルールがあるかどうか知りようがない。彼らに過度な期待をするのは現実的ではないだろう。彼らは十中八九、繰り返しを減らすという目標に完全に反して、同じ結果を得るために別の新しいルールを書こうとする。

> エレガントなコードはただ正しいだけではなく、目に見えて、明らかに正しい。単にコンピューターのためのアルゴリズムというだけではなく、それを読む人間の心理に理解や確信を促してくれる。コードのエレガンスを探求することで、私たちはより良いコードが書けるようになる。明解なコードの書き方を学ぶことは、エレガントなコードの書き方を学ぶための大きな第一歩だ。そしてコードを探しやすくしようとすることは、それを明解にすることにつながる。エレガントなコードは明解で、そして探しやすくもある。
> <footer>— エリック・S・レイモンド[^Raymond]

探しやすさの問題と関連して、フレームワーク方法論はまた、ソフトウェアが肥大する要因にもなる。具体的なセレクターを書けば、そのセレクターがすでに不必要になっていないかどうかを判断する材料になる。先ほどの後者の例では、名簿のリストがウェブサイトから削除された場合、その CSS ブロックをまるごと消すことができる。一方でフレキシブルなクラス名ベースのセレクターはほかで使われている可能性があり、そのルールを削除しても安全かどうかすぐにはわからないので、クリーンアップに地道な努力を必要とする。

パフォーマンスについて最後に、マット・ウィルコックスの完璧な説明を引用しよう。

> パフォーマンスはブラウザー・レベルの問題で、HTML/CSS のオーサーが解決したり克服したりするものじゃない。妥当でよく考えられた HTML と CSS を書いている限り、与えられたページのレンダリング速度を向上するだけのこんな馬鹿げたルールに手を出す必要はない。それにブラウザーはリリースごとに速度が向上している。パフォーマンスのためにコードを最適化しなきゃならないのはもっぱら JS 野郎たちだ。もし HTML/CSS を最適化したいなら、それはベスト・プラクティスを目指してやるべきで、パフォーマンスのためにやるべきじゃない。パフォーマンスがいまいちだったとしてもブラウザーが新しくなれば勝手に良くなるが、その場しのぎでメンテナンスしづらい意味不明のコードはそうはいかない。[^Wilcox]

## で、僕らに何ができる? {#so-what-can-we-do}

フロントエンド・ディベロッパーたちは何年もの間、CSS フレームワーク方法論*なし*で、大きなウェブ・プロジェクトをおとなしく、そして上手いこと進めてきた。OOCSS や BEM といったものを使わずともこういったプロジェクトがなんとかなったという事実は、ほかに効果的な方法があるという証拠だ。ここでは、僕と一緒に CSS について議論してきた人たちからの助言を、僕が経験から得たいくつかの提案とともにいくつか紹介したい。

### 「ID を使ってくれ、頼むから」 {#use-ids-for-the-love-of-god}

CSS フレームワーク方法論者の中には、[ID を使うのは良くない][16]と言う人たちがいる。ID は特定度 (specificity) が高いので、(いまいましい) `!important` なしではルールを上書きできなくなってしまう恐れがある、というのがその理由だ。僕はこの意見に強く反対する。[アンガス・クロールはこう書いている][17]—「厄介ごとを避けようという方針は困ったものだ。なぜなら、言語をマスターするにはその言語のすべてを知らなければならないし、恐れたり逃げたりすることは知識を得ることの妨げになるのだから」。もし君がプロのウェブ・ディベロッパーなら、特定度がどのような仕組みで、そしてそれが君の書くセレクターにどのように影響するのかを理解すべきだ。さらに、ID はドキュメントの構造上妥当だし、[とても有益な機能とセマンティックな目的がある][18]。

### 「セレクターは必要に応じて具体的かつ説明的に。それ以上でも以下でもなく」 {#write-selectors-that-are-as-specific-and-descriptive-as-they-need-to-be}

もしその CSS が色んなところに出現する可能性のある要素をターゲットにしているなら、そのようなセレクターを使おう。もしその CSS がごく限られた場合にしか使われないなら、そのようなセレクターを使おう。なにより、誰かが半年後に見たときにどんな目的なのかわからないような、いい加減なセレクターにしちゃいけない。

### 「再利用可能なモジュールなどない (実際に再利用されるまでは)。リファクターやリライトはあとまわし」 {#itll-never-be-a-reusable-module}

特定度の高い CSS を書くことに不安を覚えたとしたら、それはつまり時期尚早な最適化をしようとしているということだ。まず役目を果たすのに充分な CSS を書き、そしてそれが再利用されることが明らかになったときにはじめて適切なミックスインとしてリファクタリングしよう。来週には優先事項が変わり、その機能をまるごと捨てることになるかもしれないって? でも、半年後に誰かが見たときに目的が理解できず、消しても大丈夫かわからないような、余分なクラス名のセレクターの山なんて誰もほしくないはずだ。

### 「たくさんのファイルもひとつのビルド・プロセスで」 {#many-files-and-a-build-process}

君はプロのウェブ・ディベロッパーだ。コードを書くのにもうメモ帳を使ってはいないだろうし、作業にもっとも適したツールを使うこと。CSS プリプロセッサーのツールにはフレームワークやプラットフォームにかかわらず利用可能なものがある。そしてまた、プロジェクトを越えて CSS を効果的に再利用するにはどうすればいいのかという [良い事例][19] もたくさんある。ほかのプロジェクトではどのように仕事をしているのかに触れ、知見を広げよう。

### 「静的なプロトタイプを作ろう。すべてのスクリーンショットを残そう」 {#build-static-prototypes}

ウェブサイトやアプリケーションはふつう、よくあるイディオムの集まりから出来ている。ラベルと入力ボックスからなるフォーム、`nav` 要素内のリンクのリスト、ラベルと値のリストなど。どのような機能の実装にとりかかるにせよ、まずは静的な HTML として適切な構造のマークアップを組み立てることからはじめよう。この方法はコードがあとあと破綻しないためのテスト・ケースになるだけではなく、チームの新しいメンバーが途中経過を知るための助けにもなる。また手動でも自動のテスト・スイートでも、ページやアプリケーションのスクリーンショットを撮っておけば、継続的インテグレーションのテストにおいて壊れている箇所を見つけるのに役立つ。

---

複雑なウェブ・アプリケーションにおいて、優れた CSS アーキテクチャーを構築するのは難しい (結局のところ、良いフロントエンド・ディベロッパーがこれほど求められる理由はここにある)。かと言って独善的な「ルール」を課してしまうと、それはプロジェクトに新しく加わる人にとってもすでに関わっている人にとっても、事態をより困難にするだけだ。そのルールは最後には、無駄な複雑さや、うんざりするようなメンテナンス・コストを生じることになる。

自分が作っているプロダクトの目的を自問してみれば、プロダクトのあらゆる面について、どう表現するかだけではなく、どう構造化するかについてより重視するようになるはずだ。ウェブはセマンティックなメディアである、ということを指針にしよう。

> すべての有機物と無機物、すべての形而下のものと形而上のもの、すべての人間的なものと超人間的なもの、知恵と心と魂のすべての真の姿に、広く浸透する法則がある。生命はその表現により識別され、形態はつねに機能に従うという法則だ。
> <footer>— ルイス・サリヴァン[^Sullivan]

## 謝辞 {#thanks}

この記事を書くにあたっては、以下の人たちからのフィードバックに大いに助けられた。[Mark Norman Francis][20]、[Brad Wright][21]、[Ross Bruniges][22]、[Jake Archibald][23]、そして [Patrick Griffiths][24]。

  [^Karlton]: [Two Hard Things][25]

  [^HTML5-DOM]: [W3C: Semantics, structure, and APIs of HTML documents][26]

  [^Celik]: [Why you should say HTML classes, CSS class selectors, or CSS pseudo-classes, but not CSS classes][27]

  [^Gaynor]: [Twitter / alex_gaynor: Quick test for if your HTML ...][28]

  [^Gallagher]: [About HTML semantics and front-end architecture][29]

  [^Matthijs]: [The Cost of Performance][30]

  [^Raymond]: [The Art of Unix Programming][31]

  [^Wilcox]: [CSS Lint is harmful][32]

  [^Sullivan]: [Wikipedia: Form follows function][33]

  [1]: http://bem.info/method/definitions/ "Block, Element, Modifier — Definitions"
  [2]: https://github.com/stubbornella/oocss/wiki/faq "OOCSS FAQ"
  [3]: http://neurotheory.columbia.edu/~ken/cargo_cult.html "“Cargo Cult Science” by Richard Feynman"
  [4]: http://nicolasgallagher.com/about-html-semantics-front-end-architecture/ "About HTML semantics and front-end architecture"
  [5]: http://www.scientificamerican.com/article.cfm?id=long-live-the-web "Long Live the Web: A Call for Continued Open Standards and Neutrality"
  [6]: http://microformats.org/about "About Microformats"
  [7]: http://en.wikipedia.org/wiki/Law_of_Demeter "Law of Demeter"
  [8]: http://lesscss.org "LESS « The Dynamic Stylesheet language"
  [9]: http://sass-lang.com "Sass - Syntactically Awesome Stylesheets"
  [10]: http://sass-lang.com/documentation/file.SASS_REFERENCE.html#mixins "Sass Documentation — Mixin Directives"
  [11]: http://sass-lang.com/documentation/file.SASS_REFERENCE.html#placeholders "Sass Documentation — @extend-only Selectors"
  [12]: http://sass-lang.com/docs/yardoc/file.SASS_REFERENCE.html "SASS Reference"
  [13]: http://lists.w3.org/Archives/Public/www-style/2011Mar/0478.html "W3C www-style mailing list: CSS Mixins proposal by Tab Atkins Jr."
  [14]: http://ja.wikipedia.org/wiki/%E9%96%A2%E5%BF%83%E3%81%AE%E5%88%86%E9%9B%A2 "関心の分離"
  [15]: http://calendar.perfplanet.com/2011/css-selector-performance-has-changed-for-the-better/ "CSS Selector Performance has changed! &#40;For the better&#41;"
  [16]: http://csslint.net/about.html "About CSSLint"
  [17]: http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/ "Truth, Equality and JavaScript"
  [18]: http://www.webdirections.org/blog/in-defense-of-the-humble-id-attribute/ "In defense of the humble id attribute"
  [19]: https://github.com/alphagov/govuk_frontend_toolkit "GOV.UK Frontend Toolkit"
  [20]: http://twitter.com/cackhanded "Mark Norman Francis on Twitter"
  [21]: http://twitter.com/intranation "Brad Wright on Twitter"
  [22]: http://twitter.com/rossbruniges "Ross Bruniges on Twitter"
  [23]: http://twitter.com/jaffathecake "Jake Archibald on Twitter"
  [24]: http://twitter.com/ptg "Patrick Griffiths on Twitter"
  [25]: http://martinfowler.com/bliki/TwoHardThings.html
  [26]: http://www.w3.org/TR/html5/dom.html#classes
  [27]: http://tantek.com/2012/353/b1/why-html-classes-css-class-selectors
  [28]: https://twitter.com/alex_gaynor/statuses/389502802265133056
  [29]: http://nicolasgallagher.com/about-html-semantics-front-end-architecture/
  [30]: http://www.onderhond.com/blog/cost-of-performance-css-selector-rewriting
  [31]: http://www.faqs.org/docs/artu/transparencychapter.html
  [32]: http://2002-2012.mattwilcox.net/archive/entry/id/1054/
  [33]: http://en.wikipedia.org/wiki/Form_follows_function
