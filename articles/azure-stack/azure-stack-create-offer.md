---
title: "Azure Stack でのオファーの作成 | Microsoft Docs"
description: "クラウド管理者が Azure Stack でテナントのオファーを作成する方法について説明します。"
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: erikje
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: 3d7360a1fb1c0cf42d77b3f39bf92c30438c2e01
ms.contentlocale: ja-jp
ms.lasthandoff: 09/15/2017

---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack でのオファーの作成
[オファー](azure-stack-key-features.md)は、プロバイダーから提示されてテナントが購入またはサブスクライブする 1 つまたは複数のプランをグループ化したものです。 ここでは、最後の手順で[作成したプラン](azure-stack-create-plan.md)を含むオファーを作成する方法について説明します。 このオファーのサブスクライバーは仮想マシンをプロビジョニングすることができます。

1. Azure Stack 管理者ポータルに (https://adminportal.local.azurestack.external) にサインインし、**[新規]** > **[テナントのプラン]** > **[オファー]** をクリックします。

   ![](media/azure-stack-create-offer/image01.png)
2. **[新しいプラン]** ブレードの **[表示名]** と **[リソース名]** に入力し、新規または既存の **[リソース グループ]** を選択します。 [表示名] はオファーのフレンドリ名であり、ユーザーがサブスクライブするときに表示されるオファーに関する唯一の情報です。 そのため、ユーザーがオファーで提供される内容を理解できるように、わかりやすい名前を付けてください。 リソース名は、管理者のみが表示できます。 この名前を使用して、管理者はオファーを Azure リソース マネージャーのリソースとして操作します。

   ![](media/azure-stack-create-offer/image01a.png)
3. **[基本プラン]** をクリックし、**[プラン]** ブレードで、オファーに含めるプランを選択し、**[選択]** をクリックします。 **[作成]** をクリックしてオファーを作成します。

   ![](media/azure-stack-create-offer/image02.png)
4. **[すべてのリソース]** をクリックし、新しいオファーを検索して新しいオファーをクリックします。**[状態の変更]** をクリックし、**[パブリック]** をクリックします。

   ![](media/azure-stack-create-offer/image03.png)

サブスクライブ時にテナントが完全なビューを取得するために、オファーはパブリックにする必要があります。 オファーの状態:

* **パブリック**: テナントに表示されます。
* **プライベート**: クラウド管理者にのみ表示されます。 プランまたはオファーの下書き作成や、クラウド管理者がすべてのサブスクリプションを承認する必要がある場合に便利です。
* **使用停止**: 新しいサブスクライバーに公開されません。 クラウド管理者は [使用停止] を使って、既存のサブスクライバーを変更することなく、以降のサブスクリプションを回避することができます。

オファーへの変更は、テナントにすぐに表示されません。 変更を表示するには、新しいリソース/リソース グループを作成するときにログアウト/ログインして、"サブスクリプションの選択" に新しいサブスクリプションを表示する必要があります。

> [!NOTE]
>また、[Azure Stack Service Administrator の readme](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin) の説明に従って、PowerShell を使用して既定のオファー、プラン、クォータを作成することもできます。
>


## <a name="next-steps"></a>次のステップ
[プランへのサブスクライブと VM のプロビジョニングに関するページ](azure-stack-subscribe-plan-provision-vm.md)

