---
title: 静的サイトジェネレータとGitHub Pagesを利用したブログ作成
description: 
slug: hugo_instruction
date: 2022-12-31
categories: ["Hugo","静的サイトジェネレータ","GitHub Pages"]

draft: false
---

最初の記事投稿でサイバーセキュリティ以外の投稿になりますが、このブログの環境づくりで色々手間取ったため、まとめたいと思います。

## 概要
以下のプロダクトとサービスを利用してこのブログを作成しています。それらを選んだ理由とその構築方法（メモ）を以下にまとめます。
> * 静的サイトジェネレータ：[Hugo](https://gohugo.io) v0.107.0
> * テーマ：[Hugo Theme Stack](https://github.com/CaiJimmy/hugo-theme-stack)
> * Git
> * GitHub Pages


## 他のCMSを利用しない理由
以下の理由で私は、『静的サイトジェネレータ』『GitHub Pages』でブログを作成しようと思いました。
私もググって調べて比較した限りなので、本当はもっと良い方法もあったかもしれません。あくまでご参考までに。

#### 維持費用が安く済む

他のCMSで有名なものとして、WordPressがあると思います。これで公開ブログを作ろうとすると、LAMP（Linux、Apache、MySQL、PHP）の環境を準備する必要があります。

この環境のためのレンタルサーバやVPSをさくらインターネット等で準備する必要があり、維持費用がかかります。（とはいっても、スペックの低い環境であれば、毎月数百円で済むことも多いです。）

静的サイトジェネレータとGitHub Pagesだと維持費用がかからないため、個人ブログとしては魅力的でした。（数百円でも毎月かかると結構違います）

#### セキュリティ対策を考えなくて済む

WordPressは、ITのスキルなしにブログ作成できるところはメリットですが、深刻なセキュリティ脆弱性が頻繁にでています。そのため、頻繁に脆弱性情報をウォッチし、脆弱性に該当する場合はアップデートするなどの対応が必要になります。

また他のLAMPのプロダクトについても同様です。WordPressのような動的サイトだと、セキュリティ脆弱性が生まれやすいです。
（そうしないと、準備した環境が改ざんされたり、乗っ取られたりします。実際私もVPSを借りているのですが、バックドアプログラムを仕込まれ、外部から操作されたことがありました。。）

静的サイトジェネレータで作ったサイトだと、静的コンテンツになるためセキュリティ脆弱性のリスクは低減できています。またGitHub Pagesもアップデートの管理をGitHubに委託できるため、個人として対応することは不要になります。

セキュリティに関するブログ発信をしていて、改ざんされると情けないので、この理由が一番大きい気がします。

#### 流行しているスキルを身に着けるため

上記理由だけであれば、『はてなブログ』で作る方法もあるかと思います。その方法も考えたのですが、ITスキルが身につかないと考えたため、採用を見送りました。

『静的サイトジェネレータ』でサイトを作るためには、最低限どのような仕組みで動いているかを理解する必要があります。また、GitHub PagesもGitの初歩的な知識が必要になります。
これらの仕組みや知識を理解・習得するために、はてなブログはやめました。


## 静的サイトジェネレータとは？
CLOUDFLAREのサイトを見ると『生（未加工）データとテンプレート群を基に、完全に静的なHTML Webサイトを生成するツールのことです。』と紹介されています。

テンプレートの中に設定ファイルやデータが含まれており、それらを修正します。その後、ツールを実行（ビルド処理）することにより、静的なHTMLコンテンツを自動生成してくれます。

このツールはいくつか紹介されており、代表的なものに以下があります。
* NEXT.js
* Hugo
* Gatsby
* Jekyll

なお、以下のサイトで静的サイトの人気ランキングありました。
https://jamstack.org/generators/

Github Starが多いものが人気があると考え、上位でかつ（私があまり知識のない）JSの知識が不要と考えられるHugoを私は選択しました。


## GitHub Pagesとは？
GitHubが提供している静的サイトのホスティングサービスです。レンタルサーバでApacheなどのWEBサービスか稼働している環境を準備することなく、WEBサイトを公開することができます。

なお独自ドメインを利用したり、gooogleアドセンスのような広告を張ることも可能とのことです。（ただこのサイトはまだ対応していません。）


## 環境構築の流れ
以下にそれぞれの流れを簡単にまとめます。つまづいたところはなるべく丁寧に説明するつもりです。

### Gitをインストール
詳細に説明しているサイトは、いくつもあるのでここでは割愛します。

私は以下のサイトをもとに自分のローカル環境（Windows 11）にインストールしました。

https://www.curict.com/item/60/60bfe0e.html


### Hugoをインストール

こちらも私は以下のサイトを参考にインストールをしました。
https://dev-labo.com/windows/start-up-hugo/

特段つまづかなかったのですが、以下に簡単に手順をまとめます。
1. [Hugoのページ](https://github.com/gohugoio/hugo/releases)からローカル環境にあわせたZIPファイルをダウンロード
2. ZIPファイルを解凍
3. Hugoのexeファイルを配置する場所を作成し、解凍ファイルをその場所に移動
4. 配置したファイル（exeファイル）に対して環境変数のPATHを通す
5. コマンドプロンプトで以下のコマンドを実行し、ヘルプ一覧がでてきたら実行成功
```
hugo help
```

### サイトのドキュメントルートを作成
Apacheでいうドキュメントルートを作成します。以下のサイトを参考にしました。

https://miiitomi.github.io/p/hugo/

こちらも特段つまづかなかったのですが、以下に簡単に手順をまとめます。
1. コンテンツを配置するディレクトリを作成する。今回は例として以下とする。
```
E:\Blog
```
2. ブログを入れるディレクトリ名（任意）を決める。今回は例として```MyBlog```とする。
3. コマンドプロンプトで『1.』で作成したディレクトリに移動して、hugoでディレクトリを作成する。コマンドと出力例は以下の通り。
```
E:\Blog> cd E:\Blog
E:\Blog> hugo new site MySite
Congratulations! Your new Hugo site is created in E:\Blog\MySite.

Just a few more steps and you're ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```
4. ```MyBlog```の中に初期のデータが入っていればOK

### テーマを選択する
[Hugoの公式ホームページ](https://themes.gohugo.io/)でテーマが公開されています。この中で自分の好みに合ったものを選ぶとよいと思います。
私は[Stack](https://github.com/CaiJimmy/hugo-theme-stack)を選びました。

なお[こちらのサイト](https://automation-knowledge.com/hugo-theme-ranking/)では、GitHub Starsに基づいてランキングを集計して公開しているようです。
よく使われるサイトが後半の設定を変更するためにも有益だと思うので、スターが多いものも選択肢の候補になると思います。

### テーマを配置する
ここで色々なサイトで色々やり方が紹介されています。この方法が本来として正しいかは別ですが、私はテーマをZIPでダウンロードして以下のフォルダに配置しました。
```
E:\Blog\MySite\themes\
```

```git clone```する方法を紹介しているものもありますが、あとでこのテーマの設定を一部変更します。
そうするとgitでプッシュしてGitHub PagesでDeployできなくなるため、ZIPでダウンロードする方法としました。
（このあたりは素人なのですが、テーマは別のレポジトリで管理されており、その設定を変更したことにより不整合がおきてエラーが出ると考えています。）
私は以下のエラーがでてつまづきました。その後ZIPでダウンロードしてやるやり方に変更してできるようになりました。
```text
Error : fatal: No url found for submodule path ‘themes/hugo-theme-stack' in .gitmodules
```
サブモジュールのパスが登録されていないため、元のテーマがアップデートされたときに同期をとることが難しくなると思っていますが、
それはまた必要にお応じて是正していければと思っています…。

上記ディレクトリにテーマを配置できたら、サンプルサイトを起動できるか確認します。
以下のファイルとディレクトリを、
```
E:\Blog\MySite\themes\hugo-theme-stack\exampleSite\config.yaml
E:\Blog\MySite\themes\hugo-theme-stack\exampleSite\content
```
以下のディレクトリにコピーします。（```content```ディレクトリは置き換えになります。）
```
E:\Blog\MySite\
```
また、以下の設定ファイルはコピーした設定ファイル（```config.yaml```)を利用したいため削除します。
```
E:\Blog\MySite\config.toml
```
上記が完了したら、コマンドプロンプトで以下のコマンドを実行します。出力例は以下の通りです。
```
E:\Blog\MySite>cd E:\Blog\MySite
E:\Blog\MySite>hugo server -D -F
Start building sites …
hugo v0.105.0-0e3b42b4a9bdeb4d866210819fc6ddcf51582ffa+extended windows/amd64 BuildDate=2022-10-28T12:29:05Z VendorInfo=gohugoio
WARN 2022/12/13 10:42:35 Search page not found. Create a page with layout: search.
WARN 2022/12/13 10:42:35 Archives page not found. Create a page with layout: archives.
WARN 2022/12/13 10:42:35 Search page not found. Create a page with layout: search.
WARN 2022/12/13 10:42:35 Archives page not found. Create a page with layout: archives.

                   | EN | ZH-CN | AR
-------------------+----+-------+-----
  Pages            | 47 |    17 | 20
  Paginator pages  |  1 |     0 |  0
  Non-page files   |  5 |     4 |  1
  Static files     |  0 |     0 |  0
  Processed images | 24 |    13 |  3
  Aliases          | 23 |     7 |  9
  Sitemaps         |  2 |     1 |  1
  Cleaned          |  0 |     0 |  0

Built in 1123 ms
Watching for changes in E:\Blog\MySite\{archetypes,content,data,layouts,static,themes}
Watching for config changes in E:\Blog\MySite\config.yaml, E:\Blog\MySite\themes\hugo-theme-stack\config.yaml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
上記でローカルのWEBサービスが立ち上がったため、出力の通りhttp://localhost:1313/にブラウザでアクセスし、サンプルサイトが閲覧できれば稼働できています。

![](images/ExampleSiteTop.png)

### 設定を変更する
以下のサイトを参考に設定を変更しました。
https://miiitomi.github.io/p/hugo/

なお大きく日本語フォントに関する設定と全般の設定に分かれているため、それぞれまとめます。
#### 日本語フォント設定
元テーマの設定ファイルの変更が必要になります。以下のパスのファイルを開きます。
```
E:\Blog\MySite\themes\hugo-theme-stack\assets\scss\variables.scss
```
以下の中国語フォントの箇所を
```
    --zh-font-family: ...
～～中略～～
    --base-font-family: "Lato", var(--sys-font-family), var(--zh-font-family), sans-serif;
```
以下に変更して中国語フォントではなく日本語フォントになるように変更します。これで漢字が中国フォントになることを防ぐことができます。
```
    --ja-font-family: "游ゴシック体", "Yu Gothic", YuGothic, "ヒラギノ角ゴ Pro", "Hiragino Kaku Gothic Pro", "メイリオ", "Meiryo";
～～中略～～
    --base-font-family: "Lato", var(--sys-font-family), var(--ja-font-family), sans-serif;
```
#### 全般の設定
以下のサイトを参考に設定を変更しました。詳細は別途まとめようと思いますが、引っかかる要素はなかったと思います。
https://miiitomi.github.io/p/hugo/

なお、アバターやfaviconなどの画像は以下に格納します。```images```のディレクトリが無ければ作成します。
```
E:\Blog\MySite\static\images
```
### 記事の作成
以下のサイトのやり方を参考に記事を作成します。
https://miiitomi.github.io/p/hugo/

上記記事にある通り、```hugo server -D -F```でローカル環境でWEBサービスを立ち上げながら、記事やコンテンツの修正をしていきます。
ここで公開できるまで内容を作りこんでいきます。

### GitHub Pagesの設定
一通り公開できるまで内容ができたら、GitHub Pagesを利用するレポジトリを作っていきます。
なおここでは、GitHubのアカウントは既に作ってあるものとします。

最初にコンテンツを管理するレポジトリを作成します。
ここでレポジトリ名ですが、URLに指定したい文字列をレポジトリ名にする必要があります。
なお、GitHub Pagesが提供するドメインをそのまま利用する場合、以下の命名規則とする必要があります。
```
XXXX.github.io
```
例えばドメインをこのサイトのように```shimizu4310.github.io```としたい場合、作成するレポジトリは以下のように指定します。
```
shimizu4310.github.io
```
レポジトリ名にも```.github.io```を入れるようにします。
PublicかPrivateのどちらでも公開はできるようですが、マークダウンの記述の参考として公開もできるため、私はPublicでレポジトリ設定をしています。

レポジトリが作成できたら、ソースを```docs```になるように変更します。以下のように、```Settings```を選択し、```Pages```を選択してブランチをdocsに変更しSaveします。
![](images/GitHubPages2.png)

### レポジトリへPush
上記まで作成できたらレポジトリへプッシュします。
プッシュする前に、ローカル環境でツールを使って静的コンテンツを作成します。コマンドプロンプトでドキュメントルートのディレクトリまで移動し、以下のコマンドでビルドすれば大丈夫です。
```
E:\Blog\MySite> cd E:\Blog\MySite
E:\Blog\MySite> hugo
```
上記を実行し静的ファイルが完成していれば、以下のディレクトリに静的ファイルコンテンツが作成されているはずです。
```
E:\Blog\MySite\docs
```
なお余談ですが、上記ファイルをapache等のWebサービスのあるサーバに設置するとそのままコンテンツを表示させることができます。

ここまでできたら作成したレポジトリにプッシュします。プッシュするコンテンツは以下のパスすべてでdocsのみではありません。
```
E:\Blog\MySite
```

プッシュできたら、GitHub Pagesでコンテンツが表示できるか確認します。
以下のようにたどるとコンテンツが表示できるか確認ができます。
![](images/GitHubPages3.png)
もしくはドメインを直接指定しても確認ができると思います。

### うまくできない場合
うまく表示ができなかったりエラーが出る場合は、なんらかビルドに失敗していると考えられます。
ローカル環境で```hugo server -D -F```でうまく表示ができ、GitHub Pages上で表示できない場合、GitHub Pagesでエラーを起こしている可能性があります。
![](images/GitHubPages4.png)
上記のように、```Actions```を見るとステータスがみえるため、deployが成功しているかを確認すると原因究明につなげられると思います。
（エラーが出ている場合は、エラーで検索すると原因や対応方法がわかるかと思います。）

長くなりましたが、私も手探りしながら構築しました。もっと良い方法などありましたらツイッター等でコメントをください！