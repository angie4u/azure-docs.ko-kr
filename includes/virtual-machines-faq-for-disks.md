---
title: 포함 파일
description: 포함 파일
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 336e6e163178cd6d244460dbf9bee2a5bc9d714e
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935748"
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Azure IaaS VM 디스크와 관리 및 관리되지 않는 프리미엄 디스크에 대한 질문과 대답

이 문서에서는 Azure Managed Disks 및 Azure 프리미엄 SD 디스크에 대해 몇몇 자주 묻는 질문과 대답을 설명합니다.

## <a name="managed-disks"></a>Managed Disks

**Azure Managed Disks란?**

Managed Disks는 저장소 계정 관리를 처리하여 Azure IaaS VM을 위한 디스크 관리를 간소화하는 기능입니다. 자세한 내용은 [Managed Disks 개요](../articles/virtual-machines/windows/managed-disks-overview.md)를 참조하세요.

**80GB인 기존 VHD에서 표준 Managed Disk를 만들 경우 비용은 얼마나 드나요?**

80GB VHD로 만든 표준 관리 디스크는 다음과 같은 사용 가능 표준 디스크 크기인 S10 디스크로 취급됩니다. S10 디스크 가격 책정에 따라 요금이 부과됩니다. 자세한 내용은 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/storage)를 참조하세요.

**표준 관리 디스크에 대한 트랜잭션 비용이 있나요?**

예. 각 트랜잭션에 대해 요금이 부과됩니다. 자세한 내용은 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/storage)를 참조하세요.

**표준 관리 디스크에 대한 요금은 디스크에 있는 데이터의 실제 크기에 대해 부과되나요? 아니면 디스크의 프로비전된 용량에 대해 부과되나요?**

디스크의 프로비전된 용량에 따라 청구됩니다. 자세한 내용은 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/storage)를 참조하세요.

**프리미엄 Managed Disks의 가격은 관리되지 않는 디스크와 어떻게 다르게 책정되나요?**

프리미엄 관리 디스크의 가격은 관리되지 않는 프리미엄 디스크와 같습니다.

**내 Managed Disks의 저장소 계정 유형(표준 또는 프리미엄)을 내가 변경할 수 있나요?**

예. Azure Portal, PowerShell 또는 Azure CLI를 사용하여 Managed Disks의 저장소 계정 형식을 변경할 수 있습니다.

**Azure Storage 계정에서 VHD 파일을 사용하여 다른 구독에 Managed Disk를 만들 수 있나요?**

예.

**Azure Storage 계정에서 VHD 파일을 사용하여 다른 지역에 관리 디스크를 만들 수 있나요?**

아니요.

**고객이 Managed Disks를 사용하는 경우 규모 제한이 있나요?**

Managed Disks는 저장소 계정과 관련된 한도를 없앱니다. 그러나 최대 한도는 하나의 구독에 대해 지역당 및 디스크 형식당 50,000개의 관리 디스크입니다.

**관리 디스크의 증분 스냅숏을 가져올 수 있나요?**

아니요. 현재 스냅숏 기능은 Managed Disk의 전체 복사본을 만듭니다.

**관리 및 관리되지 않는 디스크를 조합하여 가용성 집합의 VM을 구성할 수 있나요?**

아니요. 가용성 집합에 있는 VM은 모두 Managed Disks를 사용하거나 모두 관리되지 않는 디스크를 사용해야 합니다. 가용성 집합을 만들 때 사용할 디스크 유형을 사용자가 선택할 수 있습니다.

**Managed Disks가 Azure Portal에서 기본 옵션인가요?**

예. 

**내가 빈 관리 디스크를 만들 수 있나요?**

예. 사용자가 빈 디스크를 만들 수 있습니다. Managed Disk는 VM에 독립적으로 즉, VM에 연결하지 않고 만들 수 있습니다.

**Managed Disks를 사용하는 가용성 집합에 지원되는 장애 도메인 수는 몇 개인가요?**

Managed Disks를 사용하는 가용성 집합이 위치한 지역에 따라 지원되는 장애 도메인 수는 2개 또는 3개입니다.

