---
title: "Service Fabric コンテナー アプリケーションをパッケージ化およびデプロイする | Microsoft Docs"
description: "Yeoman を使用して Azure Service Fabric アプリケーションの定義を作成し、アプリケーションをパッケージ化する方法について説明します。"
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker, コンテナー, マイクロサービス, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 8274d48db034c8a2634ab28bd634c024b8ea055c
ms.contentlocale: ja-jp
ms.lasthandoff: 09/25/2017

---

# <a name="package-and-deploy-containers-as-a-service-fabric-application"></a>Service Fabric アプリケーションとしてのコンテナーのパッケージ化とデプロイ

このチュートリアルは、シリーズの第 2 部です。 このチュートリアルでは、テンプレート ジェネレーター ツール (Yeoman) を使用して、Service Fabric アプリケーションの定義を作成します。 このアプリケーションを使用して、コンテナーを Service Fabric にデプロイすることもできます。 このチュートリアルで学習する内容は次のとおりです。 

> [!div class="checklist"]
> * Yeoman のインストール  
> * Yeoman を使用してアプリケーション パッケージを作成する
> * コンテナーで使用するアプリケーション パッケージの設定を構成する
> * アプリケーションのビルド  
> * アプリケーションのデプロイと実行 
> * アプリケーションのクリーンアップ

## <a name="prerequisites"></a>前提条件

- このチュートリアル シリーズの[第 1 部](service-fabric-tutorial-create-container-images.md)で作成した Azure Container Registry にプッシュされたコンテナー イメージを使用します。
- Linux 開発環境が[セットアップ](service-fabric-tutorial-create-container-images.md)されています。

## <a name="install-yeoman"></a>Yeoman のインストール
Service Fabric には、ターミナルから Yeoman テンプレート ジェネレーターを使用してアプリケーションを作成できる、スキャフォールディング ツールが用意されています。 以下の手順に従って、Yeoman テンプレート ジェネレーターを使用します。 

1. マシンに nodejs と NPM をインストールします。 Mac OSX ユーザーは、Homebrew パッケージ マネージャーを使用する必要があります

    ```bash
    sudo apt-get install npm && sudo apt install nodejs-legacy
    ```
2. NPM からマシンに Yeoman テンプレート ジェネレーターをインストールします 

    ```bash
    sudo npm install -g yo
    ```
3. Service Fabric Yeoman コンテナー ジェネレーターをインストールします

    ```bash 
    sudo npm install -g generator-azuresfcontainer
    ```

## <a name="package-a-docker-image-container-with-yeoman"></a>Yeoman で Docker イメージ コンテナーをパッケージ化する

1. Service Fabric コンテナー アプリケーションを作成するには、複製されたリポジトリの container-tutorial ディレクトリで、次のコマンドを実行します。

    ```bash
    yo azuresfcontainer
    ```
2. アプリケーションには "TestContainer" という名前を、アプリケーション サービスには "azurevotefront" という名前を付けます。
3. フロントエンド リポジトリの ACR にコンテナー イメージ パスを指定します (例: test.azurecr.io/azure-vote-front:v1)。 
4. Enter を押して、Commands セクションを空のままにします。
5. インスタンス数を 1 に指定します。

yo コマンドを実行するときの入力と出力を次に示します。

```bash
? Name your application TestContainer
? Name of the application service: azurevotefront
? Input the Image Name: <acrName>.azurecr.io/azure-vote-front:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/TestContainer/ApplicationManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/ServiceManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/config/Settings.xml
   create TestContainer/TestContainer/azurevotefrontPkg/code/Dummy.txt
   create TestContainer/install.sh
   create TestContainer/uninstall.sh
```

yeoman を使用して作成したアプリケーションに別のコンテナー サービスを追加するには、次の手順を実行します。

1. ディレクトリを **TestContainer** に変更します。
2. `yo azuresfcontainer:AddService` を実行します。 
3. サービスに 'aazurevoteback'a という名前を付けます。
4. バックエンド リポジトリの ACR にコンテナー イメージ パスを指定します (例: test.azurecr.io/azure-vote-back:v1)。
5. Enter を押して、Commands セクションを空のままにします。
6. インスタンス数を "1" に指定します。

使用しているサービスを追加するためのエントリがすべて表示されます。

```bash
? Name of the application service: azurevoteback
? Input the Image Name: suhuruli.azurecr.io/azure-vote-back:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/azurevotebackPkg/ServiceManifest.xml
   create TestContainer/azurevotebackPkg/config/Settings.xml
   create TestContainer/azurevotebackPkg/code/Dummy.txt
```

