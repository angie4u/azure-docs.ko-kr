---
title: Azure Kubernetes Service와 Azure Active Directory 통합
description: Azure Active Directory 사용 Azure Kubernetes Service 클러스터를 만드는 방법.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 6/17/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 2c4e0f8c31299644c912a70fc91bbdfa6da6795b
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579030"
---
# <a name="integrate-azure-active-directory-with-aks---preview"></a>AKS와 Azure Active Directory 통합 - 미리 보기

사용자 인증을 위해 Azure Active Directory를 사용하도록 AKS(Azure Kubernetes Service)를 구성할 수 있습니다. 이 구성에서 Azure Active Directory 인증 토큰을 사용하여 Azure Kubernetes Service 클러스터에 로그인할 수 있습니다. 또한 클러스터 관리자는 사용자 ID 또는 디렉터리 그룹 구성원 자격에 따라 Kubernetes 역할 기반 액세스 제어를 구성할 수 있습니다.

이 문서에서는 AKS 및 Azure AD에 대한 모든 필요한 필수 구성 요소 만들기, Azure AD 사용 클러스터 배포 및 AKS 클러스터에서 단순 RBAC 역할 만들기에 대해 자세히 설명합니다. 기존의 RBAC 비지원 AKS 클러스터는 현재 RBAC 사용을 위해 업데이트할 수 없습니다.

> [!IMPORTANT]
> AKS(Azure Kubernetes Service) RBAC와 Azure AD 통합은 현재 **미리 보기** 상태입니다. [부속 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)에 동의하면 미리 보기를 사용할 수 있습니다. 이 기능의 몇 가지 측면은 일반 공급(GA) 전에 변경될 수 있습니다.
>

## <a name="authentication-details"></a>인증 세부 정보

OpenID Connect와 함께 Azure Kubernetes 클러스터에 Azure AD 인증이 제공됩니다. OpenID Connect는 OAuth 2.0 프로토콜을 기반으로 하는 ID 계층입니다. OpenID Connect에 대한 자세한 내용은 [Open ID 연결 설명서][open-id-connect]에서 찾을 수 있습니다.

Kubernetes 클러스터 내부에서 인증 토큰을 확인하는 데 Webhook 토큰 인증이 사용됩니다. Webhook 토큰 인증은 AKS 클러스터의 일부로 구성 및 관리됩니다. Webhook 토큰 인증에 대한 자세한 내용은 [webhook 인증 설명서][kubernetes-webhook]에서 찾을 수 있습니다.

> [!NOTE]
> AKS 인증을 위해 Azure AD를 구성할 때 두 개의 Azure AD 응용 프로그램이 구성됩니다. Azure 테넌트 관리자가 이 작업을 완료해야 합니다.

## <a name="create-server-application"></a>서버 응용 프로그램 만들기

첫 번째 Azure AD 응용 프로그램은 사용자 Azure AD 그룹 구성원 자격을 가져오는 데 사용됩니다.

1. **Azure Active Directory** > **앱 등록** > **새 응용 프로그램 등록**을 선택합니다.

  응용 프로그램에 이름을 지정하고, 응용 프로그램 유형에 대해 **웹앱/API**를 선택하고, **로그온 URL**에 URI 서식 지정된 값을 입력합니다. 완료되면 **만들기**를 선택합니다.

  ![Azure AD 등록 만들기](media/aad-integration/app-registration.png)

2. **매니페스트**를 선택하고 `groupMembershipClaims` 값을 `"All"`로 편집합니다.

  완료되면 업데이트를 저장합니다.

  ![그룹 멤버 자격을 모두로 업데이트](media/aad-integration/edit-manifest.png)

3. Azure AD 응용 프로그램에서 **설정** > **키**를 선택합니다.

  키 설명을 추가하고, 만료 마감일을 선택하고, **저장**을 선택합니다. 키 값을 적어둡니다. Azure AD 사용 AKS 클러스터를 배포할 때 이 값은 `Server application secret`이라고 합니다.

  ![응용 프로그램 개인 키 가져오기](media/aad-integration/application-key.png)

