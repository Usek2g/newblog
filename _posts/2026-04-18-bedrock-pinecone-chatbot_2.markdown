---
layout: post
title:  "BedrockとPineconeを使って資料特化型AIチャットを作成してみた(後編)"
date:   2026-04-11 11:15:27 +0900
categories: diary
image:
  path: assets/img/bedrock-pinecone-chatbot.png
 # Add image post (optional)
tags: [aws,pinecone,ai,notebook] # add tag
description: BedrockとPineconeを使って社内向けセキュリティ用AIチャットを作成した
---


---
LLM (大規模言語モデル)：膨大なデータで学習した「AI の脳」そのもの。	Claude 3 Haiku や Titan など。
RAG (検索拡張生成)	：外部データを参照して回答の精度を高める手法。	今回構築した社内向けセキュリティ用AIチャットは、RAGだった。

RAG（Retrieval-Augmented Generation）とは、直訳すると「検索（Retrieval）によって拡張（Augmented）された生成（Generation）」です。
- Retrieval（検索）: 質問に関連する社内資料を Pinecone から探し出す工程。
- Augmentation（拡張）: 探し出した資料（知識）を、AI への指示（プロンプト）に付け加える工程。
- Generation（生成）: 付け加えられた知識を元に、Claude 3 Haiku が回答を作成する工程。

埋め込み (Embedding)	言葉を「意味」を反映した数字の羅列（ベクトル）に変換すること。	Titan Text Embeddings v2 が担当しました。
ベクトルデータベース	ベクトル化されたデータを高速に検索・保存する場所。	Pinecone （または OpenSearch Serverless）です。
トークン (Token)	AI が処理する「文字の断片」の単位。課金や制限の基準になります。	入力・出力の文字数に応じて課金される「通貨」のようなもの。

Bedrockは「世界中の優れた知能（モデル）」と「あなたの会社のデータ（S3/Pinecone）」を安全に、かつ簡単に繋ぎ合わせるための「高性能なハブ（統合プラットフォーム）」 。

| パーツ | 役割 | 今回の具体例 | Bedrock の仕事内容 |
| ---- | ---- | ---- | ---- |
| データソース | 原材料（資料） | S3 | 定期的に S3 を見に行き、新しい資料がないか確認する。 |
| Embedding モデル | スカウト（数値化） | Titan Text v2 | 資料を読み込み、意味を数値（ベクトル）に変換する指示を出す。 |
| ベクトルデータベース | 記憶の索引（保存） | Pinecone | 数値化したデータを、外部の Pinecone に整然と並べて保存する。 |
| LLM モデル | 執筆・回答（脳） | Claude 3 Haiku | 検索結果を読み込ませ、ユーザーへの最終回答を書かせる。 |


Bedrock がハブとして提供している「付加価値」は以下の通り。
API の共通化: どのモデル（Claude, Llama, Mistral 等）を使っても、同じ形式の API で呼び出せる。
フルマネージドなパイプライン: 「S3 のファイルを分割（チャンク化）し、ベクトル化して Pinecone へ送る」という一連の面倒な流れを、「同期」ボタン一つで実行してくれる。
セキュリティとガバナンス: 会社の機密データを OpenAI などの外部へ送ることなく、AWS のセキュアな閉域網の中で処理を完結させ、IAM でアクセス権を細かく制御できる。