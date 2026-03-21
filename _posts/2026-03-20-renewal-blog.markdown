---
layout: post
title:  "ブログをChirpyテーマでリニューアルしました"
date:   2026-03-20 10:00:27 +0900
categories: tech-artile
image:
  path: assets/img/20260320-blog-renrewal.png # Add image post (optional)
tags: [github pages, jekyll] # add tag
description: Chirpyを使いブログをリニューアル
---

このブログに来る人はあまりいないと思いますがｗ、今までより見栄えが良くなったことに驚いたかもしれません。このブログはGitHub Pagesを利用しています。無料で公開できるほか、GitHubの自分のドメインをそのまま利用できるメリットがあります。
Jekyll（ジキル）は、MarkdownやHTMLから静的なWebサイトを生成する「静的サイトジェネレータ」です。GitHub PagesはこのJekyllを公式にサポートしており、作成したファイルをGitHubへプッシュするだけで、GitHub ActionsがWebサイトのビルドと公開を自動で行ってくれます。デフォルトで簡単に設定できるので、僕もこれを使っています。

これまでブログのテーマには `flexible-jekyll` を使っていました。導入の難易度が低いから選んだのですが、記事が増えてきたことで過去の記事を探すことが難しい状態になっていました。なにより検索ボックスがない！このことから、思い切ってブログをリニューアルすることにしました。

今回もAIに協力してもらいました。まずAIに「記事の一覧やカレンダーなど、ブログのお約束コンポーネントがあるテーマをいくつか列挙して」と依頼しました。以下を提示してきました。

**Minimal Mistakes (王道・多機能)**
Jekyllテーマの中で最も人気があり、世界中で使われている「これを選べば間違いない」というテーマです。
特徴:
- サイドバーにプロフィール、カテゴリー、タグ一覧を表示可能。
- 年別・月別のアーカイブページが簡単に作れる。
- 検索機能（全文検索）が標準装備。

