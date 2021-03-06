---
title: "アクセス レビューを実行する方法 | Microsoft Docs"
description: "Azure Privileged Identity Management アプリケーションでレビューを実行する方法について説明します。"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 49ee2feb-7d2e-4acf-82c1-40ff23062862
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/06/2017
ms.author: billmath
ms.custom: pim
ms.translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5b5dabe301fae4000112aa467a22cc5c37b9b592
ms.contentlocale: ja-jp
ms.lasthandoff: 12/29/2016

---
# <a name="how-to-perform-an-access-review-in-azure-ad-privileged-identity-management"></a>Azure AD Privileged Identity Management でアクセス レビューを実行する方法
Azure Active Directory (AD) Privileged Identity Management を使用すると、企業における Azure AD や他の Microsoft オンライン サービス (Office 365 や Microsoft Intune など) のリソースへの特権アクセスの管理が簡略化されます。  

既に管理者ロールに割り当てられているユーザーは、組織の特権ロール管理者から、自分の業務にそのロールがまだ必要であるかどうかを定期的に確認するよう求められることがあります。 リンクが記載された電子メールが届く場合もあれば、直接 [Azure Portal](https://portal.azure.com)にアクセスすることもできます。 この記事に記載された手順に従って、割り当てられたロールの自己レビューを実行することができます。

特権ロール管理者としてアクセス レビューに関心がある場合は、 [アクセス レビューを開始する方法](active-directory-privileged-identity-management-how-to-start-security-review.md)に関するページで詳細を確認できます。

## <a name="add-the-privileged-identity-management-application"></a>Privileged Identity Management アプリケーションの追加
[Azure Portal](https://portal.azure.com/) で Azure AD Privileged Identity Management (PIM) アプリケーションを使用して、レビューを実行できます。  ポータルに Azure AD Privileged Identity Management アプリケーションがない場合は、まず、次の手順を実行してください。

1. [Azure Portal](https://portal.azure.com/) にサインインします。
2. Azure Portal の右上隅に表示されているユーザー名をクリックし、操作するディレクトリを選択します。
3. **[その他のサービス]** を選択し、[フィルター] ボックスを使用して "**Azure AD Privileged Identity Management**" を検索します。
4. **[ダッシュボードにピン留めする]** チェック ボックスをオンにし、**[作成]** をクリックします。 Privileged Identity Management アプリケーションが起動します。

## <a name="approve-or-deny-access"></a>アクセスの承認または拒否
アクセスを承認または拒否する場合、このロールをまだ使用するかどうかをレビュー担当者にのみ通知します。 ロールにとどまる場合は **[承認]** を選択します。また、アクセスが不要な場合は **[拒否]** を選択します。 レビュー担当者が結果を適用するまで、状態はすぐに変更されません。
アクセス レビューを検索して完了するには、次の手順に従います。

1. PIM アプリケーションで、 **[特権アクセスのレビュー]**を選択します。 保留中のアクセス レビューがある場合は、[Azure AD Access reviews (Azure AD Access レビュー)] ブレードが表示されます。
2. 完了するレビューを選択します。
3. レビューを作成した場合を除き、レビューで唯一のユーザーとして表示されます。 名前の横にあるチェック マークを選択します。
4. **[承認]** または **[拒否]** のいずれかを選択します。 **[理由の提供]** テキスト ボックスで決定の理由を含めることが必要になる場合があります。  
5. **[Azure AD ロールのレビュー]** ブレードを閉じます。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>次のステップ
[!INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png

