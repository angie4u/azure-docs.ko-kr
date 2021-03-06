---
title: Azure SQL Database 리소스 제한 개요 | Microsoft Docs
description: 이 페이지에서는 Azure SQL Database에서 단일 데이터베이스에 대한 몇 가지 일반적인 DTU 기반 리소스 제한을 설명합니다.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: carlrab
ms.openlocfilehash: 6f6fa1ebc086530f138d32ee5a9c799b5bfbbdeb
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39412113"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Azure SQL Database 리소스 제한 개요 

이 문서에서는 Azure SQL 데이터베이스 리소스 제한 개요를 제공하고 해당 리소스 제한에 도달하거나 초과할 경우 발생하는 사건에 대한 정보를 제공합니다.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>서버 및 데이터베이스의 최대 개수는 어떻게 되나요?

| 최대 | 값 |
| :--- | :--- |
| 서버당 데이터베이스 | 5,000 |
| 모든 지역에서 구독당 서버의 기본 수 | 20 |
| 모든 지역에서 구독당 서버의 최대 수 | 200 |
| 서버당 DTU/eDTU 할당량 | 54,000 |
| 서버당 vCore 할당량 | 540 |
| 서버당 최대 풀 | DTU 또는 vCore 수로 제한 |
|||

> [!NOTE]
> 기본 양보다 더 많은 DTU/eDTU 할당량, vCore 할당량 또는 더 많은 서버를 얻으려면 문제 유형 "할당량"을 사용하여 구독을 위한 Azure Portal에서 새 지원 요청을 제출할 수 있습니다. 서버당 DTU/eDTU 할당량 및 데이터베이스 제한은 서버당 탄력적 풀의 수를 제한합니다. 

> [!IMPORTANT]
> 데이터베이스 수가 서버당 제한에 근접하면 다음이 발생할 수 있습니다.
> - 마스터 데이터베이스에 대해 쿼리를 실행할 때 대기 시간이 증가합니다.  여기에는 sys.resource_stats와 같은 리소스 사용률 통계 보기가 포함됩니다.
> - 관리 작업을 수행하고 서버의 데이터베이스 열거와 관련된 포털 뷰 포인트를 렌더링할 때 대기 시간이 증가합니다.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>데이터베이스 리소스 한계에 도달하면 어떻게 되나요?

### <a name="compute-dtus-and-edtus--vcores"></a>Compute(DTU 및 eDTU / vCore)

데이터베이스 계산 사용량(DTU 및 eDTU 또는 vCore로 측정)이 높아지면 쿼리 대기 시간이 늘어나고 시간이 초과될 수 있습니다. 이러한 상황에서는 쿼리가 서비스에서 대기될 수 있으며 리소스가 사용 가능해질 경우 실행될 수 있게 리소스가 제공됩니다.
높은 계산 사용률에 도달할 경우 완화하는 방법에는 다음이 포함됩니다.

- 데이터베이스 또는 탄력적 풀의 성능 수준을 높여 데이터베이스에 더 많은 계산 리소스를 제공합니다. [단일 데이터베이스 리소스 확장](sql-database-single-database-scale.md) 및 [탄력적 풀 리소스 확장](sql-database-elastic-pool-scale.md)을 참조하세요.
- 쿼리를 최적화하여 각 쿼리당 리소스 사용률을 줄입니다. 자세한 내용은 [쿼리 튜닝/힌트](sql-database-performance-guidance.md#query-tuning-and-hinting)를 참조하세요.

### <a name="storage"></a>Storage

사용된 데이터베이스 공간이 최대 크기 제한에 도달하면 데이터 크기 증가를 가져오는 데이터베이스 삽입 및 업데이트가 실패하고 클라이언트에 [오류 메시지](sql-database-develop-error-messages.md)가 표시됩니다. 데이터베이스 선택 및 삭제는 계속 성공적으로 수행됩니다.

높은 공간 사용률에 도달할 경우 완화하는 방법에는 다음이 포함됩니다.

- 데이터베이스 또는 탄력적 풀의 최대 크기를 늘리거나 더 많은 저장소를 추가합니다. [단일 데이터베이스 리소스 확장](sql-database-single-database-scale.md) 및 [탄력적 풀 리소스 확장](sql-database-elastic-pool-scale.md)을 참조하세요.
- 데이터베이스가 탄력적 풀에 속하는 경우 데이터베이스를 풀 외부로 이동하여 저장소 공간이 다른 데이터베이스와 공유되지 않도록 할 수 있습니다.
- 데이터베이스를 축소하여 사용하지 않는 공간을 회수합니다. 자세한 내용은 [Azure SQL Database의 파일 공간 관리](sql-database-file-space-management.md)를 참조하세요.

### <a name="sessions-and-workers-requests"></a>세션 및 작업자(요청) 

세션 및 작업자의 최대 수는 서비스 계층 및 성능 수준(DTU 및 eDTU)에 따라 결정됩니다. 세션 또는 작업자 제한에 도달하면 새 요청이 거부되고 클라이언트에 오류 메시지가 표시됩니다. 사용할 수 있는 연결의 수는 응용 프로그램에서 쉽게 제어할 수 있지만, 동시 작업자 수는 예측하고 제어하기가 어렵습니다. 좀 더 오래 실행되는 쿼리로 인해 데이터베이스 리소스 제한에 도달하고 작업자가 대기하게 되는 최대 부하 기간이 특히 그렇습니다. 

높은 세션 또는 작업자 사용률에 도달할 경우 완화하는 방법에는 다음이 포함됩니다.
- 데이터베이스 또는 탄력적 풀의 서비스 계층 또는 성능 수준을 늘립니다. [단일 데이터베이스 리소스 확장](sql-database-single-database-scale.md) 및 [탄력적 풀 리소스 확장](sql-database-elastic-pool-scale.md)을 참조하세요.
- 계산 리소스에 대한 경합 때문에 작업자 사용률이 증가할 경우 쿼리를 최적화하여 각 쿼리의 리소스 사용률을 줄입니다. 자세한 내용은 [쿼리 튜닝/힌트](sql-database-performance-guidance.md#query-tuning-and-hinting)를 참조하세요.

## <a name="next-steps"></a>다음 단계

- 자주 묻는 질문에 대한 답변은 [SQL Database FAQ](sql-database-faq.md)를 참조하세요.
- 일반 Azure 제한에 대한 자세한 내용은 [Azure 구독 및 서비스 제한, 할당량 및 제약 조건](../azure-subscription-service-limits.md)을 참조하세요.
- DTU 및 eDTU에 대한 자세한 내용은 [DTU 및 eDTU](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)를 참조하세요.
- tempdb 크기 한도에 대한 자세한 내용은 https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database를 참조하세요.