[デモ画面](https://mmistakes.github.io/minimal-mistakes/)

**Chirpy (モダン・高機能)**
エンジニアに非常に人気のある、現代的なUIを備えたテーマです。

特徴:
- デフォルトで「ダークモード」に対応。
- サイドバーのナビゲーションが非常に使いやすく、記事一覧が見やすい。
- 数式（LaTeX）やグラフ（Mermaid）の表示も得意。

[デモ画面](https://chirpy.cotes.page/)

**Beautiful Jekyll (シンプルかつ親切)**
「シンプルさは維持したいけれど、ブログとしての機能は欲しい」という方にぴったりです。

特徴:
- 設定が非常に簡単（_config.yml をいじるだけでほぼ完結）。
- 記事一覧が大きなカード形式で表示され、写真が映える。
- モバイルでの読みやすさが抜群。

[デモ画面](https://deanattali.com/beautiful-jekyll/)

デモ画面を見比べて、Chirpyが自分が考えるブログの形に合っていたので、こちらを利用することとしました。

## （１）テンプレートをコピーしてリポジトリを作成

まずは本家[chirpy-starter](https://github.com/cotes2020/chirpy-starter)にアクセスし、「Use this template」 → 「Create a new repository」 をクリックしてひな形をコピーします。このリポジトリの名前を"newblog"としてこの後説明します。
リポジトリを作ったらローカルにgit cloneします。

## （２）GitHub Actionsの設定

GitHub上の newblog リポジトリの Settings > Pages を開きます。
Build and deployment > Source を「Deploy from a branch」から 「GitHub Actions」 に変更します。
これまでのflexible-jekyllはpage-deploy.yamlを自作しなくてもビルドしてくれましたが、Chirpyはリポジトリ内に .github/workflows/pages-deploy.yml を作り、こちらを実行する必要があります。


## （３）config.yml の設定

newblog 内の _config.yml を開き、あなたのブログ情報に書き換えます。

- url: https://[ユーザー名].github.io
- baseurl: /newblog （※リポジトリ名に合わせます）
- title, tagline, description などを入力します。taglineはサブタイトルです
- timezone: Asia/Tokyo に設定します。

## （４）記事と画像の移行（コンテンツの引っ越し）

既存の blog リポジトリからデータをコピーします。
記事の移行: _posts フォルダ内のMarkdownファイルを、新しいリポジトリの _posts へコピーします。
画像の移行: 既存の画像ファイルを assets/img/フォルダを作成したうえで配下にコピーします。

**注意点**

Chirpyでは、記事のアイキャッチ画像を指定するキーは image です。img だとテーマ側が認識できず、一覧画面に画像が表示されません。

これまでは以下のように書いていたmarkdownの先頭の情報を

```
img: textbook-of-mail-technology.png 
```

以下のように直す必要がありました。

```
image:
  path: assets/img/textbook-of-mail-technology.png 
```

**faviconの設定**

ブログのアイコンを設定するにはassets/img/favicons/フォルダを作成し、favicon.icoを配置する必要があります。

## （５）about.mdの記述

これまでなかったファイル_tabs/about.mdにプロフィールを書きます。上部は変更不要です。


```
---
# _tabs/about.md
layout: page
title: About
icon: fas fa-info-circle
order: 4
---

インターネット回線のデリバリからデータセンターでのオペレータ、システムエンジニアなどなんでもやってきて、現在はセキュリティ担当かつクラウドエンジニアかつ情シスをやっています。
```


## （６）ビルドと動作確認

変更を git add, git commit, git push origin main します。
GitHub上の Actions タブを開き、ワークフローが動いているのを見守ります。
完了後、https://[ユーザー名].github.io/newblog/ にアクセスして表示を確認します。

### 詰まった点
ChirpyのGitHub Actionsはビルドの後Test Siteというアクションがあり、コードの非推奨なところをエラーを出してくれます。しかしこのテスト、非常に厳密で「ビルドは出来ているのにテストが通らない」ということが起きがちでした。

- 「フロントマターの image: path: から先頭のスラッシュ（/）を削除しないとパスがおかしくなるよ」
- _config.ymlの「social_preview_image」が設定されていないよ（実際には設定されていてもいなくても動作はする）
- 「ファイルパスに変な文字コードが入っていて認識できないよ」

などなど、細かいです。逆に言うとflexible-jekyllはそのあたり適当でも動きましたw
特にタグは大文字小文字を厳密に解釈するためエラーが出ますが、この辺り前のブログは適当にやっていました。どの記事に大文字小文字が混ざったタグが使われているのだろう…と最初一つ一つ目視するしかない、と思ったのですが

**Gemini使ってブログ構築しているのだから、文字列置換もAIにやってもらえばいいじゃん！**

と気が付き、VSCodeにGemini Code Assistをインストール。私生活でもAIコーディングデビューですw

```
@workspace _posts フォルダ内にあるすべてのMarkdownファイルをチェックしてください。tags: に指定されている AWS, Security, ZeroTrust, AI などのタグが、大文字小文字混ざって存在しています。これらをすべて小文字（aws, security, zerotrust, ai）に統一したいです。
```

このように命令すると、すべての記事からタグが大文字小文字が使われている記事の一覧をリストアップしてくれました。す、すごい・・・！人間がやると何倍も時間がかかります。
ただ「OKなので直してくれ」と命令したところ、

```
直接ファイルを一括編集することはできないため、お使いのWindows環境ですぐに実行できる PowerShellスクリプト を作成しました。
このスクリプトを実行すると、_posts フォルダ内にあるすべての Markdown ファイル (.md, .markdown) をチェックし、tags: の行を見つけてタグをすべて小文字に自動変換します。また、Jekyll でエラーにならないよう BOMなしの UTF-8 で保存するようにしています。
```

だの

```
申し訳ありませんが、現在提供されているファイル情報の中に、先ほどリストアップした修正対象のMarkdownファイル（2025-12-20-result-of-CPE.markdown や 2025-08-03-devin-vs-takumi.markdown など）が含まれておりません。

IDEの機能を使って直接「適用（Apply）」するためには、対象ファイルの正確な内容を読み込む必要があります。

お手数ですが、修正対象のMarkdownファイルをエディタ上で開くか、チャットの入力欄で @ を使って対象ファイルを追加した上で、再度リクエストしていただけますでしょうか？ ファイルが提供され次第、1つずつ確認・承認いただける形式（Diff）で出力いたします。
```

だの言われて時間がかかりそうだったので、AIが提示したリストを見ながら自分で修正することにしました。文章内の単語ではなく、タグだけを狙い撃ちして小文字に変える、最も安全で速い方法はVSCode上で「キャプチャグループ」という機能を使います。例えば「Security」というタグを「security」に変更する方法は

- 検索欄： (tags:.*)Security
- 置換欄： $1security

と書きます。(...) で囲った部分は「グループ1」として保存されます。置換欄の $1 は、「グループ1（つまり tags: から Security の直前までの文字）」をそのまま再利用するという意味です。
これなら、tags: [AWS, Security] が tags: [AWS, security] になります

ただ、最終的に「bundle exec htmlproofer」のNGがどれだけAIと相談しても解消されず、Testはコメントアウトすることとしました。

そして、ブログのレイアウトでタイトルが改行されるところがどうしても直せませんでした、style.scssをアレコレいじって改行禁止したり、文字サイズを小さくしたりと挑戦したのですが上書きされず、1時間以上かけても直せなかったので、結局ブログのタイトルを短くすることで解決しました（諦めたとも言うw）

ここまで作ってgit pushすれば、わずか数時間で新しいブログを作ることができました。いや、3年前もAIを使いながらブログの公開にこぎつけましたが、今のAIは本当に賢いです。3年前ではこのスピードでデプロイは出来なかったでしょう。むしろ挫折していたと思いますｗこの圧倒的生産性速度のUpが世界中の開発現場で起きているのですから。

## （７）ブログのURLをnewblogからblogに変更

上手く表示されたので、URLを移行します。

GitHub上の blog リポジトリを開き、 Settings タブをクリック。
Repository name を blog から blog_2026archive に変更して Rename。
※このリポジトリはprivateにしたのちアーカイブします。

newblog リポジトリを開き、 Settings タブをクリック。
Repository name を newblog から blog に変更して Rename。

newblogディレクトリ配下でリモートURLの更新

`
git remote set-url origin https://github.com/ユーザ名/blog.git
`
その後config.yml の設定でbaseURLを"/blog"にします。
git pushすれば、今までのURLで新しいブログが公開されます。

いかがだったでしょうか？Google Search Consoleの設定などもしましたが、簡単にブログをリニューアルすることができました。
この文章も細かい表現やtypoをGemini Code Assistに直してもらいました。うーん、すごい時代だ。