**진단을 위한 표준 저장소 계정은 어떤 방식으로 설정되나요?**

VM 진단을 위한 개인 저장소 계정을 설정할 수 있습니다.

**어떤 종류의 역할 기반 Access Control 지원을 Managed Disks에 사용할 수 있나요?**

Managed Disks에서는 세 가지 주요 기본 역할을 지원합니다.

* 소유자: 액세스를 포함한 모든 것을 관리할 수 있음
* 참여자: 액세스를 제외한 모든 것을 관리할 수 있음
* 읽기 권한자: 모든 항목을 볼 수 있지만 변경할 수 없음

**개인 저장소 계정에 Managed Disk를 복사하거나 내보낼 수 있는 방법이 있나요?**

관리 디스크에 대한 읽기 전용 SAS(공유 액세스 서명) URI를 생성한 후 이를 사용하여 개인 저장소 계정 또는 온-프레미스 저장소에 콘텐츠를 복사할 수 있습니다. Azure Portal, Azure PowerShell, Azure CLI 또는 [AzCopy](../articles/storage/common/storage-use-azcopy.md)에서 SAS URI를 사용할 수 있습니다.

**내 관리 디스크의 복사본을 만들 수 있나요?**

사용자는 관리 디스크의 스냅숏을 만든 후 이 스냅숏을 사용하여 다른 관리 디스크를 만들 수 있습니다.

**관리되지 않는 디스크도 계속 지원되나요?**

예. 관리되지 않는 디스크와 Managed Disks가 모두 지원됩니다. 새 워크로드에 대해 Managed Disks를 사용하고 현재 워크로드를 Managed Disks로 마이그레이션하는 것이 좋습니다.


**128GB 디스크를 만든 후 130GB로 크기를 증가시키려는 경우 다음 디스크 크기(256GB)에 대한 요금이 부과되나요?**

예.

**로컬 중복 저장소, 지역 중복 저장소 및 영역 중복 저장소 Managed Disks를 만들 수 있나요?**

Azure Managed Disks에서는 현재 로컬 중복 저장소 Managed Disks만 지원합니다.

**Managed Disks를 축소하거나 크기를 줄일 수 있나요?**

아니요. 이 기능은 현재 지원되지 않습니다. 

**내 디스크에서 임대를 중단할 수 있나요?**

아니요. 현재 디스크를 사용하는 경우 실수로 삭제하지 않도록 방지하기 위해 임대로는 지원되지 않습니다.

**VM을 프로비전하는 데 특수화된(시스템 준비 도구를 사용하여 생성되거나 일반화된) 운영 체제 디스크를 사용하는 경우 컴퓨터 이름 속성을 변경할 수 있나요?**

아니요. 컴퓨터 이름 속성은 업데이트할 수 없습니다. 새 VM은 운영 체제 디스크를 만들 때 사용한 부모 VM에서 이 속성을 상속합니다. 