このチュートリアルの後の手順で、**TestContainer** ディレクトリを使用します。

## <a name="configure-the-application-manifest-with-credentials-for-azure-container-registry"></a>Azure Container Registry の資格情報を使用してアプリケーション マニフェストを構成する
Service Fabric が Azure Container Registry からコンテナー イメージをプルするには、**ApplicationManifest.xml** に資格情報を入力する必要があります。 


ACR インスタンスへログインします。 [az acr login](/cli/azure/acr#az_acr_login) コマンドを使用して、操作を完了します。 コンテナー レジストリの作成時に割り当てられた一意の名前を入力します。

```bash
az acr login --name <acrName>
```

このコマンドは、完了すると **"Login Succeeded (ログインに成功しました)"** というメッセージを返します。

次に、以下のコマンドを実行して、コンテナー レジストリのパスワードを取得します。 このパスワードは、Service Fabric が ACR で認証を行い、コンテナー イメージをプルするために使用します。

```bash
az acr credential show -n <acrName> --query passwords[0].value
```

**ApplicationManifest.xml** 内の、各サービスの **ServiceManifestImport** 要素の下にコード スニペットを追加します。 **AccountName** フィールドに **acrName** を挿入すると、前のコマンドから返されたパスワードが **Password** フィールドで使用されます。 **ApplicationManifest.xml** の完全なサンプル ファイルは、このドキュメントの最後にあります。 

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="<acrName>" Password="<password>" PasswordEncrypted="false"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>通信/コンテナー ポートからホスト ポートへのマッピングを構成する

### <a name="configure-communication-port"></a>通信ポートを構成する

クライアントがサービスと通信できるように HTTP エンドポイントを構成しましょう。  *./TestContainer/azurevotefrontPkg/ServiceManifest.xml* ファイルを開いて、**ServiceManifest** 要素にエンドポイント リソースを宣言します。  プロトコル、ポート、名前を追加します。 このチュートリアルでは、サービスがポート 80 でリッスンします。 
  
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
  
同様に、バックエンド サービスのサービス マニフェストを変更します。 このチュートリアルでは、Redis の既定値である 6379 を使用します。
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotebackTypeEndpoint" UriScheme="http" Port="6379" Protocol="http"/>
  </Endpoints>
</Resources>
```
**UriScheme** を指定すると、Service Fabric ネーム サービスを使用したコンテナー エンドポイントが自動的に登録され、検出可能性が確保されます。 バックエンド サービスの ServiceManifest.xml の完全なサンプル ファイルは、この記事の最後に示されています。 

### <a name="map-container-ports-to-a-service"></a>サービスにコンテナー ポートをマップする
    
クラスター内のコンテナーを公開するには applicationmanifest.xml にポートのバインドも作成する必要があります。 **PortBinding** ポリシーは、**ServiceManifest.xml** で定義した**エンドポイント**を参照します。 これらのエンドポイントへの受信要求は、ここで開いてバインドされたコンテナー ポートにマップされます。 **ApplicationManifest.xml** ファイルに次のコードを追加して、ポート 80 およびポート 6379 をエンドポイントにバインドします。 **ApplicationManifest.xml** の完全なサンプル ファイルは、このドキュメントの最後にあります。 
  
```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
</ContainerHostPolicies>
```

```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
</ContainerHostPolicies>
```

### <a name="add-a-dns-name-to-the-backend-service"></a>バックエンド サービスに DNS 名を追加する
  
Service Fabric がこの DNS 名をバックエンド サービスに割り当てるには、**ApplicationManifest.xml** に名前を指定する必要があります。 次のように **ServiceDnsName** 属性を**サービス**要素に追加します。 
  
```xml
<Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
  <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
    <SingletonPartition/>
  </StatelessService>
</Service>
```

フロントエンド サービスは環境変数を読み取り、Redis インスタンスの DNS 名を認識します。 環境変数は、Dockerfile で次のように定義されます。
  
```Dockerfile
ENV REDIS redisbackend.testapp
```
  
フロントエンドをレンダリングする Python スクリプトは、次のように DNS 名を使用して解決し、バックエンド Redis ストアへの接続を行います。

```python
# Get DNS Name
redis_server = os.environ['REDIS'] 

# Connect to the Redis store
r = redis.StrictRedis(host=redis_server, port=6379, db=0)
```

この時点で、サービス パッケージ アプリケーションのテンプレートをクラスターにデプロイすることができます。 次のチュートリアルでは、このアプリケーションをデプロイし、Service Fabric クラスターで実行します。

## <a name="create-a-service-fabric-cluster"></a>Service Fabric クラスターの作成
Azure 内のクラスターにアプリケーションをデプロイする場合、独自のクラスターかパーティー クラスターを使用します。

パーティー クラスターは、Azure でホストされている期間限定の無料 Service Fabric クラスターです。 Service Fabric チームによってサポートされており、誰でもアプリケーションをデプロイし、プラットフォームについて学ぶことができます。 パーティ クラスターにアクセスするには、[こちらの手順を実行します](http://aka.ms/tryservicefabric)。 

独自クラスターの作成については、「[Azure で初めての Service Fabric クラスターを作成する](service-fabric-get-started-azure-cluster.md)」を参照してください。

## <a name="build-and-deploy-the-application-to-the-cluster"></a>アプリケーションをクラスターにビルドおよびデプロイする
Service Fabric CLI を使用して、アプリケーションを Azure クラスターにデプロイできます。 Service Fabric CLI がマシンにインストールされていない場合は、[こちら](service-fabric-get-started-linux.md#set-up-the-service-fabric-cli)の指示に従ってインストールしてください。 

Azure の Service Fabric クラスターに接続します。

```bash
sfctl cluster select --endpoint http://lin4hjim3l4.westus.cloudapp.azure.com:19080
```

**TestContainer** ディレクトリに用意されているインストール スクリプトを使用してクラスターのイメージ ストアにアプリケーション パッケージをコピーし、アプリケーションの種類を登録して、アプリケーションのインスタンスを作成します。

```bash
./install.sh
```

ブラウザーを開いて、Service Fabric Explorer (http://lin4hjim3l4.westus.cloudapp.azure.com:19080/Explorer) に移動します。 アプリケーション ノードを展開し、アプリケーションの種類のエントリとインスタンスのエントリがあることを確認します。

![Service Fabric Explorer][sfx]

実行中のアプリケーションに接続するには、Web ブラウザーを開いてクラスターの URL に移動します (例: http://lin0823ryf2he.cloudapp.azure.com:80)。 Web UI に Voting アプリケーションが表示されます。

![Voting アプリケーション][votingapp]

## <a name="clean-up"></a>クリーンアップ
クラスターからアプリケーション インスタンスを削除し、その種類のアプリケーションの登録を解除するには、テンプレートに指定されているアンインストール スクリプトを使用します。 このコマンドは、インスタンスをクリーンアップするのに時間がかかるため、このスクリプトの直後に install.sh コマンドを実行することはできません。 

```bash
./uninstall.sh
```

## <a name="examples-of-completed-manifests"></a>完了したマニフェストの例

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ApplicationManifest ApplicationTypeName="TestContainerType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotefrontPkg" ServiceManifestVersion="1.0.0"/>
    <Policies> 
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
    </ContainerHostPolicies>
        </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotebackPkg" ServiceManifestVersion="1.0.0"/>
      <Policies> 
        <ContainerHostPolicies CodePackageRef="Code">
          <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
          <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
        </ContainerHostPolicies>
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="azurevotefront">
      <StatelessService ServiceTypeName="azurevotefrontType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
    <Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
      <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

### <a name="front-end-servicemanifestxml"></a>Front-end ServiceManifest.xml 
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotefrontPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotefrontType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>my.azurecr.io/azure-vote-front:v1</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="8080" Protocol="http"/>
    </Endpoints>
  </Resources>

 </ServiceManifest>
```

### <a name="redis-servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotebackPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotebackType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>my.azurecr.io/azure-vote-back:v1</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>
     <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="azurevotebackTypeEndpoint" UriScheme="http" Port="6379" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>
```
## <a name="next-steps"></a>次のステップ

このチュートリアルでは、Yeoman を使用して、複数のコンテナーを Service Fabric アプリケーションにパッケージ化しました。 その後、アプリケーションがデプロイされ、Service Fabric クラスターで実行されました。 次の手順を完了しました。

> [!div class="checklist"]
> * Yeoman のインストール  
> * Yeoman を使用したアプリケーション パッケージの作成
> * コンテナーで使用するアプリケーション パッケージの設定
> * アプリケーションのビルド  
> * アプリケーションのデプロイと実行 
> * アプリケーションのクリーンアップ

次のチュートリアルでは、Service Fabric のアプリケーションのフェールオーバーおよびスケーリングについて学習します。

> [!div class="nextstepaction"]
> [アプリケーションのフェールオーバーおよびスケーリングについて説明します](service-fabric-tutorial-containers-failover.md)

[votingapp]: ./media/service-fabric-tutorial-deploy-run-containers/votingapp.png
[sfx]: ./media/service-fabric-tutorial-deploy-run-containers/containerspackagetutorialsfx.png



