---
title: "Azure Site Recovery を使用した Azure への VMware VM のレプリケーションのためのレプリケーション ポリシーを設定する | Microsoft Docs"
description: "VMware VM を Azure Storage にレプリケートする際にレプリケーション ポリシーを設定するために必要な手順の概要を示します"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 3c4b7ad16e6a03fb605447def18f7475d502fdd1
ms.contentlocale: ja-jp
ms.lasthandoff: 06/29/2017


---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-to-azure"></a>手順 9: Azure に VMware VM をレプリケートするためのレプリケーション ポリシーを設定する


この記事では、Azure Portal の [Azure Site Recovery](site-recovery-overview.md) サービスを使用して Azure に VMware VM をレプリケートする場合にレプリケーション ポリシーを設定する方法について説明します。


コメントや質問はこの記事の末尾、または [Azure Recovery Services フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)に投稿してください。


## <a name="configure-a-policy"></a>ポリシーを構成する

始める前に、簡単なビデオ概要を見ます。
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. 新しいレプリケーション ポリシーを作成するには、**[Site Recovery インフラストラクチャ]** > **[レプリケーション ポリシー]** > **[+ レプリケーション ポリシー]** の順にクリックします。
2. **[レプリケーション ポリシーの作成]** で、ポリシー名を指定します。
3. **[RPO しきい値]** で、RPO の制限を指定します。 この値で、データの復旧ポイントを作成する頻度を指定します。 継続的なレプリケーションがこの制限を超えると、アラートが生成されます。
4. **[復旧ポイントの保持期間]** で、各復旧ポイントのリテンション期間の長さ (時間単位) を指定します。 レプリケートされた VM は、期間内の任意の時点に復旧できます。 Premium Storage にレプリケートされたマシンでは最大 24 時間のリテンション期間がサポートされ、Standard Storage の場合は 72 時間です。
5. **[アプリ整合性スナップショットの頻度]**で、アプリケーション整合性スナップショットを含む復旧ポイントの作成頻度 (分単位) を指定します。 **[OK]** をクリックしてポリシーを作成します。

    ![Replication policy](./media/vmware-walkthrough-replication/gs-replication2.png)
8. 新しいポリシーを作成すると、自動的に構成サーバーに関連付けられます。 既定でフェールバックの照合ポリシーが自動的に作成されます。 たとえば、レプリケーション ポリシーが **rep-policy** の場合、フェールバック ポリシーは **rep-policy-failback** になります。 このポリシーは、Azure からフェールバックを開始するまで使用されません。

## <a name="next-steps"></a>次のステップ

「[手順 10: モビリティ サービスをインストールする](vmware-walkthrough-install-mobility.md)」に進む

