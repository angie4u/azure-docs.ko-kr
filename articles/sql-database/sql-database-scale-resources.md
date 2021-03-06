---
title: Azure SQL Database 리소스 크기 조정 | Microsoft Docs
description: 이 문서에서는 할당된 리소스를 추가하거나 제거하여 데이터베이스 크기를 조정하는 방법을 설명합니다.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab
ms.service: sql-database
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: a6b987d9815cfabed6dd986a0d9842a97f5b5868
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39092055"
---
# <a name="scale-database-resources"></a>데이터베이스 리소스 크기 조정

Azure SQL Database를 사용하면 가동 중지 시간을 최소화하면서 데이터베이스에 리소스를 동적으로 추가할 수 있습니다.

## <a name="overview"></a>개요

앱에 대한 수요가 몇 개의 장치 및 고객에서 수백만으로 증가할 경우, Azure SQL Database는 가동 중지 시간을 최소화하면서 즉시 크기가 조정됩니다. 확장성은 PaaS의 가장 중요한 특성 중 하나로, 필요한 경우 서비스에 리소스를 동적으로 추가할 수 있게 해줍니다. Azure SQL Database를 사용하면 데이터베이스에 할당된 리소스(CPU 처리 능력, 메모리, IO 처리량 및 저장소)를 쉽게 변경할 수 있습니다.  
인덱싱 또는 쿼리 재작성 방법으로 해결할 수 없는, 응용 프로그램 사용량 증가로 인한 성능 문제를 완화할 수 있습니다. 데이터베이스가 현재 리소스 한도에 도달하고 들어오는 워크로드를 처리하기 위해 추가 처리 능력이 필요할 때 리소스를 추가하면 빠르게 대응할 수 있습니다. Azure SQL Database를 사용하면 필요하지 않을 때 리소스를 축소하여 비용을 줄일 수도 있습니다.
하드웨어 구매 및 기본 인프라 변경에 대해 염려할 필요가 없습니다. Azure Portal에서 슬라이더를 사용하여 데이터베이스 크기를 쉽게 조정할 수 있습니다.

![데이터베이스 성능 조정](media/sql-database-scalability/scale-performance.svg)

Azure SQL Database는 [DTU 기반 구매 모델](sql-database-service-tiers-dtu.md) 또는 [vCore 기반 구매 모델](sql-database-service-tiers-vcore.md)을 제공합니다. 
-   [DTU 기반 구매 모델](sql-database-service-tiers-dtu.md)에서는 경량 또는 중량 데이터베이스 워크로드를 지원하기 위해 기본, 표준 및 프리미엄의 세 가지 서비스 계층으로 계산, 메모리 및 IO 리소스를 함께 제공합니다. 각 계층 내의 성능 수준은 다양하게 섞인 리소스를 제공합니다. 여기에 저장소 리소스를 추가할 수 있습니다.
-   [vCore 기반 구매 모델](sql-database-service-tiers-vcore.md)을 통해 vCore 개수, 크기나 메모리 및 저장소의 크기와 속도를 선택할 수 있습니다.
매달 적은 비용으로 작은 단일 데이터베이스에 첫 번째 앱을 빌드한 다음 언제든지 수동 또는 프로그래밍 방식으로 서비스 계층을 변경하여 솔루션의 요구 사항을 충족시킬 수 있습니다. 앱이나 고객에게 가동 중지 시간 없이 성능을 조정할 수 있습니다. 동적 확장성을 통해 데이터베이스는 급변하는 리소스 요구 사항에 투명하게 대응할 수 있으며, 필요할 때 필요한 리소스에 대해서만 비용을 지불할 수 있습니다.


> [!NOTE]
> 동적 확장성은 자동 크기 조정과 다릅니다. 자동 크기 조정은 서비스가 조건에 따라 자동으로 크기를 조정하는 경우인 반면 동적 확장성은 가동 중지 시간 없이 수동 크기 조정을 허용합니다.
>


