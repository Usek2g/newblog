---
layout: post
title:  "Jykellの小技(RSSフィードを作成する・サイトマップを登録する)"
date:   2022-12-28 10:00:27 +0900
categories: tech-artile
image:
  path: assets/img/rssfeed.jpg # Add image post (optional)
tags: [Github Pages, jekyll] # add tag
description: Github Pagesのアクセス数やユーザビリティを向上させる小技を紹介
---

今年最後の記事はセキュリティとは関係ないJekyllの小技です。

# RSSフィードを設定する
[もう古い？！RSSで効率よく最新情報を取得する方法とは](https://www.docusign.jp/blog/guide-to-RSS-feeds)
たしかに、今ではあまり使われなくなっていますが、僕は技術ブログやニュースサイトの情報収集にまだ使っています。
そんなRSSフィードを配置しました。


<img src="{{site.baseurl}}/assets/img/rssfeed.jpg" alt="RSSフィード">
URLは[こちら](https://usek2g.github.io/blog/feed.xml)。

導入方法は簡単、というかデフォルト設定で既に登録されていました。URLを知らないだけだった…。
[https://github.com/jekyll/jekyll-feed](https://github.com/jekyll/jekyll-feed)

# サイトマップを設定する

[サイトマップって何？SEO効果から作成方法まで徹底解説](https://digitalidentity.co.jp/blog/seo/seo-tech/how-to-make-sitemap.html)
動機としては、Google検索結果にGitHub Pagesを表示させる方法を調べていました。

[GitHub PagesでGoogle検索に表示させたい](https://qiita.com/r_saiki/items/9ab3be34fa255724c9dd)

jykellを使っている場合、「jekyll-sitemap」は元々組み込まれているため、わざわざ追記する必要はありません。
上記の記事に従いGoogle Search Consoleにsitemap.xmlを登録‥‥しようとしたところエラーが表示されました。
sitemap.xmlを見ると、すべての記事のパスが相対パスとなっており、実際のURLが登録されていませんでした。
どこかでベースのURLを持っているのだろうな、と探しているうち`_config.yml`の以下の記述を見つけました。

`url: "https://usek2g.github.io" # the base hostname & protocol for your site, e.g. http://example.com`

これまでurlの値をブランクに設定していましたが、コメントにまさしくパスを入力するように書かれていたので、ここにGithub PagesのURLを設定。
すると正しくsitemap.xmlが作成され、Google Search Consoleのステータスも成功に変わりました。
数日待ち、Google検索結果に表示されるのを待つとしましょう。