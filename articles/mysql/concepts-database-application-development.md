---
title: "Azure Database for MySQL のデータベース アプリケーション開発の概要 | Microsoft Docs"
description: "開発者が Azure Database for MySQL に接続するためのアプリケーション コードを記述するときに従う必要がある、設計上の考慮事項について説明します"
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 5f8bb1631ccd6dcfe1354e3b4c0897c587149b1d
ms.contentlocale: ja-jp
ms.lasthandoff: 05/10/2017

---

# <a name="application-development-overview-for-azure-database-for-mysql"></a>Azure Database for MySQL のアプリケーション開発の概要 
この記事では、開発者が Azure Database for MySQL に接続するためのアプリケーション コードを記述するときに従う必要がある、設計上の考慮事項について説明します 

> [!TIP]
> サーバーの作成、サーバーベースのファイアウォールの作成、サーバー プロパティの表示、データベースの作成、ワークベンチおよび mysql.exe を使用した接続とクエリの方法を示すチュートリアルについては、[最初の Azure MySQL データベースの設計](tutorial-design-database-using-portal.md)に関するページをご覧ください

## <a name="language-and-platform"></a>言語とプラットフォーム
さまざまなプログラミング言語とプラットフォームで利用できるコード サンプルがあります。 コード サンプルへのリンクについては、[Azure Database for MySQL への接続に使用する接続ライブラリ](concepts-connection-libraries.md)に関するページをご覧ください

## <a name="tools"></a>ツール
Azure Database for MySQL では、Workbench などの MySQL の一般的管理ツールと互換性のある MySQL コミュニティ バージョン、または mysql.exe、[phpMyAdmin](https://www.phpmyadmin.net/)、[Navicat](https://www.navicat.com/products/navicat-for-mysql) などの MySQL ユーティリティが使用されます。 Azure Portal、Azure CLI、および REST API を使用して、データベース サービスを操作することもできます。

## <a name="resource-limitations"></a>リソースの制限事項
Azure MySQL Database では、サーバーが使用できるリソースを、次の 2 つのメカニズムを使用して管理します。 
- リソース ガバナンス 
- 制限の適用。

## <a name="security"></a>セキュリティ
Azure MySQL Database には、MySQL Database に対するアクセスの制限、データの保護、ユーザーとロールの構成、およびアクティビティの監視を行うためのリソースが用意されています。

## <a name="authentication"></a>認証
Azure MySQL Database は、ユーザーとログインのサーバー認証をサポートしています。

## <a name="resiliency"></a>回復性
MySQL Database への接続中に一時エラーが発生した場合は、コードで呼び出しを再試行する必要があります。 再試行ロジックでは、複数のクライアントが同時に再試行することで SQL Database に過大な負荷がかかるのを防ぐために、バックオフ ロジックを使用することをお勧めします。

- コード サンプル: 再試行ロジックを示すコード サンプルについては、[Azure Database for MySQL への接続に使用する接続ライブラリ](concepts-connection-libraries.md)に関するページで、好みの言語用のサンプルをご覧ください

## <a name="managing-connections"></a>接続の管理
データベースの接続リソースは限られているため、MySQL Database にアクセスするときは、優れたパフォーマンスを実現できるよう適切に接続を使用することをお勧めします。
- 接続プールまたは永続的な接続を使用してデータベースにアクセスします。
- 有効期間が短い接続を使用してデータベースにアクセスします。 
- 接続を試行するときにアプリケーションで再試行ロジックを使用して、同時接続が最大許容値に達することにより発生するエラーをキャッチします。 再試行ロジックで短い遅延を設定し、ランダムな時間だけ待機してから、追加の接続を試行します。
