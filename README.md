
# network_training

cEOSを使用して、インターン研修中に構築したネットワークを再現することを目的としています。

## 動作要件

このプロジェクトを実行するには、[**Containerlab**](https://containerlab.dev/)がインストールされている必要があります。また、[ガイド](https://containerlab.dev/manual/kinds/ceos/)に従って、Arista cEOSのイメージを準備する必要があります。

## コンテナ群の立ち上げ

各サブディレクトリ（ex `static_route`）に移動し、以下のコマンドを実行してください。

```bash
sudo clab deploy
```

このコマンドにより、定義されたネットワークトポロジーに基づいてcEOSコンテナが起動し、相互に接続されます。

## スイッチの操作

### スイッチに入る

起動したcEOSスイッチコンテナに接続するには、以下のコマンドを使用します。`CONTAINER`の部分は、`sudo clab deploy`実行時に表示されるコンテナ名（例: `sw1`, `r1` など）に置き換えてください。

```

docker exec -it CONTAINER Cli

```

これにより、cEOSのCLI（コマンドラインインターフェース）に入り、設定や状態確認を行うことができます。

### 設定の永続化

cEOSスイッチ内で`write`コマンドを実行した場合、設定は`<サブディレクトリ>/<lab name>/<sw1|sw2>/flash/startup-config`に保存されます。この設定ファイルは、コンテナが削除されると失われてしまいます。

再デプロイしても設定を保持したい場合は、以下の手順で`startup-config`の内容をサブディレクトリ直下の設定ファイル（`ceos.sw1.cfg`、`ceos.sw2.cfg`）にコピーしてください。

ex) `static_route`以下で作業する場合
```
cp clab-static/sw1/flash/startup-config ceos.sw1.cfg
cp clab-static/sw2/flash/startup-config ceos.sw2.cfg
```
`clab-static`はデプロイ後に生成されます。


## コンテナ群の停止と削除

作業が終了したら、以下のコマンドでコンテナを停止し、関連するリソースを削除できます。

```

sudo clab destroy

```

これにより、Dockerコンテナ、ネットワークブリッジなどがクリーンアップされます。

## ネットワークトポロジー

各サブディレクトリには、そのネットワーク構成の概要を示す`*clab.yml`ファイルが含まれています。このファイルを確認することで、どのようなスイッチやルーターがどのように接続されているかを確認できます。
