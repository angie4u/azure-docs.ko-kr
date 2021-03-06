---
title: Azure 관리자 구독 역할 추가 또는 변경 | Microsoft Docs
description: Azure 공동 관리자, 서비스 관리자 및 계정 관리자를 추가 또는 변경하는 방법에 대해 설명합니다.
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: genli
ms.openlocfilehash: 97210c0d9dba9c4130b1da9ad17a257ff1d81b42
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449229"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Azure 구독 관리자 추가 또는 변경

Azure 리소스에 대한 액세스를 관리하려면 적절한 관리자 역할이 있어야 합니다. 이 문서에서는 구독 수준에서 사용자에 대한 관리자 역할을 추가하거나 변경하는 방법에 대해 설명합니다.

## <a name="what-administrator-role-do-i-use"></a>어떤 관리자 역할을 사용하나요?

Azure에는 다양한 역할이 있습니다. 리소스에 대한 액세스를 관리하려면 클래식 구독 관리자 역할(예: 서비스 관리자 및 공동 관리자) 또는 RBAC(역할 기반 액세스 제어)라는 최신 권한 부여 시스템을 사용할 수 있습니다. 보다 효율적으로 제어하고 액세스 관리를 간소화하려면 모든 액세스 관리 요구 사항에 RBAC를 사용하는 것이 좋습니다. 가능한 경우 RBAC를 사용하여 기존 액세스 정책을 다시 구성하는 것이 좋습니다. 자세한 내용은 [RBAC(역할 기반 액세스 제어)란?](../role-based-access-control/overview.md) 및 [Azure의 다른 역할 이해](../role-based-access-control/rbac-and-directory-admin-roles.md)를 참조하세요.

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Azure Portal에서 구독에 대한 RBAC 소유자 추가 

