---
layout: post.ja
title: APIドキュメント化始めました (2013/8/12に完了しました)
description: APIドキュメント化始めました
---

APIドキュメント化始めました (2013/8/12に完了しました)
-----------------------------------------------------

> *2013-08-12更新* APIドキュメント化作業は
> [whombx](https://github.com/whombx) さんのご協力により目標としていた作業を完成させることができました。pull requestをたくさん送ってくれた [whombx](https://github.com/whombx)
> さんのおかげです。とても助かりました。

長い間懸案であった、groongaのAPIをドキュメント化する作業を始めました。

始めた理由は二つあります。

一つはAPIのドキュメントがないので、groongaをライブラリとして使おうとしたときにはソースを直接参照しないといけないためです。

もう一つの理由は、Visual
Studioでビルドするときに大量の警告メッセージが出力されるのを解消したい、というのがあります。現在mroongaをMariaDBにバンドルできるようにする作業を進めているのですが、この状態でバンドルしてもらうのは難しいです。groongaのヘッダに日本語で説明を書いているせいなので、ヘッダから日本語を追い出すことで問題を解消したいなぁというのがあります。

上記からドキュメント化の作業を始めたわけですが、現状リソースがまったく足りていません。

そこで、ユーザのみなさんにも協力していただけたらいいなと思っています。MariaDBへのバンドルを応援したいという人はドキュメント化作業に参加してくれると嬉しいです。

では具体的にどんなことをやればいいのかを説明します。基本的には関数ごとにpull
requestを送ってもらうと進めやすいです。

あまりGitHubでの作業に慣れていなくてもできるように、「最初にやること」と「作業ごとにやること」、「関数ごとにやること」に分けて順に説明します。

-   最初にやること
-   作業ごとにやること
-   関数ごとにやること

### 最初にやること

以下では、最初に一度だけ実施しておけば良いことを説明します。

#### 最初の設定

まずは、gitの設定をしましょう。すでにある程度gitを使っている場合には初期設定はすでに完了しているかも知れません。その場合には飛ばして構いません。

    % git config --global user.name "あなたのユーザー名"
    % git config --global user.email "あなたのメールアドレス"

上記はコミットログに使われます。公開しても差し支えないユーザ名もしくはメールアドレスを設定します。

#### GitHubでforkする

GitHubにアカウントがなければ用意してください。アカウントが用意できたら、ログインした状態で以下のURLにアクセスします。

-   [groongaリポジトリをforkする](https://github.com/groonga/groonga/fork)

リポジトリ選択画面でご自分のリポジトリへとforkしてください。

#### 作業用リポジトリの初期設定

次は作業用にforkしたリポジトリを手元にcloneします。このとき、本家の変更に追従するための設定を行います。

    % git clone git@github.com:(あなたのGitHubのアカウント)/groonga.git
    % cd groonga
    % git remote add upstream git@github.com:groonga/groonga

#### ドキュメントビルド用の初期設定

ドキュメントを生成するために以下を実行します。

    % ./autogen.sh
    % ./configure --enable-document

ここまでで、「最初にやること」は完了です。次は「作業ごとにやること」へと進みます。

### 作業ごとにやること

以下では作業ごとにやることを説明します。

#### groonga本家に追従する

groonga本家の最新状態に追従して、作業がかぶらないようにします。

    % git fetch --all
    % git checkout master
    % git rebase upstream/master

最新の状態に追従できたら、「関数ごとにやること」へと進みます。

### 関数ごとにやること

以下では、例えば `grn_ctx_open()` の作業をする場合で説明します。

#### 作業用のブランチを作成

作業対象のブランチを作成します。

    % git checkout -b grn-ctx-open

#### 編集作業

`include/groonga.h` から `doc/source/reference/api/grn_ctx.txt`
にドキュメントを移動します。

中には英訳されていないものもありますが、英訳はしなくて構いません。
移動先のドキュメントのフォーマット化を機械的に行っていただければOKです!

#### ドキュメントの確認

マークアップに問題がないか、HTMLを確認します。HTMLを生成するには以下のコマンドを実行します。

    % cd doc/locale/en
    % make html

いつも使っているブラウザで該当ファイルを確認して、移動した内容が追加されていればOKです。

    % firefox html/reference/api/grn_ctx.html

#### コミット

HTMLに問題がないことを確認できたら、コミットします。

    % cd ${cloneしたディレクトリーのトップディレクトリー}
    % git add include/groonga.h
    % git add doc/source/reference/api/grn_ctx.txt
    % git commit

コミットするときのメッセージについては、例えば以下のようにします。

    doc: move grn_ctx_open() document to Sphinx text from header file

#### pushとpull request

ご自分のリポジトリにpushして変更点をとりこめるように公開します。

    % git push -u origin grn-ctx-open

ここで `grn-ctx-open` は前の方の作業で作ったブランチ名です。

ブラウザで https://github.com/(GitHubのアカウント)/groonga を開くと「
`grn-ctx-open` 」ブランチをpull
requestする！みたいなUIができているので、そこのボタンを押してpull
requestしてください。入力フォームがでてきますが、コミットしたときメッセージで十分なのでそのままpull
requestしてOKです！

これで、ひととおりの作業は完了しました。

まだまだできるよ!という人は
「作業ごとにやること」の「groonga本家に追従」に戻って繰り返します。
