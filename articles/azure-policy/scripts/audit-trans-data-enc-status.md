---
title: Azure Policy json 샘플 ‑ 투명한 데이터 암호화 상태 감사 | Microsoft Docs
description: 이 json 샘플 정책에서는 Microsoft Azure SQL Database 투명한 데이터 암호화를 사용하도록 설정되지 않은 경우 감사합니다.
services: azure-policy
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-policy
ms.devlang: ''
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: ''
ms.date: 10/30/2017
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 0c81226074962316547f43c3e75beb17a4c0b0f0
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602281"
---
# <a name="audit-transparent-data-encryption-status"></a>투명한 데이터 암호화 상태 감사

이 정책에서는 Microsoft Azure SQL Database 투명한 데이터 암호화를 사용하도록 설정되지 않은 경우 감사합니다.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-template"></a>샘플 템플릿

[!code-json[main](../../../policy-templates/samples/SQL/audit-sql-db-tde-status/azurepolicy.json "Audit transparent data encryption status")]

[PowerShell](#deploy-with-powershell) 또는 [Azure CLI](#deploy-with-azure-cli)와 함께 [Azure Portal](#deploy-with-the-portal)을 사용하여 이 템플릿을 배포할 수 있습니다.

## <a name="deploy-with-the-portal"></a>포털을 사용하여 배포

[![Azure에 배포](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?feature.customportal=false&microsoft_azure_policy=true&microsoft_azure_policy_policyinsights=true&feature.microsoft_azure_security_policy=true&microsoft_azure_marketplace_policy=true#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FSQL%2Faudit-sql-db-tde-status%2Fazurepolicy.json)

## <a name="deploy-with-powershell"></a>PowerShell을 사용하여 배포 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

```powershell
$definition = New-AzureRmPolicyDefinition -Name "audit-sql-db-tde-status" -DisplayName "Audit transparent data encryption status" -description "Audit transparent data encryption status for SQL databases" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-tde-status/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-tde-status/azurepolicy.parameters.json' -Mode All
$definition
$assignment = New-AzureRMPolicyAssignment -Name <assignmentname> -Scope <scope>  -PolicyDefinition $definition
$assignment
```

### <a name="clean-up-powershell-deployment"></a>PowerShell 배포 정리

다음 명령을 실행하여 리소스 그룹, VM 및 모든 관련된 리소스를 제거할 수 있습니다.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="deploy-with-azure-cli"></a>Azure CLI를 사용하여 배포

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

```azurecli-interactive
az policy definition create --name 'audit-sql-db-tde-status' --display-name 'Audit transparent data encryption status' --description 'Audit transparent data encryption status for SQL databases' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-tde-status/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/SQL/audit-sql-db-tde-status/azurepolicy.parameters.json' --mode All

az policy assignment create --name <assignmentname> --scope <scope> --policy "audit-sql-db-tde-status"
```

### <a name="clean-up-azure-cli-deployment"></a>Azure CLI 배포 정리

다음 명령을 실행하여 리소스 그룹, VM 및 모든 관련된 리소스를 제거할 수 있습니다.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>다음 단계

- [Azure Policy 샘플](../json-samples.md)에서 더 많은 예제를 검토합니다.