---
layout: post
title:  "AWS認定 AWS Security Specialityを更新しました"
date:   2024-03-31 21:50:27 +0900
categories: certification
image:
  path: /assets/img/AWS-Certified-Security-Specialty_badge.png
 # Add image post (optional)
tags: [AWS,Security] # add tag
description: AWSセキュリティ専門知識を更新しました
---

もっとセキュリティの仕事を極めたい、セキュリティ担当の僕です。
AWSの資格は現在

- Solution Architect Professional
- Devops Engineer Professional
- Security Speciality

の3つを保有しています。DevOpsはあまり意味のない資格でしたが、残りの2つはこれからも保持し続けたいと思っています。

昨年のSAP更新後3ヶ月ほど勉強し挑んだ今回の試験。「あれ、思ったより難易度が高いぞ？」という感想です。3年前は単純な知識を問われる比率が多かった記憶があるのですが、今回出てきた問題は、サービスを組み合わせて利用する応用問題が多かったです。加えて、S3のボールトロックや、IAM Identity Center（元AWS SSO）など、普段遣いしないサービスに対する問題も出てきてかなり悶絶しました。

ただ、これらの問題が全て実践的でないかというとそんなことは全く無くて、LambdaとVPCエンドポイントを使った構成でなぜかDBにLambdaが接続できない理由が問われたのですが、これはまさに最近仕事でうまくいかず、原因を調べたうえで解決した事例でした。こういう問題がドンピシャで出題されたりしますので、試験合格のための試験勉強ではなく、自分の実力を証明するため、自分の実践に必要な学びのために資格勉強と資格取得を続けたいですね。

翌日合格の通知が来ました。3年前はもっと余裕のスコアだった記憶があるのですが、今回はかなりギリギリでした。知らないことも多く聞かれましたし、危なかった・・・。

今回の勉強に利用した教材を紹介します。

[Practice Exam AWS Certified Solutions Architect Professional](https://www.udemy.com/course/practice-exam-aws-certified-solutions-architect-professional/learn/quiz/5723044)
AWSの資格の講座をUdemyでたくさん配信しているStephane Maarekの講座。お安定のクオリティです。

[AWS Certified Security Specialty](https://www.udemy.com/course/draft/5460176/learn/quiz/5977920)
こちらはちょっと癖のある難易度の問題がそろった講座。KMSの問題が多かったのですが、実際にはそこまでKMSの問題は出ませんでした。

[要点整理から攻略する『AWS認定 セキュリティ-専門知識』](https://book.mynavi.jp/ec/products/detail/id=115809)
3年前に購入した本、AWSはセキュリティに関してはけっこうアップデートが多く、3年も前ですと内容も古かったりします。今やInspectorは自動的にECRにデプロイされたコンテナイメージを脆弱性診断しますしね。とはいえ貴重な日本語の本、これからセキュリティ専門知識の勉強をする人にはお勧めの1冊。