Azure 구독에 대한 관리자로 다른 사용자를 추가하려면 구독 범위에서 [소유자](../role-based-access-control/built-in-roles.md#owner) 역할(RBAC 역할)을 할당합니다. 소유자 역할은 할당한 구독에서 리소스를 관리할 수 있으며 다른 구독에 대한 액세스 권한이 없습니다.

1. [Azure Portal에서 **구독**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)에 방문하세요.
2. 액세스 권한을 부여하려는 구독을 선택합니다.
3. **추가**를 선택합니다.
   (추가 단추가 없으면 사용 권한을 추가할 수 있는 권한이 없는 것입니다.)
4. 목록에서 **액세스 제어(IAM)** 를 선택합니다.
5. **역할**상자에서 **소유자**를 선택합니다. 
6. **다음에 대한 액세스 할당** 상자에서 **Azure AD 사용자, 그룹 또는 응용 프로그램**을 선택합니다. 
7. **선택** 상자에서 소유자로 추가할 사용자의 메일 주소를 입력합니다. 사용자를 선택한 다음 **저장**을 선택합니다.

    ![선택한 소유자 역할을 보여 주는 스크린샷](./media/billing-add-change-azure-subscription-administrator/add-role.png)

이렇게 하면 다른 사용자에게 액세스를 위임할 수 있는 권한을 비롯한 리소스에 대한 모든 권한을 사용자에게 부여할 수 있습니다. 리소스 그룹과 같이 다른 범위의 액세스 권한을 부여하려면 해당 범위에 대한 **액세스 제어(IAM)** 블레이드를 방문하세요.

## <a name="add-or-change-co-administrator"></a>공동 관리자 추가 또는 변경

[소유자](../role-based-access-control/built-in-roles.md#owner)만 공동 관리자로 추가할 수 있습니다. [기여자](../role-based-access-control/built-in-roles.md#contributor) 및 [읽기 권한자](../role-based-access-control/built-in-roles.md#reader)와 같은 역할이 있는 다른 사용자는 공동 관리자로 추가할 수 없습니다.

> [!TIP]
> 사용자가 Azure 클래식 배포를 관리해야 하는 경우 소유자만 공동 관리자로 추가하면 됩니다. 다른 모든 용도로 RBAC를 사용하는 것이 좋습니다.

1. 아직 하지 않는 경우 위의 지침에 따라 소유자로 사용자를 추가합니다.
2. 방금 추가한 소유자 사용자를 **마우스 오른쪽 단추로 클릭**한 다음 **공동 관리자로 추가**를 선택합니다. **공동 관리자로 추가** 옵션이 표시되지 않으면 페이지를 새로 고치거나 다른 인터넷 브라우저를 사용해 보세요. 

    ![공동 관리자를 추가하는 스크린샷](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    공동 관리자 권한을 제거하려면 공동 관리자 사용자를 **마우스 오른쪽 단추로 클릭**한 다음, **공동 관리자 제거**를 선택합니다.

    ![공동 관리자를 제거하는 스크린샷](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Azure 구독에 대한 서비스 관리자 변경

계정 관리자만 구독에 대한 서비스 관리자를 변경할 수 있습니다. 기본적으로 등록할 때 서비스 관리자는 계정 관리자와 동일합니다. 서비스 관리자가 다른 사용자로 변경되면 계정 관리자는 Azure Portal에 대한 액세스 권한을 상실하게 됩니다. 그러나 계정 관리자는 계정 센터를 사용하여 언제든지 서비스 관리자를 다시 자신으로 변경할 수 있습니다.

1. [서비스 관리자 변경에 대한 제한](#limits)을 확인하여 시나리오가 지원되는지 확인합니다.
1. [계정 센터](https://account.windowsazure.com/subscriptions)에 계정 관리자 권한으로 로그인합니다.
1. 구독을 선택합니다.
1. 오른쪽에서 **구독 세부 사항 편집**을 선택합니다.

    ![계정 센터에서 구독 편집 단추를 보여 주는 스크린샷](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. **서비스 관리자** 상자에서 새 서비스 관리자의 메일 주소를 입력합니다.

    ![서비스 관리자 전자 메일을 변경하는 상자를 보여 주는 스크린샷](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>서비스 관리자 변경에 대한 제한 사항

* 각 구독은 Azure AD 디렉터리와 연결됩니다. 구독이 연결된 디렉터리를 찾으려면 [**구독**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)으로 이동한 다음 구독을 선택하여 해당 디렉터리를 확인합니다.
* 회사 또는 학교 계정으로 로그인하는 경우 해당 조직의 다른 계정을 서비스 관리자로 추가할 수 있습니다. 예를 들어 abby@contoso.com은 bob@contoso.com을 서비스 관리자로 추가할 수 있지만 john@notcontoso.com이 contoso.com 디렉터리에 있지 않으면 john@notcontoso.com은 추가할 수 없습니다. 회사 또는 학교 계정을 사용하여 로그인한 사용자는 Microsoft 계정 사용자를 서비스 관리자로 계속해서 추가할 수 있습니다.

  | 로그인 방법 | Microsoft 계정 사용자를 서비스 관리자로 추가하는가요? | 서비스 관리자와 동일한 조직에 회사 또는 학교 계정을 추가하는가요? | 다른 조직의 회사 또는 학교 계정을 서비스 관리자로 추가하는가요? |
  | --- | --- | --- | --- |
  |  Microsoft 계정 |예 |아니오 |아니오 |
  |  회사 또는 학교 계정 |예 |예 |아니오 |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Azure 구독에 대한 계정 관리자 변경

계정 관리자는 초기에 Azure 구독에 등록한 사용자이며 구독의 청구 소유자로서 책임이 있습니다. 구독의 계정 관리자를 변경하려면 [다른 계정에 Azure 구독의 소유권 이전](billing-subscription-transfer.md)을 참조하세요.

<a name="check-the-account-administrator-of-the-subscription"></a>

**계정 관리자가 누구인지 확실하지 않은 경우?** 다음 단계를 수행하세요.

1. [Azure Portal에서 **구독**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)에 방문하세요.
1. 확인하려는 구독을 선택한 다음 **설정**에서 확인합니다.
1. **속성**을 선택합니다. 구독의 계정 관리자는 **계정 관리자** 상자에 표시됩니다.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>리소스 액세스 제어 및 Active Directory에 대해 자세히 알아보기

* RBAC에 대한 자세한 내용은 [RBAC(역할 기반 액세스 제어)란?](../role-based-access-control/overview.md)을 참조하세요.
* Azure의 모든 역할에 대한 자세한 내용은 [Azure의 다른 역할 이해](../role-based-access-control/rbac-and-directory-admin-roles.md)를 참조하세요.
* Azure Active Directory에 대한 자세한 내용은 [Azure 구독을 Azure Active Directory에 연결하는 방법](../active-directory/active-directory-how-subscriptions-associated-directory.md) 및 [Azure Active Directory에서 관리자 역할 할당](../active-directory/users-groups-roles/directory-assign-admin-roles.md)을 참조하세요.

## <a name="need-help-contact-support"></a>도움 필요 시 지원에 문의

추가 도움이 필요한 경우 [지원에 문의](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)하여 문제를 신속하게 해결하세요.
