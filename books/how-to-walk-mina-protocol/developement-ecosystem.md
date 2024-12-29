---
title: "Mina Protoclの開発エコシステム"
free: false
---

この章では、Mina Protocolの開発エコシステムについて解説します。

## O1js

通常であればSDKに相当するのが[O1js](https://github.com/o1-labs/o1js)です。ユニークなのは、[ゼロ知識証明を利用したアプリケーションをTypeScriptで開発するライブラリという側面をもち](https://docs.minaprotocol.com/zkapps/o1js)、Mina Protocolとは独立して利用が可能なところでしょう。

構成要素としては、以下のようになっています

### Mina ProtocolのSDK部分

公式の名称はないのですが、スマートコントラクトやデプロイ、トランザクション、鍵管理などの一般的なSDK機能の部分です。

### Field・Function・Conditionals


ゼロ知識証明を書くための[基本的なデータタイプ及び専用の計算や条件分岐関数](https://docs.minaprotocol.com/zkapps/o1js/basic-concepts)です。これを利用して書かれたロジックは、実行の保証がされた状態になります。逆にいうとこれを通さない普通のコードは証明されません


## Gadeget

ビット演算や公開鍵暗号など、ゼロ知識証明そのものとは結びついていないもののアプリケーションを書くのに必要なライブラリです。

## Markle Tree等の基礎関数群

これも公式な名称はないのですが、何かの一部に含まれていることを証明するための[Markle Tree](https://gaiax-blockchain.com/merkle-tree)やゼロ知識証明と計算量的に相性のいいPoseidon Hash、有限体を表現するForien Field、群を表現するgroupといった理論的な部分をアプリケーションや追加のライブラリに実装する際に使われるクラスや関数があり、上述のGagdetや[サードパーティのライブラリ](https://github.com/o1-labs/o1js?tab=readme-ov-file#community-packages)に使われています。

。

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