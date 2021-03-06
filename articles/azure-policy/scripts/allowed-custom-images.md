---
title: Azure Policy 샘플 - 승인된 VM 이미지
description: 이 정책에서는 승인된 사용자 지정 이미지만 환경에 배포되어야 합니다.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 06/03/2018
ms.author: dacoulte
ms.custom: mvc
ms.openlocfilehash: 408968723ab9dc14a1db1321c4db3b3d8518d511
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36319302"
---
# <a name="approved-vm-images"></a>승인된 VM 이미지

이 정책에서는 승인된 사용자 지정 이미지만 환경에 배포되어야 합니다. 승인된 이미지 ID 배열을 지정합니다.

다음 방법을 사용하여 이 샘플 정책을 배포할 수 있습니다.

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [Azure CLI](#azure-cli)
- [REST API](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>샘플 정책

### <a name="policy-definition"></a>정책 정의 

완전히 구성된 JSON 정책 정의로 REST API에서, 'Azure에 배포' 단추에서, 그리고 포털에서 수동으로 사용됩니다.

[!code-json[full](../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.json "Complete policy definition (JSON)")]

> [!NOTE]
> 포털에서 수동으로 정책을 만드는 경우 위의 **properties.parameters** 및 **properties.policyRule** 부분을 사용합니다. 두 섹션을 중괄호 `{}`로 묶어서 유효한 JSON으로 만듭니다.

### <a name="policy-rules"></a>정책 규칙

정책 규칙을 정의하는 JSON으로, Azure CLI 및 Azure PowerShell에서 사용됩니다.

[!code-json[rule](../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>정책 매개 변수

정책 매개 변수를 정의하는 JSON으로, Azure CLI 및 Azure PowerShell에서 사용됩니다.

[!code-json[parameters](../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.parameters.json "Policy parameters (JSON)")]

## <a name="parameters"></a>매개 변수

|Name |type |필드 |설명 |
|---|---|---|---|
|imageIds |배열 |Microsoft.Compute/imageIds |승인된 VM 이미지 목록|

PowerShell 또는 Azure CLI를 통해 할당을 만드는 경우 `-PolicyParameter`(PowerShell) 또는 `--params`(Azure CLI) 명령을 사용하여 매개 변수 값을 문자열로 또는 파일을 통해 JSON으로 전달할 수 있습니다.
PowerShell은 cmdlet에 이름/값 해시 테이블을 전달해야 하는 `-PolicyParameterObject` 명령도 지원하는데, 여기서 **이름**은 매개 변수 이름이고 **값**은 할당 과정에 전달되는 단일 값 또는 값 배열입니다.

이 예제 매개 변수에서는 리소스 그룹 _YourResourceGroup_의 _ContosoStdImage_ 또는 '미국 중부'에 위치한 Windows Server 2016 데이터 센터의 2018년 5월 버전만 허용됩니다.

```json
{
    "imageIds": {
        "value": [
            "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
            "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
        ]
    }
}
```

## <a name="azure-portal"></a>Azure portal

[![Azure에 배포](../media/deploy/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fallowed-custom-images%2Fazurepolicy.json)
[![Azure Gov에 배포](../media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fallowed-custom-images%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

### <a name="deploy-with-azure-powershell"></a>Azure PowerShell을 사용하여 배포

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzureRmPolicyDefinition -Name 'allowed-custom-images' -DisplayName 'Approved VM images' -description 'This policy governs the approved VM images' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzureRmResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
$assignment = New-AzureRmPolicyAssignment -Name 'allowed-custom-images-assignment' -DisplayName 'Approved VM images Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyparam
```

### <a name="remove-with-azure-powershell"></a>Azure PowerShell을 사용하여 제거

이전 할당 및 정의를 제거하려면 다음 명령을 실행합니다.

```azurepowershell-interactive
# Remove the Policy Assignment
Remove-AzureRmPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzureRmPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Azure PowerShell 설명

배포 및 제거 스크립트는 다음 명령을 사용합니다. 다음 표에 있는 각 명령은 명령에 해당하는 문서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) | 새 Azure Policy 정의를 만듭니다. |
| [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/get-azurermresourcegroup) | 단일 리소스 그룹을 가져옵니다. |
| [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) | 새 Azure Policy 할당을 만듭니다. 이 예제에서는 정의를 제공하지만, 이니셔티브를 취할 수도 있습니다. |
| [Remove-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/remove-azurermpolicyassignment) | 기존 Azure Policy 할당을 제거합니다. |
| [Remove-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/remove-azurermpolicydefinition) | 기존 Azure Policy 정의를 제거합니다. |

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Azure CLI를 사용하여 배포

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'allowed-custom-images' --display-name 'Approved VM images' --description 'This policy governs the approved VM images' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'allowed-custom-images-assignment' --display-name 'Approved VM images Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
```

### <a name="remove-with-azure-cli"></a>Azure CLI를 사용하여 제거

이전 할당 및 정의를 제거하려면 다음 명령을 실행합니다.

```azurecli-interactive
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Azure CLI 설명

| 명령 | 메모 |
|---|---|
| [az policy definition create](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-create) | 새 Azure Policy 정의를 만듭니다. |
| [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) | 단일 리소스 그룹을 가져옵니다. |
| [az policy assignment create](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | 새 Azure Policy 할당을 만듭니다. 이 예제에서는 정의를 제공하지만, 이니셔티브를 취할 수도 있습니다. |
| [az policy assignment delete](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) | 기존 Azure Policy 할당을 제거합니다. |
| [az policy definition delete](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-delete) | 기존 Azure Policy 정의를 제거합니다. |

## <a name="rest-api"></a>REST API

[ARMClient](https://github.com/projectkudu/ARMClient) 또는 PowerShell처럼 Resource Manager REST API와 상호 작용하는 데 사용할 수 있는 여러 도구가 있습니다. PowerShell에서 REST API를 호출하는 예는 [정책 정의 구조](../policy-definition.md#aliases)의 **별칭** 섹션에서 확인할 수 있습니다.

### <a name="deploy-with-rest-api"></a>REST API를 사용하여 배포

- 정책 정의를 만들기(구독 범위) 요청 본문에 [정책 정의](#policy-definition) JSON 사용

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
  ```

- 정책 할당 만들기(리소스 그룹 범위)

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

  요청 본문에 다음 JSON 예제 사용:

  ```json
  {
      "properties": {
          "displayName": "Approved VM images Assignment",
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images",
          "parameters": {
              "imageIds": {
                  "value": [
                      "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
                      "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
                  ]
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>REST API를 사용하여 제거

- 정책 할당 제거

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

- 정책 정의 제거

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
  ```

### <a name="rest-api-explanation"></a>REST API 설명

| 서비스 | 그룹 | 작업 | 메모 |
|---|---|---|---|
| 리소스 관리 | 정책 정의 | [만들기](/rest/api/resources/policydefinitions/createorupdate) | 구독에서 새 Azure Policy 정의를 만듭니다. 대안: [관리 그룹 만들기](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| 리소스 관리 | 정책 할당 | [만들기](/rest/api/resources/policyassignments/create) | 새 Azure Policy 할당을 만듭니다. 이 예제에서는 정의를 제공하지만, 이니셔티브를 취할 수도 있습니다. |
| 리소스 관리 | 정책 할당 | [삭제](/rest/api/resources/policyassignments/delete) | 기존 Azure Policy 할당을 제거합니다. |
| 리소스 관리 | 정책 정의 | [삭제](/rest/api/resources/policydefinitions/delete) | 기존 Azure Policy 정의를 제거합니다. 대안: [관리 그룹 삭제](/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>다음 단계

- 추가 [Azure Policy 샘플](../json-samples.md) 검토
- [Azure Policy 정의 구조](../policy-definition.md) 검토
- [Windows VM에 정책 적용](../../virtual-machines/windows/policy.md)에서 Virtual Machines에 대한 추가 Azure Policy 예제 살펴보기