단일 Azure SQL Database는 수동 동적 확장성을 지원하지만 자동 크기 조정은 지원하지 않습니다. 더 많은 *자동* 환경은 데이터베이스에서 개별 데이터베이스 요구 사항에 따라 풀에 리소스를 공유하도록 허용하는 탄력적 풀을 사용하는 것이 좋습니다.
그러나 단일 Azure SQL Database에 대한 확장성을 자동화할 수 있는 스크립트가 있습니다. 예제는 [PowerShell을 사용하여 단일 SQL Database 모니터링 및 크기 조정](scripts/sql-database-monitor-and-scale-database-powershell.md)을 참조하세요.


Azure SQL Database의 세 가지 버전은 모두 데이터베이스 크기를 동적으로 조정할 수 있는 기능을 제공합니다.
-   [Azure SQL 단일 데이터베이스](sql-database-single-database-scale.md)에서는 [DTU](sql-database-dtu-resource-limits-single-databases.md) 또는 [vCore](sql-database-vcore-resource-limits-single-databases.md) 모델 중 하나를 사용하여 각 데이터베이스에 할당되는 최대 리소스 양을 정의할 수 있습니다.
-   [Azure SQL 관리되는 인스턴스](sql-database-managed-instance.md)는 [vCores](/azure/sql-database/sql-database-managed-instance#vcore-based-purchasing-model-preview) 모드를 사용하며, 인스턴스에 할당되는 최대 CPU 코어 수와 최대 저장소 수를 정의할 수 있습니다. 인스턴스 내의 모든 데이터베이스가 인스턴스에 할당된 리소스를 공유합니다.
-   [Azure SQL 탄력적 풀](sql-database-elastic-pool-scale.md)에서는 풀의 데이터베이스 그룹당 최대 리소스 한도를 정의할 수 있습니다.

## <a name="alternative-scale-methods"></a>대체 크기 조정 방법
리소스 크기 조정은 데이터베이스 또는 응용 프로그램 코드를 변경하지 않고 데이터베이스 성능을 향상하는 가장 쉽고 효과적인 방법입니다.
경우에 따라 가장 높은 성능 계층과 성능 최적화를 사용해도 워크로드가 성공적이고 비용 효과적으로 처리되지 않을 수 있습니다. 이 경우, 데이터베이스 크기를 조정하는 다른 옵션이 있습니다.
-   [읽기 확장](sql-database-read-scale-out.md)은 데이터의 읽기 전용 복제본이 한 개 있고, 보고서 등 까다로운 읽기 전용 쿼리를 실행할 수 있는 경우에 사용할 수 있는 기능입니다. 읽기 전용 복제본은 주 데이터베이스의 리소스 사용량에 영향을 주지 않고 읽기 전용 워크로드를 처리합니다.
-   [데이터베이스 분할](sql-database-elastic-scale-introduction.md)은 데이터를 여러 데이터베이스로 분할하고 독립적으로 크기를 조정할 수 있는 기술 집합입니다.

## <a name="next-steps"></a>다음 단계
- 데이터베이스 코드를 변경하여 데이터베이스 성능을 향상하는 방법에 대한 자세한 내용은 [성능 권장 사항 찾기 및 적용](sql-database-advisor-portal.md)을 참조하세요.
- 기본 제공 데이터베이스 인텔리전스를 통해 데이터베이스를 최적화하는 방법에 대한 자세한 내용은 [자동 조정](sql-database-automatic-tuning.md)을 참조하세요.
- Azure SQL Database 서비스의 읽기 확장에 대한 자세한 내용은 [읽기 전용 복제본을 사용하여 읽기 전용 쿼리 워크로드의 부하를 분산](sql-database-read-scale-out.md)하는 방법을 참조하세요.
- 데이터베이스 분할에 대한 자세한 내용은 [Azure SQL Database를 사용하여 확장](sql-database-elastic-scale-introduction.md)을 참조하세요.

