---
title: "Azure Stack でのカスタム Marketplace アイテムの発行 (クラウド オペレーター) | Microsoft Docs"
description: "クラウド オペレーターとして、Azure Stack でカスタム Marketplace アイテムを発行する方法について説明します。"
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 60871cbb-eed2-433c-a76d-d605c7aec06c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: erikje
ms.translationtype: HT
ms.sourcegitcommit: 5b6c261c3439e33f4d16750e73618c72db4bcd7d
ms.openlocfilehash: be61e746d97fbce166f44262fcc33e2f7118fd82
ms.contentlocale: ja-jp
ms.lasthandoff: 09/15/2017

---
# <a name="the-azure-stack-marketplace-overview"></a>Azure Stack Marketplace の概要
Marketplace は、ネットワーク、仮想マシン、ストレージなどの Azure Stack 向けにカスタマイズされたサービス、アプリケーション、リソースのコレクションです。 ユーザーはここで、新しいリソースを作成して、新しいアプリケーションをデプロイします。 ユーザーが使用する商品を参照し、選択できるショッピング カタログと考えてください。 ユーザーが Marketplace アイテムを使用するには、アイテムへのアクセスが許可されるサービスにサブスクライブする必要があります。

クラウド オペレーターとして、Marketplace に追加 (公開) するアイテムを決定します。 データベース、App Services などを公開できます。 これにより、すべてのユーザーに対してそれらが表示されます。 作成したカスタム アイテムを公開できます。 拡大を続ける [Azure Marketplace アイテムの一覧](azure-stack-marketplace-azure-items.md)からアイテムを公開することもできます。 Marketplace にアイテムを公開すると、5 分以内にユーザーがそれらを表示できます。

Marketplace を開くには、 **[新規]**をクリックします。

![](media/azure-stack-publish-custom-marketplace-item/image1.png)

## <a name="marketplace-items"></a>Marketplace アイテム
Azure Stack Marketplace の項目は、ユーザーがダウンロードおよび使用できるサービス、アプリケーション、またはリソースです。 すべての Azure Stack Marketplace のアイテムは、すべてのユーザーに表示されます。

各 Marketplace アイテムには以下のものが含まれます。

* リソース プロビジョニングの Azure リソース マネージャー テンプレート
* 文字列、アイコン、その他のマーケティング資料などのメタデータ
* ポータルでアイテムを表示するための書式設定情報

Marketplace に発行されるすべてのアイテムでは、Azure ギャラリー パッケージ (azpkg) と呼ばれる形式を使用します。 デプロイまたは実行時リソース (コード、ソフトウェアの zip ファイル、または仮想マシン イメージなど) を Marketplace アイテムの一部ではなく、Azure Stack に個別に追加します。 

## <a name="next-steps"></a>次のステップ
[Marketplace アイテムの作成と発行](azure-stack-create-and-publish-marketplace-item.md)


