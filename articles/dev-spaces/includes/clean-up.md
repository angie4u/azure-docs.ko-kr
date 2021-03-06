---
title: 포함 파일
description: 포함 파일
ms.custom: include file
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 05/11/2018
ms.topic: include
manager: douge
ms.openlocfilehash: b8b8b1a593328197c61f8edba9f8f849518dd2e5
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/15/2018
ms.locfileid: "40129115"
---
## <a name="clean-up"></a>정리
모든 개발 환경 및 그 안에서 실행되는 서비스를 포함하여 클러스터에서 Azure Dev Spaces 인스턴스를 완전히 삭제하려면 `az aks remove-dev-spaces` 명령을 사용합니다. 이 작업은 되돌릴 수 없습니다. 클러스터에서 Azure Dev Spaces에 대한 지원을 추가할 수 있지만 마치 다시 시작하는 것 같을 것입니다. 이전 서비스와 공간을 복원할 수 없습니다.

다음 예제에서는 활성 구독에 Azure Dev Spaces 컨트롤러를 나열한 다음, 리소스 그룹 'myaks-rg'에서 AKS 클러스터 'myaks'와 연결되는 Azure Dev Spaces 컨트롤러를 삭제합니다.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```

