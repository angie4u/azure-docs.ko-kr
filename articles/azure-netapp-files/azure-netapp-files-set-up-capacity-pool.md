---
title: Azure NetApp Files에 대한 용량 풀 설정 | Microsoft Docs
description: 볼륨을 만들 수 있도록 용량 풀을 설정하는 방법을 설명합니다.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/28/2018
ms.author: b-juche
ms.openlocfilehash: 0e9203f5b4e2a9043e242b804c82017cf6fc3ee1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39010806"
---
# <a name="set-up-a-capacity-pool"></a>용량 풀 설정
용량 풀 설정을 통해 볼륨을 만들 수 있습니다.  

## <a name="before-you-begin"></a>시작하기 전에 
NetApp 계정을 만들어야 합니다.   

[NetApp 계정 만들기](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>단계 

1. NetApp 계정의 관리 블레이드로 이동한 다음, 탐색 창에서 **용량 풀**을 선택합니다.

2. **+ 풀 추가**를 클릭하여 새 용량 풀을 만듭니다.   
    새 용량 풀 창이 나타납니다.

3. 새 용량 풀에 대한 다음과 같은 정보를 제공합니다.  
  * **Name**  
    용량 풀의 이름을 지정합니다.  
    용량 풀 이름은 각 NetApp 계정에 대해 고유해야 합니다.

  * **서비스 수준**   
    이 필드는 용량 풀에 대한 대상 성능을 보여줍니다.  
    현재 프리미엄 서비스 수준만 제공됩니다. 

  *  **크기**     
      구입하려는 용량 풀의 크기를 지정합니다.        
      최소 용량 풀 크기는 4TiB입니다. 4TiB의 배수인 크기로 풀을 만들 수 있습니다.   
      
      ![새 용량 풀](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. **확인**을 클릭합니다.

## <a name="next-steps"></a>다음 단계 

1. [Azure NetApp Files에 대한 볼륨 만들기](azure-netapp-files-create-volumes.md)
2. [볼륨에 대한 내보내기 정책 구성(선택 사항)](azure-netapp-files-configure-export-policy.md)

