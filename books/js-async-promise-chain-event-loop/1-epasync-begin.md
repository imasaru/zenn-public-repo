---
title: "はじめに"
cssclass: zenn
date: 2022-04-17
modified: 2022-11-02
AutoNoteMover: disable
tags: [" #type/zenn/book  #JavaScript/async "]
aliases: ch_はじめに
---

# このチャプターについて

このチャプターでは、この本を読むに当たっての注意点や本の内容についての説明を記載しています。具体的な解説を読み始める前に必ず確認するようにしてください。

# この本について

この本を書き始める以前に、記事として『[非同期処理を理解できるようになるまでので学習ロードマップ](https://zenn.dev/estra/articles/js-async-programming-roadmap)』を執筆しました。その結果、大きな反響があったため「学習のロードマップは分かったけど、より具体的な非同期処理のポイントも知りたい」といった一歩進んだ要望に応えようとこの本を書きました。

この本では、プロミスチェーン(Promise chain)とイベントループ(Event loop)という大きく２つのポイントから非同期処理について解説しています。学習経験から重要な情報に沿ってロードマップを再構成した作りとなっているので内容重視の解説順番となります。

執筆目標としては「読者が非同期処理の制御の流れを理解し、予測できるようになる」ことを掲げています。また、**学習の過程で陥りがちなトラップや誤解**などについても実際の学習から得た知見で解説しているので、「非同期処理について学習してみたけど、やっぱり分からない」という方の誤解や勘違いを解きつつ「非同期処理の謎を解明する」ということも目標としています。

:::message
**なぜ今更プロミスチェーンなのか？**

非同期処理の解説であるのに「async/await」ではなく「プロミスチェーン」をあえてタイトルに入れているのは意味があります。というのも、直感に反して**非同期処理の主役は Promise であり**、非同期処理の動きを理解するための本質的な部分が **イベントループ上でのタスク・マイクロタスクの連鎖的な処理(chain)** であるからです。async/await で扱う Promise の性質を知るため、マイクロタスクが連鎖的に処理されることを理解するために、プロミスチェーン(Promise chain)が必要となります。

Promise について詳しくなることで非同期処理を柔軟に書けるようにもなります。急がば回れということで Promise をじっくりと解説していきます。
:::

今回あえて Book(本) として公開している理由の一つとしては、かなり長い内容になるので記事一本では書ききれないと感じたためです。実際、アウトプット用の記事として書いたものが予想以上に長くなってしまったため、本へと再構成しました。

本としての体裁は取っていますが、筆者自身のアウトプットを大きく兼ねているため、この本は無料公開となっています。

この本では可能な限り正確な情報になるように努力していますが、学習者のアウトプットとしての側面も強いので、間違いや勘違い等があるかもしれません。その点についてはご容赦ください。なにか不備や間違いを見つけた場合には[スクラップ](https://zenn.dev/estra/scraps/20dc6c4a1b64f8)や Github の issue などにて教えていただけると非常に助かります。

# 対象読者について

想定している対象読者は「**非同期処理の理解に悪戦苦闘している人**」です。

従って、本当にゼロから非同期処理を学習したいという人にはこの本は難しすぎるかもしれません。「非同期処理について一度学んでみたが分からない」という人や「どうしてこのような実行順序になるのか分からない」という人向けの内容となります。

とはいえ、非同期処理を学習する上でトラップとなる点や深堀りすべき点については徹底的に記載しているので、ゼロから学習したい場合には他のリソースなどの併用を推奨しています。

# 構成について

この本は大きく分けて４つの章から構成されています。

- [第１章 - API を提供する環境と実行メカニズム](sec-01-epasync)
- [第２章 - Promise インスタンスと連鎖](sec-02-epasync)
- [第３章 - async 関数と await 式の挙動](sec-03-epasync)
- [第４章 - 制御と型注釈](sec-04-epasync)

第１章の内容はメタ的な視点での解説を含んでいるため、部分的に第２章や第３章の内容である Promise や async/await についての知識を利用している場合があります。難しい場合には、ざっと目を通して具体的な Promise の解説である第２章の内容を読み進めるのが良いかもしれません。

第１章から第３章までの具体的な流れとしては、非同期 API とそれを提供する環境についての話から始め、イベントループへの理解を深めます。具体的な非同期処理については、Promise の基礎的概念から Promise chain へと続き、そこまでのすべての知識と V8 エンジンからのアプローチによって async/await の挙動を理解します。

第４章では応用として Promise の静的メソッドと await 式の配置による複数処理や反復処理の制御方法を学び、最終的には TypeScript での型注釈を行えるようにします。

第４章の後には非同期処理のテーマに関しての[まとめと総括のチャプター](y-epasync-conclusion)を設けているので、そちらを見つつ他のチャプターの学習をすすめていくのもよいでしょう。

:::message alert
リアルタイムで学習した内容を反映させたり、勘違いしていた内容についての大規模な修正を追加したこともあり、以前は古い知見に基づいたチャプターと更新された新しい知見に基づいたチャプターが入り混じった構成になっていましたが、現時点ではすべてのチャプターを新しい知見に基づく解説へと統一しました。

例外的に『[イベントループの概要と注意点](2-epasync-event-loop)』のチャプターだけはそのまま古い内容とそれについての訂正・注釈を残しています(分かりやすいように修正なども施しています)。いくつかの誤解や間違いを直すための試行錯誤の過程が見て取れるので「そういう資料」であると思って読み進めてください。
:::

内容としての充足度は 100% で、当初予定していた内容についてのチャプターはすべて収録済みとなり、最新の ECMAScript のシンタックスと TypeScript までを含めたモダンな解説になっています。また、筆者自身の知識が更新される限り内容を追加して最新に保つつもりです。

# 参考文献について

各チャプターのいたるところで参考となる文献や参照したリソースの URL を貼っておきます。また、[最終チャプター](z-epasync-reference)にて各チャプターのリソースの URL をまとめていますので気になった場合はそちらも参照してください。

もちろん、この本１つで非同期処理を完全に理解することは難しいので、それらを含めたインターネット上のあらゆる他のリソースを併用することを推奨しています(非同期処理の理解にはかなり多くの情報が必要なため、**１つのリソースだけでは情報が不足しすぎています**)。特に、この本で扱う領域は非同期処理の制御を予測するための基礎的な事象、あるいは本質的な事象を中心としています。より実践的な話については他のリソースが必要となります。

とは言え、非同期処理の解説等であまり語られていない、環境や V8 エンジン、マイクロタスク、実行コンテキストなどの概念が登場しますので読んで損がないようにしています。これによって非同期処理だけでなく、付随的・必然的に JavaScript の実行メカニズムの基礎についても理解できるようになります。

参考になるドキュメントでは azu さんの『JavaScript Primer』が非常におすすめです。非同期処理の基礎やシンタックスについて網羅的に学ぶことができます。

https://jsprimer.net/basic/async/

もちろん、MDN のドキュメントも必須です。何か細かいことを知りたくなったら、MDN のドキュメントを読んでください。ただし、場合によっては英語版も読む必要があります[^mdnについて]。

  [^mdnについて]: MDN のドキュメントは未翻訳の箇所に重要なことが書かれていたり、翻訳時点でのドキュメントよりも分かりやすい最新版などが確認できる場合がたまにあるため、両方確認した方がいいです。この本でも英語版ドキュメントからの引用がいくつかあります。

https://developer.mozilla.org/ja/

動画では基本的に JSConf[^JSConfについて] にアップロードされているものがオススメです。実はドキュメントよりも動画を使って視覚的に理解した方がいい場合もよくあるので、ぜひ活用してください。JSConf の動画は本質情報を得られる場合が多いです。この本の中ではも JSConf で行われた『[What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)』などのいくつかの動画を参照しています。

https://www.youtube.com/c/JSConfEU/videos?view=0&sort=p&shelf_id=0

  [^JSConfについて]: Conf 系(NodeConf, ReactConf など)はオーソリティ性が確保されているのである程度安心してみることができます。基本的には最新のものが良いですが、JSConfEU の動画ではよく視聴されているものを見ておくと良いです。動画は英語ですが、平易な英語なので字幕などを活用して視聴してください。

この本で主に使う JavaScript の実行環境である Deno については uki00a さんの『Effective Deno』が分かりやすいので参考にすると良いと思います。

https://zenn.dev/uki00a/books/effective-deno

「そもそも Deno とか Node とかって何なの？」という疑問がある場合には non_cal さんの以下の記事などが分かりやすいので参考にしてください。

https://qiita.com/non_cal/items/a8fee0b7ad96e67713eb

これら以外で細かい学習リソースに困ったら、『[非同期処理を理解できるようになるまでので学習ロードマップ](https://zenn.dev/estra/articles/js-async-programming-roadmap)』の記事に挙げたリソースなどを参考にしてもらえばいいかと思います。

# スクラップとリポジトリについて

この本に関する感想や質問等あれば、紐付けられたスクラップの方へお願いします。間違いや誤植等を見つけた場合にも教えていただけるとありがたいです。

https://zenn.dev/estra/scraps/20dc6c4a1b64f8

また先日、今まで使用していた Zenn のリポジトリを無料公開用のリポジトリとして Public にしました。以下のリポジトリでタイポや内容修正などのプルリクエストを受け付けています。

https://github.com/yo-goto/zenn-public-repo

Book の各チャプターの右下にも「GitHub で編集を提案」のボタンがあるので、ミスや間違い、タイポなどを見つけた場合にはプルリクエストを作成していただけると助かります。

![GitHubで編集を提案](/images/js-async/img_githubSuggestion.jpg)

# 表記について

イベントループで Promise chain がどのように動くか "[JavaScript Visualizer 9000](https://www.jsv9000.app) "というツールをこの本の各所で使用していきます。名前が長いので、以降「JS Visualizer」などと呼称するようにします。

また、関数の引数に渡すコールバック関数について `then(cb)` や `then(callback)` のように `cb` や `callback` と省略表記する場合があるので注意してください。

ECMAScript や HTML 仕様、Node.js などで同じもの(似たもの)を表す場合に用語が違ったりする場合がありますが、混乱を避けるために次の用語のみを意図的に使用するようにします。また、英単語を記載する際には大文字から始めるようにします。

- イベントループ(Event loop)
- タスク(Task)[^TaskとMacrotaskについて]
- マイクロタスク(Microtask)

  [^TaskとMacrotaskについて]: 最初はタスク(Task)よりもマクロタスク(Macrotask)の単語の方が理解しやすいかと考えていたのですが、タスク(Task)の方が本質を捉えやすい言葉だと感じたのでこちらをなるべく使っていきたいと思います。加えて Macro と Micro は似ているので見間違いやすいという理由もあります。

# 使用する環境とJSエンジン

この記事では実際のコードを通して、Promise chain がどのような制御で実行されるかを確認していきます。この本を読む際にはぜひ自分で実行してみるようにしてください。

JavaScript と一言で言っても様々な環境で使用される言語ですから、どういった環境で使用するかを考える必要があります。

- Chrome や Safari などのブラウザ環境
- Node や Deno といったランタイム環境

筆者は Deno がお気に入りなので、この本の説明で使用する JavaScript の実行は主に [Deno 環境](https://deno.land)で行っていきます。

macOS であれば、Deno は Homebrew などでコマンドラインから簡単にインストールできます。他の OS でのインストール方法は[公式マニュアル](https://deno.land/manual/getting_started/installation)を参照してください。

```sh
# homebrew でインストール
❯ brew install deno
```

使用する Deno のバージョンは以下のものとなります。

```sh
❯ deno -V
deno 1.20.4
```

https://deno.land/x/deno@v1.20.4

Deno では設定無しで TypeScript をすぐ実行できるという利点があります。そのためこの本で Deno を利用しているのは、**最終的に TypeScript で Promise の型を考える**ために使っているという側面もあります。Deno を使うことで JavaScript に型情報の操作を加えた TypeScript へとスムーズに移行していくことができます[^DenoのTS]。という訳で、この本の[第４章最終チャプター](j-epasync-ts-promise-type-annotation)では JavaScript から TypeScript へ移行する方法と Promise の型注釈についても解説します。

  [^DenoのTS]: Deno は V8 エンジンでの JavaScript をベースにしつつ、型の世界(TypeScript)へと簡単に立ち入ることができる環境であり、段階的に「型情報の操作」に関する学習を進めることができます。また、node で利用する [ts-node](https://typestrong.org/ts-node/) などの追加パッケージが必要なく TypeScript ファイルをコマンドラインから実行できます。

サーバーを立てたり HTML ファイルなどを噛ませず、以下の様に JS ファイルを用意してターミナルから `deno run` コマンドを使ってローカル環境で JavaScript を実行していきます(`node` コマンドでもいいです)。これが JavaScript の非同期処理をテストするための最も早い方法です。

```js:helloWorld.js
console.log("hello world!");
```

```sh:コマンドライン
# deno run コマンドでスクリプトファイルを実行
❯ deno run helloWorld.js
hello world!
# node コマンドでスクリプトファイルを実行
❯ node helloWorld.js
hello world!
```

小さいスニペットをコマンドライン実行してテストするというこの方法を筆者は「[コマンドライン JavaScript](x-epasync-epilogue#コマンドラインから始める-javascript)」と呼んでいます。ちなみに、この方法に利用できる環境は MDN では「JavaScript Shell」と呼ばれています。

> A JavaScript shell allows you to quickly test snippets of JavaScript code without having to reload a web page. They are extremely useful for developing and debugging code.
> ([JavaScript technologies overview - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/JavaScript_technologies_overview#shells) より引用)

後のチャプターで詳しく解説しますが、Chrome ブラウザ環境や Node, Deno といったランタイム環境において**イベントループの本質的な部分(抽象的な動作メカニズム)は共通しています**。とはいえ環境による違いもあるので、それについても解説しておきました。つまりこの本では、代表的な複数の環境において JavaScript の非同期処理を包括的に考えます。

というわけで、Node 環境についても考える場面があるので `node` コマンドを使用している際には次のバージョンで考えください。

```sh
❯ node -v
v18.2.0
```

https://nodejs.org/dist/v18.2.0/docs/api/

そして、そういった特定の環境に拘らず考えるため、途中から V8 エンジンを使用します。この V8 エンジンについても解説しますが、V8 エンジンは Chrome, Node, Deno, Electron などで使用されている JavaScript エンジン(ECMAScript を実装しているもの)です。

この本で使用する V8 エンジンのバージョンは次のものとなります。

```sh
❯ v8
V8 version 10.3.154
```

# JS Visualizer 使用上の注意点

:::message alert
こちらの内容については今理解できなくても大丈夫です。疑問を抱いた時に確認してください。
:::

JS Visuzalizer では使用する上でいくつか注意点があります。

https://www.jsv9000.app

まずは、日本語が使えませんので、日本語を入力したり、日本語が含まれるコードをペーストしようとすると画面が白くなり固まります。

非同期処理を理解する上で初期は非常に有用ですが、使用して行くうちにいくつかの疑問点が湧いてくるはずです。筆者自身は非同期処理を理解していく内に、この違和感に気づきました。

JS Visualizer では実装ミスとも言える点や勘違いしやすい点が実はいくつかあります。

- 環境での並列的作業としての `setTimeout()` 関数などの Web API の概念が掴みづらく、タスクキューにタスクが挿入される順番に間違いと思われる部分があります
- イベントループには多くの場合、タスクキューが１つ以上あるため、タスクキューが１つであると勘違いする可能性が高いです
- コールスタックに積まれるコンテキストとして必要なグローバルコンテキストが視覚化されていないため、最初のタスクやマイクロタスクの実行順番について誤解する可能性があります

そのため、この本でも途中から(新しい知見に基づいたチャプターでは)あまり使用しなくなりますが、イベントループのイメージを掴む上で非常に有用であることは変わりませんので注意して利用してください。

# ChangeLog

大きな変更のみトラッキングしています。

:::details ChangeLog
- 2022-11-05
  - 『[タスクキューとマイクロタスクキュー](d-epasync-task-microtask-queues)』のタスクキューについての解説に間違いがあったため修正
- 2022-08-15
  - ブランチに纏わるあれこれの設定を変更し、リポジトリを Public として公開
- 2022-07-21
  - 新チャプター『[総括 - 非同期処理のまとめ](y-epasync-conclusion)』を追加
- 2022-07-18
  - 新チャプター『[イテレータとイテラブルとジェネレータ関数](k-epasync-iterator-generator)』を追加
- 2022-07-12
  - 新チャプター『[TypeScript における Promise の型注釈](j-epasync-ts-promise-type-annotation)』を追加
- 2022-06-30
  - 新チャプター『[await 式の配置による制御](18-epasync-await-position)』を追加
  - 新チャプター『[反復処理の制御](19-epasync-async-loop)』を追加
  - 新チャプター『[参考文献](z-epasync-reference)』を追加
- 2022-06-26
  - 新チャプター『[Promise の静的メソッドと並列化](17-epasync-static-method)』を追加
- 2022-06-16
  - 章分けのためのチャプターを追加
- 2022-06-15
  - 新チャプター『[同期 API とブロッキング](f-epasync-synchronus-apis)』を追加
- 2022-06-06
  - すべてのチャプターの説明を新しい知見に基づく内容へと書き換え完了
  - 新チャプター『[あとがき](x-epasync-epilogue)』を追加
- 2022-05-28
  - 新チャプター『[Top-level await](16-epasync-top-level-async)』を追加
- 2022-05-22
  - いくつかチャプターでの説明を加筆
- 2022-05-21
  - 全チャプターのフォーマットを統一
- 2022-05-14
  - 全体を完成
  - 新チャプター追加
    - 『[catch メソッドと finally メソッド](h-epasync-catch-finally)』を追加
    - 『[Promise chain から async 関数へ](14-epasync-chain-to-async-await)』を追加
    - 『[V8 エンジンによる async/await の内部変換](15-epasync-v8-converting)』を追加
- 2022-05-06
  - 新チャプター追加
    - 『[コールスタックと実行コンテキスト](b-epasync-callstack-execution-context)』を追加
    - 『[それぞれのイベントループ](c-epasync-what-event-loop)』を追加
    - 『[タスクキューとマイクロタスクキュー](d-epasync-task-microtask-queues)』を追加
    - 『[V8 エンジン](e-epasync-v8-engine)』を追加
    - 『[非同期 API と環境](f-epasync-asynchronous-apis)』を追加
    - 『[resolve 関数と reject 関数の使い方](g-epasync-resolve-reject)』を追加
  - 『[古い非同期 API を Promise でラップする](12-epasync-wrapping-macrotask)』を改修
  - 『[イベントループは内部にネストしたループがある](9-epasync-dont-nest-promise-chain)』を改修
- 2022-05-03
  - 『[コールバックで副作用となる非同期処理](10-epasync-dont-use-side-effect)』に「副作用とは」の項目を追加
  - 新チャプター『[Promise の基本概念](a-epasync-promise-basic-concept)』を追加
- 2022-04-23
  - 『[コールバックで副作用となる非同期処理](10-epasync-dont-use-side-effect)』を大幅追記
- 2022-04-21
  - JavaScript Visuzalizer で共有したコードが文字化けしていたので修正
  - 『[Event loop の概要と注意点](2-epasync-event-loop)』を大幅に修正追記
    - それに伴い Event loop のステップについて各所を修正
  - 新チャプター追加
    - 『[Promise コンストラクタと Executor 関数](3-epasync-promise-constructor-executor-func)』で関数式とアロー関数の補足を追加
  - 『[コールバック関数の同期実行と非同期実行](4-epasync-callback-is-sync-or-async)』で「コールバック関数はいつ実行される?」の項目を追加
  - 『[then メソッドは常に新しい Promise を返す](6-epasync-then-always-return-new-promise)』で「Promise の状態を確かめる」の項目を追加
  - 『[Promise chain で値を繋ぐ](7-epasync-pass-value-to-the-next-chain)』で「チェーンの最後まで値を繋ぐ」の項目を追加
  - 『[Promise chain はネストさせない](9-epasync-dont-nest-promise-chain)』で「fetch の例」の項目を追加
  - 『[Event loop は内部にネストしたループがある](13-epasync-loop-is-nested)』を追加
  - その他全体的に加筆修正
  - コードの表記を修正
:::
