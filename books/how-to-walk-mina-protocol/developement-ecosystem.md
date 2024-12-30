---
title: "Mina Protoclの開発エコシステム"
free: false
---

この章では、Mina Protocolの開発エコシステムについて解説します。

## O1js

通常であればSDKに相当するのが[O1js](https://github.com/o1-labs/o1js)です。ユニークなのは、[ゼロ知識証明を利用したアプリケーションをTypeScriptで開発するライブラリという側面をもち](https://docs.minaprotocol.com/zkapps/o1js)、Mina Protocolとは独立して利用が可能なところでしょう。

構成要素としては、以下のようになっています

### Mina ProtocolのSDK部分

スマートコントラクトやデプロイ、トランザクション、鍵管理などの一般的なSDK機能の部分です。

### Field・Function・Conditionals


ゼロ知識証明を書くための[基本的なデータタイプ及び専用の計算や条件分岐関数](https://docs.minaprotocol.com/zkapps/o1js/basic-concepts)です。これを利用して書かれたロジックは、実行の保証がされた状態になります。逆にいうとこれを通さない普通のコードは証明されません。


### Gadeget

ビット演算や公開鍵暗号など、汎用的かつ比較的高レベルな機能を実現するクラス・関数群です。

### ZKApps

Mina Protocolとは独立したゼロ知識証明アプリケーションを書くためのフレームワークです。証明のための鍵と証明をそれぞれ生成することが可能で、再帰的ゼロ知識証明を書くことができます。

### Markle Tree等の基礎関数群

何かの一部に含まれていることを証明するための[Markle Tree](https://gaiax-blockchain.com/merkle-tree)やゼロ知識証明と計算量的に相性のいいPoseidon Hash、有限体を表現するForien Field、群を表現するgroupといった理論的な部分をアプリケーションや追加のライブラリに実装する際に使われるクラスや関数があり、上述のGagdetや[サードパーティのライブラリ](https://github.com/o1-labs/o1js?tab=readme-ov-file#community-packages)に使われています。



## Protokit

protokitは上で述べたO1jsを使用したフレームワークです。アプリケーションチェーンとよぶ、再帰的ゼロ知識証明を利用したアプリケーションの為のフレームワークであり、またMina Protocolの特徴であるローカル実行の欠点であるレースコンディション(競合状態)をサーバーサイドで解消するための機能を備えています。これは、予約サイトのようなケースを想定してみると解りやすいかと思います。

https://protokit.dev

アーキテクチャとしてはRuntime、Protocol、Sequencerの三つで構成されており、それぞれ以下のような機能を提供します

### Runtime

ゼロ知識証明によるビジネスロジックを書く部分であり、[O1jsによるロジックをそれぞれ分離して書く機能](https://protokit.dev/docs/architecture/runtime#runtime-module-interoperability)と、o1jsにない[「判定に失敗したことを保証する」機能があります](https://protokit.dev/docs/architecture/runtime#soft-failing-runtime-methods)。

### Protocol

Protokitの中核部分です。Runtimeを繋げた証明のグラフ(ネットワーク)構造を記述し、それを元にBlockと呼ぶ再帰的ゼロ知識証明のステップを生成、マージする機能と、そのグラフに対して拡張するためのフックを設定する機能があります。

### Sequencer

ユーザーのような外部の入力とProtokit内部のロジックを結びつける部分です。入力に基づいてprotocolを呼び出し、必要に応じて並列実行や競合状態の解決、状態保存を行い、Mina Protocolに書き込む一連の流れを受け持ちます。

公式サイトトップにある初期化コマンドで生成された状態ではSequencerはGraphQLのAPIを持っている状態になっており、同梱されている
[caddy](https://caddyserver.com/docs/automatic-https)とまとめてDockerでサーバーにデプロイすることでSSLの下で配信されるAPIが構築できるようになっています。

## Orochi Network

Orochi Networkは最近[Mina Protocolとの提携が発表された](https://x.com/MinaProtocol/status/1869759614868410619?mx=2)チーム^[公式サイトを調べた限りでは企業なのか財団やNPOなのか解りませんでした]です。ゼロ知識証明を中心に暗号系技術を提供しており、その中の[zkDatabase](https://www.zkdatabase.org)が現在Mina Protocolと密接に結びついているプロダクトになります。

https://www.orochi.network

この本の最初に述べた通り、Mina Protoclはスマートコントラクトをオフチェーンで実行するという特性を持ちます。この特性にはデメリットとして、スマートコントラクト上に記録されたデータを全て読み込んでしまうのでデータ量に制限があり、トークンの実装のようなオンチェーンでそれなりに大きなデータを蓄積するユースケースが難しくなります。

この問題を解決するのがzkDatabaeです。公式サイトのトップにあるデモの通り、データを書き込むとトランザクションを証明するマークルツリーとハッシュが出力され、これを元に検証することで内容を保証することができます。アクセス制限などの機能も持っていて、実用性は十分だと感じました。

##　Mina Playground

[Mina Playground](https://minaplayground.com/)