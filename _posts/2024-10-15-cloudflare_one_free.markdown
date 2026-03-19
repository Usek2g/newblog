---
layout: post
title:  "CloudFlare Zero Trust無料化で何ができるのか調べてみた"
date:   2024-10-15 00:21:27 +0900
categories: product
image:
  path: assets/img/cloudflareone.png
 # Add image post (optional)
tags: [zerotrust,cloudflare] # add tag
description: CloudFlare Zero Trustの一部サービスが無料化したとのこと、何ができるのだろう
---

10.23 現在CloudFlare OneはCloudFlare Zero Trustに改称しているということですので、タイトルをCloudFlare OneからCloudFlare Zero Trustに修正しました。とはいえ、最近の公式の文章の中でもCloudFlare Oneが使われているのをよく見かけます。

CloudFlareは今最も勢いのあるインターネット企業の1つです。僕もCloudFlare Pagesを利用させてもらっています。zipもしくはGitHubのリポジトリから簡単に静的サイトをデプロイできる便利なサービスです。そのCloudFlareが9/24に自社のセキュリティツールを無料化するとアナウンスしました。

[Cloudflareが「ゼロトラストセキュリティ」のツールを無料化　何が狙いなのか(ITmedia)](https://atmarkit.itmedia.co.jp/ait/articles/2410/03/news062.html)
※会員限定記事ですが、無料で読める枠にほぼすべての情報が入っています


[Cloudflareでより安全なインターネットを：無料の脅威インテリジェンス、分析、新しい脅威検出(CloudFlareブログ)](https://blog.cloudflare.com/ja-jp/a-safer-internet-with-cloudflare/)
非常に長い文章が書かれていますが、終盤に登場します。

>Cloudflare One
>Cloudflare Oneは、インターネットで人、アプリ、デバイス、ネットワークを保護し、接続するために設計された包括的なセキュアアクセスサービスエッジ（SASE）プラットフォームです。Zero Trustネットワークアクセス（ZTNA）、セキュアWebゲートウェイ（SWG）などのサービスを1つのソリューションに統合します。

AIはより分かりやすい説明をしてくれました。
> Cloudflare Oneは、Zero Trustセキュリティサービスとネットワークサービス（Magic WANおよびファイアウォールなど）を組み合わせたシングルベンダーSASEプラットフォームです

SASE（Secure Access Service Edge）とは、2019年にガートナーによって提唱された、ネットワークの機能とセキュリティの機能を一体として提供するクラウドサービス、またはその考え方・概念を表す言葉です。ゼロトラストの時代はネットワークのセキュリティを固めることこそ最重要と言っても過言ではなく、様々なサービス・機器によりリアルタイムで広範囲のセキュリティを担保しようとしていますが、セキュリティ確保のための情報をまとめたものをSASEと言います。

[SASE（サシー）とは？ ゼロトラストとの違い・関係も解説(日立ソリューションズ・クリエイト)](https://www.hitachi-solutions-create.co.jp/column/security/sase.html)

[SASEとはどんな概念か？誕生の背景とアーキテクチャの考え方](https://www.ntt.com/business/lp/sase.html)


>過去2年間にCloudflare Oneに追加された4つの新製品がすべての方に無料でご利用いただけるようになりました：
>    SaaSアプリケーションのリスクを軽減するためのクラウドアクセスセキュリティブローカー（CASB）。
>    ネットワークおよびSaaSアプリケーションから機密データが漏えいするのを防ぐためのデータ損失防止（DLP）。
>    あらゆるネットワーク上のユーザーエクスペリエンスを表示するためのDigital Experience Monitoring。
>    ネットワークを通過するすべてのトラフィックを表示するためのMagic Network Monitoring。

つまり、従来有料サービスとして提供されていた上記機能が、Freeプランでも利用できるということのようです。
今回調べた結果、Freeプランは**おそらく**CloudFlareサービスのエージェントであるWARPをインストールした台数でユーザ数をカウントし、50人以下までであれば無料で使えると思われます。中小企業はもちろん、大企業でもセキュリティを強化しなければならない特定のプロジェクトに対して利用することもできると思います。

今回、CloudFlare Zero Trustの基本機能と、今回追加された新たな製品で何ができるのか、調べてみることとしました。

[はじめての Cloudflare One (1) Secure Web Gateway で Agentless DNS Filtering][https://zenn.dev/kameoncloud/articles/032ec7d8572d9f]

CloudFlareのゼロトラスト技術に関してはCloudFlareエバンジェリストが書かれた上記記事のイラストが分かりやすいです。インターネットに抜けるあらゆる通信に対してプロキシ機能を持ち、外から中へのアクセス制限「Zero Trust Network Access」と、中から外へのアクセス制限「Secure Web Gateway」で構成されているようです。このSecure Web Gatewayは一般用語でいうと CASB (Cloud Access Security Broker) に相当するものです。

この記事を見ながらCloudFlare Zero Trustの基本機能「Agentless DNS Filtering」を試してみました。最新の画面UIと説明に少し差異がありましたが、基本的には問題なく進めました。

最後のGoogle ChromeでセキュアDNSを利用するところだけ、ChromeでなかなかセキュアDNSの設定変更箇所を見つけられず苦労しましたが、添付画像の位置にあります。

<img src="{{site.baseurl}}/assets/img/CustomeDNSonChrome.jpg" alt="セキュアDNSの場所">

次に、CloudFlare Zero Trustに今回提供された4つの機能について調べてみました。いったん調べた結果飲み記述しますが、後日実際にエージェントをインストールして動作を確認してみたいと思っています。

## CASB
[Cloudflare Zero Trust の CASB機能 をまとめてみた](https://dev.classmethod.jp/articles/cloudflare-zerotrust-casb-summary/)
CASBはクラウドアクセスセキュリティブローカーのことで、クラウドへのアクセスを検知してブロックしたりすることで、企業内のシャドーITを防ぐ効果が期待できるソリューションのことです。CloudFlare ZeroTrustの内部ではそのものずばり「Shadow IT Discovery」という名前で、許可していないクラウドサービスへのアクセスを監視できるようです。先ほどセキュアDNSサーバの迂回によるWebアクセスフィルタリングはエージェントレスで実現できたものの、Webブラウザ限定でした。ここにあるCloudFlare WARPはエージェントとしてPCやスマートフォンに常駐し、VPN機能やCASBのようなフィルタリング機能を実現できるようです。

<img src="{{site.baseurl}}/assets/img/CloudFlareZeroTrustShadowIT.jpg" alt="CASB">

## DLP
DLP（Data Loss Prevention）とは、機密情報や重要データを監視・保護し、漏洩や消失を防ぐためのシステムや製品のことを指します。クレジットカード情報やアクセスキー、パスワードが書かれているファイルのアップロードを防ぐような機能があります。

<img src="{{site.baseurl}}/assets/img/CloudFlareZeroTrust_DLP.jpg" alt="DLP">

[Cloudflare Zero Trust で DLP を実装してみた](https://dev.classmethod.jp/articles/cloudflare-dlp-implementation/)
こちらの記事にあるように、ちゃんとDLPとして機能するようです。これが無料なのは凄い。

## Digital Experience Monitoring
これは一般的なセキュリティ用語ではなく、CloudFlareの造語と思いますが、どのような機能なのでしょうか。こちらもDeveloper’s I/Oで紹介されていました。

[Cloudflare Zero Trust 〜DEXのFleet Statusを使ってみた〜](https://dev.classmethod.jp/articles/dex_fleetstatus_nanami/)

ユーザ端末のCloudflare Zero Trustに登録されているユーザーの端末のインベントリを収集や、特定サイトへのアクセスのパフォーマンスを核にできる機能のようです。EDRを導入している場合は既に同じようなモノを導入済みかもしれませんね。

## Magic Network Monitoring
公式に[Cloudflare Magic Network Monitoring](https://www.cloudflare.com/ja-jp/network-services/products/magic-network-monitoring/)の記事があります。しかし探してもどこにこの機能があるのか…英語のページも調べていたところ、Zero Trustの中にはなく、トップページの「分析とログ」配下にありました。[設定方法](https://developers.cloudflare.com/magic-network-monitoring/get-started/)も英語サイトに載っていましたが、物理ルータのIPアドレスを登録し監視用の設定を行うことで、ルータを通るトラフィックの内容をCloudFlare上でモニタリングすることができるようです。VPNルータを設定することで、リモートワークで業務を行っている従業員のトラフィックも確認することができそうです。

# まとめ
無料で利用できるようになる前の画面と価格を知らないのすが、現時点でCloudFlare Zero Trustで無料で使えるようになった4つのサービスについて確認することができました。特にCASBとDLPに魅力を感じました。特定サイトへのアクセス禁止はEDRで実現している企業もあると思いますが、DLPに関してはかなり本格的な機能を持っており、ちゃんと設定できればセキュリティを相応に強化することができます。会社で導入できるかも、というビジョンが頭の中にあるので関係者に話をしてみようと思いました。