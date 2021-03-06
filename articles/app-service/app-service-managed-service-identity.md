---
title: App Service 및 Azure Functions의 Managed Service Identity | Microsoft Docs
description: Azure App Service 및 Azure Functions에서 Managed Service Identity를 지원하기 위한 개념 참조 및 설치 가이드
services: app-service
author: mattchenderson
manager: cfowler
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/25/2018
ms.author: mahender
ms.openlocfilehash: 2e392a3a50cda3daacb5bc358baaea2627eeafc0
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39578833"
---
# <a name="how-to-use-azure-managed-service-identity-in-app-service-and-azure-functions"></a>App Service 및 Azure Functions에서 Azure 관리 서비스 ID를 사용하는 방법

> [!NOTE] 
> Linux의 App Service 및 Web App for Containers는 현재 관리 서비스 ID를 지원되지 않습니다.

> [!Important] 
> 앱이 구독/테넌트 간에 마이그레이션되는 경우 App Service 및 Azure Functions에 대한 관리 서비스 ID가 예상대로 작동하지 않습니다. 앱에서 새 ID를 확보해야 하며 해당 기능을 사용 중지했다가 다시 사용하도록 설정하여 확보할 수 있습니다. 아래 [ID 제거](#remove)를 참조하세요. 다운스트림 리소스에도 새 ID를 사용하려면 액세스 정책을 업데이트해야 합니다.

이 토픽에서는 App Service 및 Azure Functions 응용 프로그램에 대한 관리되는 앱 ID를 만들어서 다른 리소스에 액세스하는 데 사용하는 방법을 보여줍니다. Azure Active Directory의 관리되는 서비스 ID를 사용하면 앱이 Azure Key Vault처럼 AAD로 보호되는 다른 리소스에 쉽게 액세스할 수 있습니다. ID는 Azure 플랫폼에서 관리하며 비밀을 프로비전하거나 회전할 필요가 없습니다. Managed Service Identity에 대한 자세한 내용은 [Managed Service Identity 개요](../active-directory/managed-service-identity/overview.md)를 참조하세요.

## <a name="creating-an-app-with-an-identity"></a>ID를 사용하여 앱 만들기

ID를 사용하여 앱을 만들려면 응용 프로그램에서 추가 속성을 설정해야 합니다.

### <a name="using-the-azure-portal"></a>Azure Portal 사용

포털에서 관리되는 서비스 id를 설정하려면 먼저 평소와 같은 방법으로 응용 프로그램을 만든 후 기능을 설정합니다.

1. 평소처럼 포털에서 앱을 만듭니다. 포털에서 해당 앱으로 이동합니다.

2. 함수 앱을 사용하는 경우 **플랫폼 기능**으로 이동합니다. 다른 유형의 앱을 사용하는 경우 왼쪽 탐색 창에서 **설정** 그룹이 나올 때까지 아래로 스크롤합니다.

3. **관리되는 서비스 ID**를 선택합니다.

4. **Azure Active Directory**을 **켜기**로 전환합니다. **저장**을 클릭합니다.

![App Service의 Managed Service Identity](media/app-service-managed-service-identity/msi-blade.png)

### <a name="using-the-azure-cli"></a>Azure CLI 사용

Azure CLI를 사용하여 관리되는 서비스 ID를 설정하려면 기존 응용 프로그램에 대해 `az webapp identity assign` 명령을 사용해야 합니다. 이 섹션의 예제를 실행하는 옵션은 세 가지가 있습니다.

- Azure Portal에서 [Azure Cloud Shell](../cloud-shell/overview.md)을 사용합니다.
- 아래 각 코드 블록의 오른쪽 위에 있는 "사용해 보세요" 단추를 통해 포함된 Azure Cloud Shell을 사용합니다.
- 로컬 CLI 콘솔을 사용하려는 경우 [CLI 2.0의 최신 버전(2.0.31 이상)을 설치](https://docs.microsoft.com/cli/azure/install-azure-cli)합니다. 

다음 단계는 웹앱을 만들고 CLI를 사용하여 ID를 할당하는 과정을 안내합니다.

1. Azure CLI를 로컬 콘솔에서 사용하는 경우 [az login](/cli/azure/reference-index#az-login)을 사용하여 먼저 Azure에 로그인합니다. 응용 프로그램을 배포하려는 Azure 구독과 연결된 계정을 사용합니다.

    ```azurecli-interactive
    az login
    ```
2. CLI를 사용하여 웹앱을 만듭니다. App Service에서 CLI를 사용하는 방법에 대한 예제는 [App Service CLI 샘플](../app-service/app-service-cli-samples.md)을 참조하세요.

    ```azurecli-interactive
    az group create --name myResourceGroup --location westus
    az appservice plan create --name myPlan --resource-group myResourceGroup --sku S1
    az webapp create --name myApp --resource-group myResourceGroup --plan myPlan
    ```

3. 이 응용 프로그램에 대한 ID를 만들려면 `identity assign` 명령을 실행합니다.

    ```azurecli-interactive
    az webapp identity assign --name myApp --resource-group myResourceGroup
    ```

### <a name="using-azure-powershell"></a>Azure PowerShell 사용

다음 단계는 웹앱을 만들고 Azure PowerShell을 사용하여 ID를 할당하는 과정을 안내합니다.

1. 필요한 경우 [Azure PowerShell 가이드](/powershell/azure/overview)에 있는 지침을 사용하여 Azure PowerShell을 설치한 다음, `Login-AzureRmAccount`를 실행하여 Azure에 연결합니다.

2. Azure PowerShell을 사용하여 웹앱을 만듭니다. App Service에서 Azure PowerShell을 사용하는 방법에 대한 예제는 [App Service PowerShell 샘플](../app-service/app-service-powershell-samples.md)을 참조하세요.

    ```azurepowershell-interactive
    # Create a resource group.
    New-AzureRmResourceGroup -Name myResourceGroup -Location $location
    
    # Create an App Service plan in Free tier.
    New-AzureRmAppServicePlan -Name $webappname -Location $location -ResourceGroupName myResourceGroup -Tier Free
    
    # Create a web app.
    New-AzureRmWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName myResourceGroup
    ```

3. 이 응용 프로그램에 대한 ID를 만들려면 `identity assign` 명령을 실행합니다.

    ```azurepowershell-interactive
    Set-AzureRmWebApp -AssignIdentity $true -Name $webappname -ResourceGroupName myResourceGroup 
    ```

### <a name="using-an-azure-resource-manager-template"></a>Azure Resource Manager 템플릿 사용

Azure Resource Manager 템플릿을 사용하여 Azure 리소스 배포를 자동화할 수 있습니다. App Service 및 Functions에 배포하는 방법에 대한 자세한 내용은 [App Service에서 리소스 배포 자동화](../app-service/app-service-deploy-complex-application-predictably.md) 및 [Azure Functions에서 리소스 배포 자동화](../azure-functions/functions-infrastructure-as-code.md)를 참조하세요.

모든 `Microsoft.Web/sites` 유형의 리소스는 ID를 사용하여 리소스 정의에 다음 속성을 포함하는 방법으로 만들 수 있습니다.
```json
"identity": {
    "type": "SystemAssigned"
}    
```

이는 응용 프로그램에 대한 ID를 만들어서 관리하라고 Azure에 알려주는 것입니다.

예를 들어 웹앱이 다음과 같을 수 있습니다.
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

사이트가 생성되면 다음과 같은 추가 속성이 있습니다.
```json
"identity": {
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

여기서 `<TENANTID>` 및 `<PRINCIPALID>`는 GUID로 대체됩니다. tenantId 속성은 ID가 속한 AAD 테넌트를 식별합니다. principalId는 응용 프로그램 새 ID의 고유 식별자입니다. AAD 내에서 서비스 주체는 사용자가 App Service 또는 Azure Functions 인스턴스에 지정한 이름과 동일한 이름을 갖습니다.

## <a name="obtaining-tokens-for-azure-resources"></a>Azure 리소스 토큰 가져오기

앱은 자체 ID를 사용하여 Azure Key Vault 같은 AAD로 보호되는 다른 리소스의 토큰을 가져올 수 있습니다. 이러한 토큰은 응용 프로그램의 특정 사용자가 아닌 리소스에 액세스하는 응용 프로그램을 나타냅니다. 

> [!IMPORTANT]
> 응용 프로그램의 액세스를 허용하도록 대상 리소스를 구성해야 할 수도 있습니다. 예를 들어 Key Vault 토큰을 요청할 때에는 응용 프로그램의 ID를 포함하는 액세스 정책을 추가했는지 확인해야 합니다. 그렇지 않으면 토큰이 포함되어 있더라도 Key Vault 호출이 거부됩니다. 어떤 리소스가 Managed Service Identity 토큰을 지원하는지 자세히 알아보려면 [Azure AD 인증을 지원하는 Azure 서비스](../active-directory/managed-service-identity/overview.md#which-azure-services-support-managed-service-identity)를 참조하세요.

App Service 및 Azure Functions에서 토큰을 가져오는 간단한 REST 프로토콜이 있습니다. .NET 응용 프로그램의 경우 Microsoft.Azure.Services.AppAuthentication 라이브러리에서 이 프로토콜에 대한 추상화를 제공하고 로컬 개발 환경을 지원합니다.

### <a name="asal"></a>.NET용 Microsoft.Azure.Services.AppAuthentication 라이브러리 사용

.NET 응용 프로그램 및 함수의 경우 관리되는 서비스 ID를 사용하는 가장 간단한 방법은 Microsoft.Azure.Services.AppAuthentication 패키지입니다. 이 라이브러리 역시 개발 컴퓨터에서 Visual Studio, [Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) 또는 Active Directory 통합 인증의 사용자 계정을 사용하여 로컬로 코드를 테스트할 수 있습니다. 이 라이브러를 통한 로컬 개발 옵션에 대한 자세한 내용은[Microsoft.Azure.Services.AppAuthentication 참조]를 참조하세요. 이 섹션에서는 코드에서 이 라이브러리를 시작하는 방법을 보여 줍니다.

1. 응용 프로그램에 [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) 및 [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) NuGet 패키지의 참조를 추가합니다.

2.  응용 프로그램에 다음 코드를 추가합니다.

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.KeyVault;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Microsoft.Azure.Services.AppAuthentication 및 노출하는 작업에 대한 자세한 내용은 [Microsoft.Azure.Services.AppAuthentication 참조] 및 [MSI .NET이 포함된 App Service 및 KeyVault](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)를 참조하세요.

### <a name="using-the-rest-protocol"></a>REST 프로토콜 사용

관리되는 서비스 ID를 사용하는 앱에는 다음 두 개의 환경 변수가 정의되어 있습니다.
- MSI_ENDPOINT
- MSI_SECRET

**MSI_ENDPOINT**는 앱이 토큰을 요청할 수 있는 로컬 URL입니다. 리소스 토큰을 가져오려면 이 끝점에 다음 매개 변수를 포함하여 HTTP GET 요청을 보냅니다.

> [!div class="mx-tdBreakAll"]
> |매개 변수 이름|그런 다음|설명|
> |-----|-----|-----|
> |resource|쿼리|토큰을 가져와야 하는 리소스의 AAD 리소스 URI입니다.|
> |api-version|쿼리|사용할 토큰 API의 버전입니다. "2017-09-01"은 현재 지원되는 유일한 버전입니다.|
> |secret|헤더|MSI_SECRET 환경 변수의 값입니다.|


성공적인 200 OK 응답에는 다음 속성을 가진 JSON 본문이 포함됩니다.

> [!div class="mx-tdBreakAll"]
> |속성 이름|설명|
> |-------------|----------|
> |access_token|요청된 액세스 토큰입니다. 호출 웹 서비스는 이 토큰을 사용하여 수신 웹 서비스에 인증할 수 있습니다.|
> |expires_on|액세스 토큰이 만료되는 시간입니다. 날짜는 1970-01-01T0:0:0Z UTC부터 만료 시간까지 기간(초)으로 표시됩니다. 이 값은 캐시된 토큰의 수명을 결정하는 데 사용됩니다.|
> |resource|수신 웹 서비스의 앱 ID URI입니다.|
> |token_type|토큰 유형 값을 나타냅니다. Azure AD는 전달자 유형만 지원합니다. 전달자 토큰에 대한 자세한 내용은 [OAuth 2.0 권한 부여 프레임워크: 전달자 토큰 사용(RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt)을 참조하세요.|


이 응답은 [AAD 서비스 간 액세스 토큰 요청에 대한 응답](../active-directory/develop/v1-oauth2-client-creds-grant-flow.md#service-to-service-access-token-response)과 동일합니다.

> [!NOTE] 
> 환경 변수는 프로세스가 처음 시작될 때 설정되므로 응용 프로그램에 대해 관리되는 서비스 ID를 활성화한 후에는 코드에 `MSI_ENDPOINT` 및 `MSI_SECRET`를 사용하기 위해 응용 프로그램을 다시 시작하거나 코드를 재배포해야 할 수 있습니다.

### <a name="rest-protocol-examples"></a>REST 프로토콜 예제
예제 요청은 다음과 유사할 수 있습니다.
```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```
샘플 응답은 다음과 유사할 수 있습니다.
```
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "access_token": "eyJ0eXAi…",
    "expires_on": "09/14/2017 00:00:00 PM +00:00",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer"
}
```

### <a name="code-examples"></a>코드 예제
<a name="token-csharp"></a>C#에서 이 요청을 만들려면:
```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```
> [!TIP]
> .NET 언어의 경우 이 요청을 직접 만드는 대신 [Microsoft.Azure.Services.AppAuthentication](#asal)을 사용해도 됩니다.

<a name="token-js"></a>Node.JS:
```javascript
const rp = require('request-promise');
const getToken = function(resource, apiver, cb) {
    var options = {
        uri: `${process.env["MSI_ENDPOINT"]}/?resource=${resource}&api-version=${apiver}`,
        headers: {
            'Secret': process.env["MSI_SECRET"]
        }
    };
    rp(options)
        .then(cb);
}
```

<a name="token-powershell"></a>PowerShell:
```powershell
$apiVersion = "2017-09-01"
$resourceURI = "https://<AAD-resource-URI-for-resource-to-obtain-token>"
$tokenAuthURI = $env:MSI_ENDPOINT + "?resource=$resourceURI&api-version=$apiVersion"
$tokenResponse = Invoke-RestMethod -Method Get -Headers @{"Secret"="$env:MSI_SECRET"} -Uri $tokenAuthURI
$accessToken = $tokenResponse.access_token
```

## <a name="remove"></a>ID 제거

포털, PowerShell 또는 CLI를 사용하여 생성할 때와 같은 방식으로 기능을 사용하지 않도록 설정하여 ID를 제거할 수 있습니다. REST/ARM 템플릿 프로토콜에서는 유형을 "None"으로 설정하여 수행됩니다.

```json
"identity": {
    "type": "None"
}    
```

ID를 이런 방식으로 제거하면 AAD에서 보안 주체도 삭제됩니다. 앱 리소스가 삭제될 때 시스템 할당 ID가 AAD에서 자동으로 제거됩니다.

> [!NOTE] 
> 설정할 수 있는 응용 프로그램 설정인 WEBSITE_DISABLE_MSI도 있으며 이것은 로컬 토큰 서비스를 비활성화합니다. 하지만 ID는 그대로 유지되고 도구에는 MSI가 "on" 또는 "enabled"로 표시됩니다. 따라서 이 설정은 사용하지 않는 것이 좋습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [관리되는 서비스 ID를 사용하여 SQL Database에 안전하게 액세스](app-service-web-tutorial-connect-msi.md)

[Microsoft.Azure.Services.AppAuthentication 참조]: https://go.microsoft.com/fwlink/p/?linkid=862452