**Managed Disks를 사용하여 VM을 만드는 Azure Resource Manager 템플릿 예제를 어디에 배치할 수 있나요?**
* [Managed Disks를 사용하는 템플릿 목록](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

**비관리 및 관리 디스크를 동일한 VM에 함께 배치할 수 있나요?**

아니요.

## <a name="standard-ssd-disks-preview"></a>표준 SSD 디스크(미리 보기)

**Azure 표준 SSD 디스크란?**
표준 SSD 디스크는 반도체 미디어에서 지원되며 더 낮은 IOPS 수준에서 일관된 성능을 필요로 하는 워크로드에 대해 비용 효율적인 저장소로 최적화된 표준 디스크입니다. 미리 보기에서는 제한된 수의 지역에서 제한된 관리 효율성으로 사용할 수 있습니다(Resource Manager 템플릿을 통해 사용 가능).

<a id="standard-ssds-azure-regions"></a>**현재 표준 SSD 디스크(미리 보기)가 지원되는 지역**
* 북유럽
* 프랑스 중부
* 미국 동부 2
* 미국 중부
* 캐나다 중부
* 동아시아
* 한국 남부
* 오스트레일리아 동부

**표준 SSD 디스크를 만드는 방법**
현재, Azure Resource Manager 템플릿을 사용하여 표준 SSD 디스크를 만들 수 있습니다. 다음은 Resource Manager 템플릿에서 표준 SSD 디스크를 만드는 데 필요한 매개 변수입니다.

* Microsoft.Compute에 대한 *apiVersion*은 `2018-04-01` 이상으로 설정해야 합니다.
* *managedDisk.storageAccountType*을 `StandardSSD_LRS`로 지정합니다.

다음 예제에서는 표준 SSD 디스크를 사용하는 VM에 대한 *properties.storageProfile.osDisk* 섹션을 보여 줍니다.

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

템플릿을 사용하여 표준 SSD 디스크를 만드는 방법에 대한 전체 템플릿 예제를 보려면 [표준 SSD 데이터 디스크를 사용하여 Windows 이미지에서 VM 만들기](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)를 참조하세요.

**기존 디스크를 표준 SSD로 변환할 수 있나요?**
예, 할 수 있습니다. Managed Disks로 변환하는 방법에 대한 일반 지침을 보려면 [Azure 관리 디스크 저장소를 표준에서 프리미엄으로, 또 그 반대로 변환](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/convert-disk-storage)을 참조하세요. 또한 다음 값을 사용하여 디스크 유형을 표준 SSD로 업데이트하세요.
-AccountType StandardSSD_LRS

**표준 SSD를 관리되지 않는 디스크로 사용할 수 있나요?**
아니요. 표준 SSD 디스크는 Managed Disks로만 사용할 수 있습니다.

**표준 SSD 디스크가 "단일 인스턴스 VM SLA"를 지원하나요?**
아니요. 표준 SSD에는 단일 인스턴스 VM SLA가 없습니다. 단일 인스턴스 VM SLA에는 프리미엄 SSD 디스크를 사용합니다.

## <a name="migrate-to-managed-disks"></a>Managed Disks로 마이그레이션 

**Managed Disks로 마이그레이션하기 전/후에 기존 Azure Backup 서비스 구성에 어떤 변경이 필요한가요?**

변경할 필요가 없습니다. 

**마이그레이션 전에 Azure Backup 서비스를 통해 만든 내 VM 백업은 계속 작동하나요?**

예. 백업은 원활하게 작동합니다.

**Managed Disks로 마이그레이션하기 전/후에 기존 Azure Disk Encryption 구성에 어떤 변경이 필요한가요?**

변경할 필요가 없습니다. 

**기존 VM Scale Sets가 관리되지 않는 디스크에서 Managed Disks로 자동으로 마이그레이션되도록 지원되나요?**

아니요. 관리되지 않는 디스크에서 이전 확장 집합의 이미지를 사용하여 Managed Disks로 새 확장 집합을 만들 수 있습니다. 

**Managed Disks로 마이그레이션하기 전에 페이지 Blob 스냅숏에서 Managed Disks를 만들 수 있나요?**

아니요. 페이지 blob 스냅숏을 페이지 blob으로 내보내고 내보낸 페이지 blob에서 Managed Disks를 만들 수 있습니다. 

**Azure Site Recovery에 의해 보호되는 온-프레미스 컴퓨터를 Managed Disks가 있는 VM으로 장애 조치(Failover)할 수 있나요?**

예. Managed Disks가 있는 VM으로 장애 조치(Failover)하도록 선택할 수 있습니다.

**마이그레이션을 수행할 경우 Azure 간 복제를 통해 Azure Site Recovery에 의해 보호되는 Azure VM에 영향을 미치나요?**

예. 현재 Managed Disks가 있는 VM에 대한 Azure Site Recovery Azure 간 보호는 공개 미리 보기 서비스로만 제공됩니다.

**Managed Disks에 이전에 암호화된 저장소 계정에 있는 관리되지 않는 디스크가 있는 VM을 마이그레이션할 수 있나요?**

예

## <a name="managed-disks-and-storage-service-encryption"></a>Managed Disks 및 Storage 서비스 암호화 

**Managed Disk를 만들 경우 Azure Storage 서비스 암호화를 기본적으로 사용하나요?**

예.

**암호화 키는 누가 관리하나요?**

Microsoft에서 암호화 키를 관리합니다.

**내 Managed Disks에 Storage 서비스 암호화를 비활성화할 수 있나요?**

아니요.

**Storage 서비스 암호화는 특정 지역에서만 사용할 수 있나요?**

아니요. Managed Disks를 사용할 수 있는 모든 지역에서 사용할 수 있습니다. 모든 공용 지역 및 독일에서 Managed Disks를 사용할 수 있습니다. 중국에서도 사용할 수 있지만, 고객 관리 키가 아닌 Microsoft 관리 키의 경우로 국한됩니다.

**내 Managed Disk가 암호화되었는지 어떻게 확인할 수 있나요?**

Azure Portal, Azure CLI 및 PowerShell에서 Managed Disk를 만든 시간을 알아볼 수 있습니다. 2017년 6월 9일 이후인 경우 디스크는 암호화됩니다.

**2017년 6월 10일 이전에 만들어진 내 기존 디스크를 어떻게 암호화할 수 있나요?**

2017년 6월 10일을 기준으로 기존 Managed Disks에 작성된 새 데이터는 자동으로 암호화됩니다. 기존 데이터를 암호화할 예정이며 암호화는 백그라운드에서 비동기적으로 수행됩니다. 이제 기존 데이터를 암호화해야 하는 경우 디스크의 복사본을 만듭니다. 새 디스크가 암호화됩니다.

* [Azure CLI를 사용하여 Managed Disks 복사](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [PowerShell을 사용하여 Managed Disks 복사](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

**관리되는 스냅숏 및 이미지가 암호화되나요?**

예. 2017년 6월 9일 이후에 만든 모든 관리되는 스냅숏 및 이미지는 자동으로 암호화됩니다. 

**Managed Disks에 이전에 암호화된 저장소 계정에 있는 관리되지 않는 디스크가 있는 VM을 변환할 수 있나요?**

예

**Managed Disk 또는 스냅숏에서 내보낸 VHD도 암호화되나요?**

아니요. 하지만 암호화된 Managed Disk 또는 스냅숏의 암호화된 저장소 계정에 VHD를 내보낼 경우 암호화됩니다. 

## <a name="premium-disks-managed-and-unmanaged"></a>프리미엄 디스크: 관리 및 관리되지 않는 디스크

**VM에서 DSv2와 같이 프리미엄 SD 디스크를 지원하는 크기를 사용하는 경우 프리미엄 및 표준 데이터 디스크를 모두 연결할 수 있나요?** 

예.

**프리미엄 및 표준 데이터 디스크를 모두 D, Dv2, G 또는 F 시리즈와 같이 프리미엄 SD 디스크를 지원하지 않는 크기에 연결할 수 있나요?**

아니요. 프리미엄 SD 디스크를 지원하는 크기를 사용하지 않는 VM에는 표준 데이터 디스크만 연결할 수 있습니다.

**80GB인 기존 VHD로 프리미엄 데이터 디스크를 만들 경우 비용은 얼마가 드나요?**

80GB VHD에서 만든 프리미엄 데이터 디스크는 그 다음 프리미엄 디스크 크기인 P10으로 취급됩니다. P10 디스크 가격 책정에 따라 요금이 부과됩니다.

**프리미엄 SD 디스크를 사용하면 트랜잭션 비용이 있나요?**

특정 한도의 IOPS 및 처리량이 프로비전되는 디스크 크기마다 고정 비용이 있습니다. 기타 비용은 아웃바운드 대역폭 및 스냅숏 용량입니다(해당하는 경우). 자세한 내용은 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/storage)를 참조하세요.

**디스크 캐시에서 가져올 수 있는 IOPS 및 처리량 한도는 얼마나 되나요?**

DS 시리즈의 캐시 및 로컬 SSD에 대한 결합 제한은 코어당 4,000 IOPS 및 코어당 초당 33MB입니다. GS 시리즈는 코어당 5,000 IOPS 및 코어당 초당 50MB를 제공합니다.

**Managed Disks VM에 로컬 SSD가 지원되나요?**

로컬 SSD는 Managed Disks VM에 포함되어 있는 임시 저장소입니다. 이 임시 저장소에 대한 추가 비용은 없습니다. 이 로컬 SSD가 Azure Blob Storage에 보존되지 않기 때문에 응용 프로그램 데이터를 저장하는 데 사용하지 않는 것이 좋습니다.

**프리미엄 디스크에서 TRIM의 사용에 대한 영향이 있나요?**

프리미엄 또는 표준 디스크의 Azure 디스크에서 TRIM을 사용해도 문제는 없습니다.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>새 디스크 크기: 관리 및 관리되지 않는 디스크

**운영 체제 및 데이터 디스크에 지원되는 가장 큰 디스크 크기는 무엇인가요?**

Azure에서 운영 체제 디스크에 지원하는 파티션 형식은 MBR(마스터 부트 레코드)입니다. MBR 형식은 최대 2TB의 디스크를 지원합니다. Azure에서 운영 체제 디스크에 지원하는 최대 크기는 2TB입니다. Azure는 데이터 디스크에 최대 4TB를 지원합니다. 

**지원되는 가장 큰 페이지 Blob 크기는 무엇인가요?**

Azure에서 지원하는 페이지 Blob 크기는 최대 8TB(8,191GB)입니다. VM에 데이터 또는 운영 체제 디스크로 연결될 경우의 최대 페이지 Blob 크기는 4TB(4,095GB)입니다.

**새 버전의 Azure 도구를 사용하여 1TB를 초과하는 디스크를 생성, 연결, 크기 조정 및 업로드해야 하나요?**

1TB를 초과하는 디스크를 생성, 연결 또는 크기 조정하기 위해 기존 Azure 도구를 업그레이드할 필요가 없습니다. 페이지 Blob 또는 관리되지 않는 디스크와 같이 온-프레미스에서 Azure에 직접 VHD 파일을 업로드하려면 최신 도구 집합을 사용해야 합니다.

|Azure 도구      | 지원되는 버전                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | 버전 번호 4.1.0: 2017년 6월 릴리스 이상|
|Azure CLI v1     | 버전 번호 0.10.13: 2017년 5월 릴리스 이상|
|AzCopy           | 버전 번호 6.1.0: 2017년 6월 릴리스 이상|

Azure CLI v2 및 Azure Storage Explorer에 대한 지원이 곧 제공됩니다. 

**P4 및 P6 디스크 크기가 관리되지 않는 디스크 또는 페이지 Blob에 지원되나요?**

아니요. P4(32GB) 및 P6(64GB) 디스크 크기는 Managed Disks에만 지원됩니다. 관리되지 않는 디스크 및 페이지 Blob에도 곧 지원됩니다.

**작은 디스크를 사용하기 전에 64GB 미만인 기존 프리미엄 Managed Disks를 만든 경우(2017년 6월 15일경) 어떻게 청구되나요?**

64GB 미만의 기존 프리미엄 디스크는 P10 가격 책정 계층에 따라 청구됩니다. 

**P4 또는 P6 P10에서 64GB 미만인 작은 프리미엄 디스크의 디스크 계층을 전환하려면 어떻게 해야 하나요?**

작은 디스크의 스냅숏을 찍고 디스크를 만들어서 프로비전된 크기에 따라 P4 또는 P6으로 가격 책정 계층을 자동으로 전환할 수 있습니다. 


## <a name="what-if-my-question-isnt-answered-here"></a>여기서 내 질문에 대답하지 않으면 어떻게 하나요?

질문하려는 내용이 아래 목록에 나와 있지 않으면 알려 주세요. 대답을 확인하는 방법을 알려 드리겠습니다. 설명에 있는 이 문서의 끝에 질문을 게시할 수 있습니다. Azure Storage 팀 및 다른 커뮤니티 구성원과 이 문서에 대한 의견을 교환하려면 MSDN [Azure Storage 포럼](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)을 사용합니다.

기능을 요청하려면 요청 내용과 아이디어를 [Azure Storage 피드백 포럼](https://feedback.azure.com/forums/217298-storage)으로 제출해 주세요.