4. Azure AD 응용 프로그램으로 돌아와서 **설정** > **필요한 권한** > **추가** > **API 선택** > **Microsoft Graph** > **선택**을 선택합니다.

  ![Graph API 선택](media/aad-integration/graph-api.png)

5. **응용 프로그램 사용 권한** 아래에서 **디렉터리 데이터 읽기** 옆을 체크합니다.

  ![응용 프로그램 그래프 사용 권한 설정](media/aad-integration/read-directory.png)

6. **위임된 사용 권한**  아래에서 **로그인 및 사용자 프로필 읽기** 및 **디렉터리 데이터 읽기** 옆을 체크합니다. 완료되면 업데이트를 저장합니다.

  ![응용 프로그램 그래프 사용 권한 설정](media/aad-integration/delegated-permissions.png)

7. **완료**를 선택하고 API 목록에서 *Microsoft Graph*를 선택 한 후 **권한 부여**를 선택합니다. 현재 계정이 테넌트 관리자가 아닌 경우 이 단계가 실패합니다.

  ![응용 프로그램 그래프 사용 권한 설정](media/aad-integration/grant-permissions.png)

8. 응용 프로그램으로 돌아오고 **응용 프로그램 ID**를 기록해 둡니다. Azure AD 사용 AKS 클러스터를 배포할 때 이 값은 `Server application ID`이라고 합니다.

  ![응용 프로그램 ID 가져오기](media/aad-integration/application-id.png)

## <a name="create-client-application"></a>클라이언트 응용 프로그램 만들기

두 번째 Azure AD 응용 프로그램은 Kubernetes CLI(kubectl)를 사용하여 로그인할 때 사용됩니다.

1. **Azure Active Directory** > **앱 등록** > **새 응용 프로그램 등록**을 선택합니다.

  응용 프로그램에 이름을 지정하고, 응용 프로그램 유형에 대해 **네이티브**를 선택하고, **리디렉션 URI**에 URI 서식 지정된 값을 입력합니다. 완료되면 **만들기**를 선택합니다.

  ![AAD 등록 만들기](media/aad-integration/app-registration-client.png)

2. Azure AD 응용 프로그램에서 **설정** > **필요한 권한** > **추가** > **API 선택**을 선택하고 이 문서의 마지막 단계에서 만든 서버 응용 프로그램의 이름을 검색합니다.

  ![응용 프로그램 사용 권한 구성](media/aad-integration/select-api.png)

3. 응용 프로그램 옆에 확인 표시를 두고 **선택**을 클릭합니다.

  ![AKS AAD 서버 응용 프로그램 엔드포인트 선택](media/aad-integration/select-server-app.png)

4. **완료** 및 **사용 권한 부여**를 선택하여 이 단계를 완료합니다.

  ![권한 부여](media/aad-integration/grant-permissions-client.png)

5. AD 응용 프로그램으로 돌아오고, **응용 프로그램 ID**를 기록해 둡니다. Azure AD 사용 AKS 클러스터를 배포할 때 이 값은 `Client application ID`이라고 합니다.

  ![응용 프로그램 ID 가져오기](media/aad-integration/application-id-client.png)

## <a name="get-tenant-id"></a>테넌트 ID 가져오기

마지막으로 Azure 테넌트의 ID를 가져옵니다. 이 값은 AKS 클러스터를 배포하는 경우에도 사용됩니다.

Azure Portal에서 **Azure Active Directory** > **속성**을 선택하고 **디렉터리 ID**를 기록해 둡니다. Azure AD 사용 AKS 클러스터를 배포할 때 이 값은 `Tenant ID`이라고 합니다.

![Azure 테넌트 ID 가져오기](media/aad-integration/tenant-id.png)

## <a name="deploy-cluster"></a>클러스터 배포

[az group create][az-group-create] 명령을 사용하여 AKS 클러스터에 대한 리소스 그룹을 만듭니다.

```azurecli
az group create --name myAKSCluster --location eastus
```

