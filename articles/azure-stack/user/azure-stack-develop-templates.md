---
title: Azure Stack용 템플릿 개발 | Microsoft Docs
description: Azure Stack 템플릿 모범 사례 알아보기
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 8a5bc713-6f51-49c8-aeed-6ced0145e07b
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: d09dec2f327d8b5911a4e55832ba106838c7ebc3
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42139586"
---
# <a name="azure-resource-manager-template-considerations"></a>Azure Resource Manager 템플릿 고려 사항

*적용 대상: Azure Stack 통합 시스템 및 Azure Stack 개발 키트*

응용 프로그램을 개발할 때 Azure 및 Azure Stack 간에 템플릿 이식성을 보장하는 것이 중요합니다. 이 문서에서는 Azure Resource Manager를 개발 하기 위한 고려 사항 [템플릿](http://download.microsoft.com/download/E/A/4/EA4017B5-F2ED-449A-897E-BD92E42479CE/Getting_Started_With_Azure_Resource_Manager_white_paper_EN_US.pdf)이므로 Azure Stack 환경에 액세스 하지 않고도 Azure에서 응용 프로그램 및 테스트 배포 프로토타입 수 있습니다.

## <a name="resource-provider-availability"></a>리소스 공급자 가용성

서식 파일을 배포할 계획 이라면을 이미 제공 되거나 Azure Stack에서 미리 보기에 있는 Microsoft Azure 서비스를 사용만 해야 합니다.

## <a name="public-namespaces"></a>공용 네임스페이스

Azure Stack이 데이터 센터에서 호스트되므로 Azure 공용 클라우드와는 다른 서비스 엔드포인트 네임스페이스가 제공됩니다. 결과적으로, Azure Resource Manager 템플릿의 하드 코드 된 공용 끝점에는 Azure Stack에 배포 하려고 하면 실패 합니다. 사용 하 여 서비스 끝점을 동적으로 빌드할 수 있습니다는 *참조* 하 고 *연결* 배포 중 리소스 공급자에서 값을 검색 하는 함수입니다. 하드 코딩 하는 대신에 예를 들어 *blob.core.windows.net* 서식 파일을 검색 합니다 [primaryEndpoints.blob](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/101-simple-windows-vm/azuredeploy.json#L201) 동적으로 설정 합니다 *osDisk.URI* 끝점:

     "osDisk": {"name": "osdisk","vhd": {"uri":
     "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2015-06-15').primaryEndpoints.blob, variables('vmStorageAccountContainerName'),
      '/',variables('OSDiskName'),'.vhd')]"}}

## <a name="api-versioning"></a>API 버전 관리

Azure 서비스 버전이 Azure와 Azure Stack 간에 다를 수 있습니다. 각 리소스에 필요 합니다 **apiVersion** 제공 하는 기능을 정의 하는 특성입니다. Azure Stack에서 API 버전 호환성을 보장 하려면 다음과 같은 API 버전은 각 리소스 공급자에 대 한 유효 합니다.

| 리소스 공급자 | apiVersion |
| --- | --- |
| 컴퓨팅 |`'2015-06-15'` |
| 네트워크 |`'2015-06-15'`, `'2015-05-01-preview'` |
| Storage |`'2016-01-01'`, `'2015-06-15'`, `'2015-05-01-preview'` |
| KeyVault | `'2015-06-01'` |
| App Service |`'2015-08-01'` |

## <a name="template-functions"></a>템플릿 함수

Azure Resource Manager [함수](../../azure-resource-manager/resource-group-template-functions.md) 동적 템플릿을 빌드하는 데 필요한 기능을 제공 합니다. 예를 들어 다음과 같은 작업에 함수를 사용할 수 있습니다.

* 문자열 연결 또는 자르기 합니다.
* 다른 리소스에서 참조 값입니다.
* 리소스의 여러 인스턴스 배포를 반복 합니다.

이러한 함수는 Azure Stack에서 사용할 수 있습니다.

* Skip
* Take

## <a name="resource-location"></a>리소스 위치

Azure Resource Manager 템플릿을 배포 하는 동안 리소스를 배치 하는 위치 특성을 사용 합니다. Azure에서 위치는 미국 서부 또는 남아메리카와 같은 하위 지역을 나타냅니다. Azure Stack은 데이터 센터에 있기 때문에 위치가 다릅니다. Azure 및 Azure Stack 간에 템플릿을 전달할 수 있으려면 개별 리소스를 배포할 때 리소스 그룹 위치를 참조해야 합니다. 이렇게 하려면 사용 하 여 `[resourceGroup().Location]` 모든 리소스가 리소스 그룹 위치를 상속 하도록 합니다. 다음 발췌 구문 다음과 같습니다. 저장소 계정을 배포 하는 동안이 함수를 사용 하는 예제

    "resources": [
    {
      "name": "[variables('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "[variables('apiVersionStorage')]",
      "location": "[resourceGroup().location]",
      "comments": "This storage account is used to store the VM disks",
      "properties": {
      "accountType": "Standard_GRS"
      }
    }
    ]

## <a name="next-steps"></a>다음 단계

* [PowerShell을 사용하여 템플릿 배포](azure-stack-deploy-template-powershell.md)
* [Azure CLI을 사용하여 템플릿 배포](azure-stack-deploy-template-command-line.md)
* [Visual Studio를 사용하여 템플릿 배포](azure-stack-deploy-template-visual-studio.md)
