---
layout: post
title:  "Chromebook上にjekyllをインストールするまで"
date:   2022-11-20 23:30:27 +0900
categories:  tech-artile
image:
  path: assets/img/software.jpg # Add image post (optional)
tags: [github pages, jekyll] # add tag
description: Chromebook上にjekyllをインストールするまでを書いた記事.
---

記事を書く前に、ここまでの環境を用意した際の手順、参考にしたURLをメモします。
同じことをやりたいときに１から調べ直さないよう、また同じことをやりたい人にとって参考になれば幸いです。

# GitHubの準備
もともとGitHubは使っていたので参考程度に

- [Chromebook にGitHubリポジトリを作成してリモートリポジトリにプッシュする方法](https://kewton.blog/archives/510#i-0)
- [GitHubにSSH接続する](https://hishitu.net/posts/githubsshkeys/)
- [gitでssh接続する際にBad configuration option: usekeychainやterminating, 1 bad configuration optionsとエラーが出たときの解決方法](https://qiita.com/kaino5454/items/98dcf3a996f2074e0074)

# ChromeBookでVisual Studio Codeを使う
- [ChromebookでVSCodeを使う（日本語化対応）](https://www.tenohira.xyz/tech/vscode-install-chromebook/)
- [ChromeBookのVSCodeで日本語入力できるようにする](https://gotoblog.org/chromebook-vscode-japanese/)

## GitHub Pagesの作り方
はてなブログも考えたけど、あまりソーシャルにつながる必要もないかな、と思いGitHub Pagesを使ってみることにしました。

- [GitHub Pagesによる静的サイトの公開](https://note.com/npaka/n/n9040538e2edd)
- [公式手順](https://docs.github.com/ja/pages/getting-started-with-github-pages/creating-a-github-pages-site)

## jekyllの利用
jekyllはMarkDownからhtmlを出力してくれる便利ツール。デザインも用意してくれています。

- [GitHub Pagesの使い方 – Jekyllでブログを作る方法【記事投稿まで】](https://howpon.com/9443)

ただしインストールはかなり苦戦しました。調べていくうちに以下の動画が見つかりました。やはり今の時代は動画か・・・。

- [Install Jekyll On ChromeOS (Linux Beta)?](https://youtu.be/lZrOVh-4CAw)

最終的に以下を実行したと思われます（試行錯誤なので自信が・・・gemでblenderもインストールしたような)
```
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"'>> ~/.bashr
source ~/.bashrc
gem install jekyll 
```

- [jekyll公式のトラブルシューティング](http://jekyllrb-ja.github.io/docs/troubleshooting/)

せっかくなのでデザインを変更してみました。

- [GitHub Pagesの使い方 – Jekyllのブログテーマを変更する方法](https://howpon.com/10476)

テーマは[flexible-jekyll](https://github.com/artemsheludko/flexible-jekyll)を使わせていただきました。シンプルでよいです。
faviconを変えたりbaseUrlを設定してcssの相対パスが正しく動くよう調整しました。