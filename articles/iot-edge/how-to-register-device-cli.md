---
title: 새 Azure IoT Edge 장치 등록(CLI) | Microsoft Docs
description: Azure CLI 2.0용 IoT 확장을 사용하여 새 IoT Edge 장치 등록
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 07/27/2018
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 451f4df31cd1c520b14227829923f72fe80c38c3
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325499"
---
# <a name="register-a-new-azure-iot-edge-device-with-azure-cli-20"></a>Azure CLI 2.0을 사용하여 새 Azure IoT Edge 장치 등록

Azure IoT Edge에서 IoT 장치를 사용하려면 먼저 IoT Hub에 등록해야 합니다. 장치를 등록하면 에지 워크로드에 대해 장치를 설정하는 데 사용할 수 있는 연결 문자열을 받게 됩니다. 

[Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)은 IoT Edge 같은 Azure 리소스를 관리하기 위한 오픈 소스 교차 플랫폼 명령줄 도구입니다. 이 기능을 사용하면 Azure IoT Hub 리소스, 장치 프로비전 서비스 인스턴스 및 연결된 허브를 즉시 관리할 수 있습니다. 새로운 IoT 확장은 장치 관리 및 전체 IoT Edge 같은 기능으로 Azure CLI 2.0을 강화합니다.

이 아티클에서는 Azure CLI 2.0을 사용하여 새 IoT Edge 장치를 등록하는 방법을 보여줍니다.

## <a name="prerequisites"></a>필수 조건

* Azure 구독의 [IoT Hub](../iot-hub/iot-hub-create-using-cli.md) 
* 사용자 환경의 [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli). Azure CLI 2.0 버전이 2.0.24 이상이어야 합니다. `az –-version` 명령을 사용하여 유효성을 검사합니다. 이 버전은 az extension 명령을 지원하며 Knack 명령 프레임워크를 도입했습니다. 
* [Azure CLI 2.0에 대한 IoT 확장](https://github.com/Azure/azure-iot-cli-extension).

## <a name="create-a-device"></a>장치 만들기

다음 명령을 사용하여 IoT Hub에서 새 장치 ID를 만듭니다. 

   ```cli
   az iot hub device-identity create --device-id [device id] --hub-name [hub name] --edge-enabled
   ```

이 명령에는 세 개의 매개 변수가 포함됩니다.
* **device-id**: IoT Hub에 고유한 설명 이름을 제공합니다.
* **hub-name**: IoT Hub의 이름을 제공합니다.
* **edge-enabled**: 이 매개 변수는 장치를 IoT Edge에서 사용한다고 선언합니다.

   ![IoT Edge 장치 만들기](./media/how-to-register-device-cli/Create-edge-device.png)

## <a name="view-all-devices"></a>모든 장치 보기

다음 명령을 사용하여 IoT Hub에서 모든 장치를 봅니다.

   ```cli
   az iot hub device-identity list --hub-name [hub name]
   ```

IoT Edge 장치로 등록된 모든 장치에서는 **capabilities.iotEdge** 속성이 **true**로 설정됩니다. 

## <a name="retrieve-the-connection-string"></a>연결 문자열 검색

장치를 설정할 준비가 되면, 물리적 장치를 IoT Hub에 있는 해당 ID와 연결하는 연결 문자열이 필요합니다. 다음 명령을 실행하여 단일 장치에 대한 연결 문자열을 반환합니다.

   ```cli
   az iot hub device-identity show-connection-string --device-id [device id] --hub-name [hub name] 
   ```

장치 ID 매개 변수는 대소문자를 구분합니다. 연결 문자열 앞뒤의 따옴표를 복사하지 마세요. 

## <a name="next-steps"></a>다음 단계

[Azure CLI 2.0을 사용하여 장치에 모듈을 배포](how-to-deploy-modules-cli.md)하는 방법을 알아봅니다.