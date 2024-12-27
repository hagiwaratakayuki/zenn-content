---
title: "Mina Protoclの開発エコシステム"
free: false
---

## Mina Protocolの開発環境

開発環境の視点でみると、Mina Protocolはゼロ知識証明を利用したアプリを手軽に開発、運用できるようにするツールチェーンと一種のクラウド環境の組み合わせです。本体自体はSDKであると同時にゼロ知識証明をTypeScriptで独立しての利用も可能な[O1js](https://github.com/o1-labs/o1js)とデータの証明の置き場兼検証環境としてのブロックチェーンがあり、最低限だけど絶対に必要となるものはそろっています。

## 

## Protokit
再帰的ゼロ知識証明のチェーンを作るためのフレームワークである[protokit](https://protokit.dev)
Runtime = application logic
Protocol = underlying VM, block production, transaction fees, account state, …
Sequencer = mempool, block production, settlement to L1
starter kitでは、GraphQLベースのインデクサもサポート

・[caddy](https://caddyserver.com/docs/automatic-https)を内蔵
・Dockerで固めてデプロイ

## Orochi Network

・最近協業が発表された
・ゼロ知識証明がついたデータベース
・O1js的にはLocal State
・証明可能な乱数や、ゼロ知識で証明されたメモリも提供

##　Mina Playground

[Mina Playground](https://minaplayground.com/)