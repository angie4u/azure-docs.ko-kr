---
title: Visual Studio Code용 Azure IoT Toolkit 확장을 사용한 Azure IoT 장치 관리 | Microsoft Docs
description: Direct 메서드와 Twin의 desired 속성 관리 옵션을 제공하는 Visual Studio Code용 Azure IoT Toolkit 확장을 사용하여 Azure IoT Hub 장치를 관리합니다.
author: formulahendry
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 8/3/2018
ms.author: junhan
ms.openlocfilehash: ef99f4be97f5168add44d373a7e74de62347d110
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39450273"
---
# <a name="use-azure-iot-toolkit-extension-for-visual-studio-code-for-azure-iot-hub-device-management"></a>Visual Studio Code용 Azure IoT Toolkit 확장을 사용하여 Azure IoT Hub 장치 관리

![종단 간 다이어그램](media/iot-hub-get-started-e2e-diagram/2.png)

[Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)은 IoT Hub 관리를 쉽게 해주는 유용한 Visual Studio Code 확장입니다. 다양한 작업을 수행하는 데 사용할 수 있는 관리 옵션이 제공됩니다.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| 관리 옵션          | Task                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| 직접 메서드             | 메시지 보내기 시작 또는 중지, 장치 다시 부팅 등의 장치 작업을 수행합니다.                                        |
| 장치 쌍 읽기           | 장치의 보고된 상태를 가져옵니다. 예를 들어 장치에서 지금 LED가 깜박이고 있다고 보고합니다.                                    |
| 장치 쌍 업데이트         | 장치를 특정 상태(예: LED를 녹색으로 설정 또는 원격 분석 전송 간격을 30 분으로 설정)로 지정합니다.         |
| 클라우드-장치 메시지   | 장치에 알림을 보냅니다. 예를 들어 "오늘 비가 올 가능성이 매우 높습니다. 반드시 우산을 챙기세요."              |

이러한 옵션을 사용하는 방법에 대한 차이점과 지침에 대한 자세한 내용은 [장치-클라우드 통신 지침](iot-hub-devguide-d2c-guidance.md) 및 [클라우드-장치 통신 지침](iot-hub-devguide-c2d-guidance.md)을 참조하세요.

장치 쌍은 장치의 상태 정보(메타데이터, 상태 및 조건)를 저장하는 JSON 문서입니다. IoT Hub는 여기에 연결하는 각 장치에 대해 하나의 장치 쌍을 유지합니다. 장치 쌍에 대한 자세한 내용은 [장치 쌍 시작](iot-hub-node-node-twin-getstarted.md)을 참조하세요.

## <a name="what-you-learn"></a>학습 내용

배포 머신에서 다양한 관리 옵션으로 Visual Studio Code용 Azure IoT Toolkit 확장을 사용하는 방법에 대해 알아봅니다.

## <a name="what-you-do"></a>수행할 작업

다양한 관리 옵션으로 Visual Studio Code용 Azure IoT Toolkit 확장을 실행합니다.

## <a name="what-you-need"></a>필요한 항목

- 활성 Azure 구독.
- 구독 중인 Azure IoT Hub
- [Visual Studio Code](https://code.visualstudio.com/)
- [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)

## <a name="sign-in-to-access-your-iot-hub"></a>로그인하여 IoT Hub에 액세스

1. **탐색기**에서 VS Code를 보고 왼쪽 아래 모서리에서 **Azure IoT Hub 장치** 섹션을 확장합니다.
1. 상황에 맞는 메뉴에서 **IoT Hub 선택**을 클릭합니다.
1. 처음으로 Azure에 로그인할 수 있게 하는 팝업 메시지가 오른쪽 아래 모서리에 표시됩니다.
1. 로그인 후 Azure 구독 목록이 표시되면 IoT Hub 및 Azure 구독을 선택합니다.
1. 잠시 후 장치 목록이 **Azure IoT Hub 장치** 탭에 표시됩니다.

   > [!Note]
   > **IoT Hub 연결 문자열 설정**을 선택하여 설정을 완료할 수도 있습니다. 팝업 창에서 IoT 장치를 연결할 IoT Hub의 연결 문자열을 입력합니다.

## <a name="direct-methods"></a>직접 메서드

1. 장치를 마우스 오른쪽 단추로 클릭하고 **직접 메서드 호출**을 선택합니다. 
1. 입력 상자에 메서드 이름 및 페이로드를 입력합니다.
1. 결과가 **OUTPUT** > **Azure IoT Toolkit** 보기에 표시됩니다.

## <a name="read-device-twin"></a>장치 쌍 읽기

1. 장치를 마우스 오른쪽 단추로 클릭하고 **장치 쌍 편집**을 선택합니다. 
1. **azure-iot-device-twin.json** 파일은 장치 쌍의 콘텐츠를 사용해 열립니다.

## <a name="update-device-twin"></a>장치 쌍 업데이트

1. **태그** 또는 **properties.desired** 필드의 일부를 편집합니다.
1. **azure-iot-device-twin.json** 파일을 마우스 오른쪽 단추로 클릭합니다.
1. **장치 쌍 업데이트**를 선택하여 장치 쌍을 업데이트합니다.

## <a name="send-cloud-to-device-messages"></a>클라우드-장치 메시지 보내기

IoT Hub에서 장치로 메시지를 보내려면 다음 단계를 수행합니다.
 
1. 장치를 마우스 오른쪽 단추로 클릭하고 **장치에 C2D 메시지 보내기**를 선택합니다. 
1. 입력 상자에 메시지를 입력합니다.
1. 결과가 **OUTPUT** > **Azure IoT Toolkit** 보기에 표시됩니다.

## <a name="next-steps"></a>다음 단계

다양한 관리 옵션으로 Visual Studio Code용 Azure IoT Toolkit 확장을 사용하는 방법에 대해 알아보았습니다.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