[az aks create][az-aks-create] 명령을 사용하여 클러스터를 배포합니다. 아래 샘플 명령의 값을 Azure AD 응용 프로그램을 만들 때 수집한 값으로 바꿉니다.

```azurecli
az aks create --resource-group myAKSCluster --name myAKSCluster --generate-ssh-keys --enable-rbac \
  --aad-server-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-server-app-secret wHYomLe2i1mHR2B3/d4sFrooHwADZccKwfoQwK2QHg= \
  --aad-client-app-id 7ee598bb-0000-0000-0000-83692e2d717e \
  --aad-tenant-id 72f988bf-0000-0000-0000-2d7cd011db47
```

## <a name="create-rbac-binding"></a>RBAC 바인딩 만들기

Azure Active Directory 계정을 AKS 클러스터와 함께 사용하려면 역할 바인딩 또는 클러스터 역할 바인딩을 만들어야 합니다.

먼저 `--admin` 인수와 함께 [az aks get-credentials][az-aks-get-credentials] 명령을 사용하여 관리자 액세스로 클러스터에 로그인합니다.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster --admin
```

다음으로 다음 매니페스트를 사용하여 Azure AD 계정에 대한 ClusterRoleBinding을 만듭니다. 사용자 이름을 Azure AD 테넌트의 이름으로 업데이트합니다. 이 예제에서는 계정에 클러스터의 모든 네임스페이스에 대한 모든 권한을 제공합니다.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: "user@contoso.com"
```

Azure AD 그룹의 모든 구성원에 대해 역할 바인딩을 만들 수도 있습니다. Azure AD 그룹은 그룹 개체 ID를 사용하여 합니다.

 ```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: contoso-cluster-admins
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- apiGroup: rbac.authorization.k8s.io
   kind: Group
   name: "894656e1-39f8-4bfe-b16a-510f61af6f41"
```

RBAC를 사용하여 Kubernetes 클러스터 보호에 대한 자세한 내용은 [RBAC 권한 부여 사용][rbac-authorization]을 참조하세요.

## <a name="access-cluster-with-azure-ad"></a>Azure AD를 사용하여 클러스터에 액세스

다음으로 [az aks get-credentials][az-aks-get-credentials] 명령을 사용하여 비 관리자 사용자에 대한 컨텍스트를 끌어 옵니다.

```azurecli
az aks get-credentials --resource-group myAKSCluster --name myAKSCluster
```

모든 kubectl 명령을 실행한 후에 Azure에 인증할지를 묻는 메시지가 표시됩니다. 화면에 나타나는 지침을 따릅니다.

```console
$ kubectl get nodes

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code BUJHWDGNL to authenticate.

NAME                       STATUS    ROLES     AGE       VERSION
aks-nodepool1-42032720-0   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-1   Ready     agent     1h        v1.9.6
aks-nodepool1-42032720-2   Ready     agent     1h        v1.9.6
```

완료되면 인증 토큰이 캐시됩니다. 토큰이 만료되거나 Kubernetes config 파일이 다시 생성될 때 로그인할지를 묻는 메시지가 다시 표시됩니다.

성공적으로 로그인한 후 인증 오류 메시지가 표시되는 경우 로그인하는 사용자가 Azure AD에서 게스트가 아닌지 확인합니다(다른 디렉터리에서 페더레이션된 로그인을 사용하는 경우 게스트인 경우가 종종 있음).
```console
error: You must be logged in to the server (Unauthorized)
```


## <a name="next-steps"></a>다음 단계

[RBAC 권한 부여 사용][rbac-authorization] 설명서로 RBAC를 사용하여 Kubernetes 클러스터 보호에 대해 자세히 알아봅니다.

<!-- LINKS - external -->
[kubernetes-webhook]:https://kubernetes.io/docs/reference/access-authn-authz/authentication/#webhook-token-authentication
[rbac-authorization]: https://kubernetes.io/docs/reference/access-authn-authz/rbac/

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[open-id-connect]:../active-directory/develop/v1-protocols-openid-connect-code.md
