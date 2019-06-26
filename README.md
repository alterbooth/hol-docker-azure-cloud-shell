<!-- TOC -->

- [Azure Cloud Shell を使った Docker ハンズオン](#Azure-Cloud-Shell-%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%9F-Docker-%E3%83%8F%E3%83%B3%E3%82%BA%E3%82%AA%E3%83%B3)
- [Azure Portal](#Azure-Portal)
    - [Azure Portal とは](#Azure-Portal-%E3%81%A8%E3%81%AF)
  - [ログインする](#%E3%83%AD%E3%82%B0%E3%82%A4%E3%83%B3%E3%81%99%E3%82%8B)
- [Azure Cloud Shell](#Azure-Cloud-Shell)
  - [Azure Cloud Shell とは](#Azure-Cloud-Shell-%E3%81%A8%E3%81%AF)
  - [試してみる](#%E8%A9%A6%E3%81%97%E3%81%A6%E3%81%BF%E3%82%8B)
- [Docker Machine](#Docker-Machine)
  - [Docker Machine とは](#Docker-Machine-%E3%81%A8%E3%81%AF)
  - [Docker Machine を使用する](#Docker-Machine-%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%99%E3%82%8B)
- [Docker in Azure](#Docker-in-Azure)
  - [Cloud Shell で進めるために](#Cloud-Shell-%E3%81%A7%E9%80%B2%E3%82%81%E3%82%8B%E3%81%9F%E3%82%81%E3%81%AB)
  - [Microsoft Learn](#Microsoft-Learn)

<!-- /TOC -->

# Azure Cloud Shell を使った Docker ハンズオン

# Azure Portal

### Azure Portal とは

[Microsoft Azure Portal | Microsoft Azure](https://azure.microsoft.com/ja-jp/features/azure-portal/)

## ログインする

<https://portal.azure.com/>

# Azure Cloud Shell

## Azure Cloud Shell とは

[Azure Cloud Shell の概要 | Microsoft Docs](https://docs.microsoft.com/ja-jp/azure/cloud-shell/overview)

## 試してみる

[Azure Cloud Shell の Bash のクイックスタート | Microsoft Docs](https://docs.microsoft.com/ja-jp/azure/cloud-shell/quickstart)

# Docker Machine

Cloud Shell から Docker をコントロールする準備をします。

> 注意<br>
> [Azure Cloud Shell のトラブルシューティング | Microsoft Docs](https://docs.microsoft.com/ja-jp/azure/cloud-shell/troubleshooting#cannot-run-the-docker-daemon)

## Docker Machine とは

- [Docker Machine Overview | Docker Documentation](https://docs.docker.com/machine/overview/)
- [Docker Machine 概要 — Docker-docs-ja 17.06.Beta ドキュメント](http://docs.docker.jp/machine/overview.html)

## Docker Machine を使用する

Cloud Shell で動かすためにはドキュメント内のコマンドで一部オプションの追加 (`--shell bash`) が必要になります。ドキュメント内のコマンドを下に記載のコマンドと読み替えて進めてください。

[Docker マシンを使用して Azure で Linux ホストを作成する | Microsoft Docs](https://docs.microsoft.com/ja-jp/azure/virtual-machines/linux/docker-machine)

Azure サブスクリプション ID を取得します。

```shell
sub=$(az account show --query "id" -o tsv)
```

リソースを作成および管理するアクセス許可を Docker マシンに付与します。

```shell
docker-machine create -d azure \
    --azure-subscription-id $sub \
    --azure-ssh-user azureuser \
    --azure-open-port 80 \
    --azure-open-port 8080 \
    --azure-size "Standard_DS1_v2" \
    --azure-location japanwest \
    myvm
```

Docker ホストの接続情報を表示します。

```shell
docker-machine env myvm --shell bash
```

接続設定を定義します。

```shell
eval $(docker-machine env myvm --shell bash)
```

コンテナーを実行します。

```shell
docker run -d -p 80:80 --restart=always nginx
```

コンテナーをテストします。

```shell
docker-machine ip myvm
```

# Docker in Azure

## Cloud Shell で進めるために

- ドキュメントに出てくる `localhost` は `docker-machine ip myvm` で取得する Docker ホストのパブリック IP アドレスと読み替えてください。
- Cloud Shell ではエディターを使えます。
  - [Azure Cloud Shell エディターの使用 | Microsoft Docs](https://docs.microsoft.com/ja-jp/azure/cloud-shell/using-cloud-shell-editor)
- Cloud Shell でファイルを永続化するには `~/clouddrive` フォルダを使用してください。

## Microsoft Learn

- [Docker を使用してコンテナー化された Web アプリケーションを構築する - Learn | Microsoft Docs](https://docs.microsoft.com/ja-jp/learn/modules/intro-to-containers/)
  - Docker HUB
  - Docker CLI
  - Dockerfile
  - Azure Container Registory
  - Azure Container Instance
- [Azure App Service を使ってコンテナー化された Web アプリをデプロイして実行する - Learn | Microsoft Docs](https://docs.microsoft.com/ja-jp/learn/modules/deploy-run-container-app-service/)
  - Azure Container Registory
  - Azure Web Apps for Container
  - 継続的デプロイ

