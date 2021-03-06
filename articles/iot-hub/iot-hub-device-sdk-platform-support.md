---
title: Azure IoT 장치 SDK 플랫폼 지원 | Microsoft Docs
description: 개념 - Azure IoT 장치 SDK에서 지원하는 플랫폼 목록
author: yzhong94
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: yizhon
ms.openlocfilehash: cf3c80424c4626b62317bda537f9491cafc8198c
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043602"
---
# <a name="azure-iot-sdks-platform-support"></a>Azure IoT SDK 플랫폼 지원

[Azure IoT SDK](iot-hub-devguide-sdks.md)는 광범위한 언어 및 플랫폼 지원을 통해 IoT Hub 및 Device Provisioning Service와 상호 작용하는 라이브러리 집합입니다.  SDK는 가장 일반적인 플랫폼에서 실행되며, 개발자는 [이식 지침](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)에 따라 C SDK를 특정 플랫폼에 이식할 수 있습니다. 

Microsoft는 다양한 운영 체제/플랫폼/프레임워크를 지원하며, Azure IoT C SDK를 사용하여 확장할 수 있습니다.  일부는 팀에서 공식적으로 지원하며, 사용자에게 필요한 지원 수준을 나타내는 계층으로 그룹화됩니다.  완벽하게 지원되는 플랫폼은 Microsoft에서 다음을 보장한다는 것을 의미합니다.
    - 마스터 및 LTS 지원 버전에 대한 지속적인 빌드 및 통합형 테스트
    - 해당되는 경우 설치 가이드 또는 패키지 제공
    - GitHub에 대한 완벽한 지원

또한 파트너 목록을 통해 C SDK를 더 많은 플랫폼에 이식했으며 PAL(플랫폼 추상화 계층)을 유지 관리하고 있습니다.  [IoT용 Azure Certified 장치 카탈로그](https://catalog.azureiotsolutions.com/)에는 다양한 SDK가 테스트된 OS 플랫폼 목록도 있습니다.  또한 SDK는 제한된 테스트 및 지원을 통해 다음 플랫폼에서 정기적으로 구축됩니다.
- MBED2
- Arduino
- Windows CE 2013(2018년 10월 사용 중단)
- .NET Standard 1.3(.NET Core 2.1 및 .NET Framework 4.7 포함)
- Xamarin iOS, Android, UWP
- Android(Java 포함)

## <a name="supported-platforms"></a>지원되는 플랫폼
### <a name="c-sdk"></a>C SDK
| OS                  | 아키텍처 | 컴파일러             | TLS 라이브러리       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | gcc-5.4.0            | openssl - 1.0.2g |
| Ubuntu 18.04 LTS    | X64  | gcc-7.3              | WolfSSL – 1.13    |
| Ubuntu 18.04 LTS    | X64  | Clang 6.0.X          | Openssl – 1.1.0g  |
| OSX 10.13.4         | x64  | XCode 9.4.1          | 네이티브 OSX        |
| Windows Server 2016 | x64  | Visual Studio 14.0.X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0.X | SChannel          |
| Debian 9 Stretch    | x64  | gcc-7.3              | Openssl – 1.1.0f  |

### <a name="python-sdk"></a>Python SDK
| OS                  | 아키텍처 | 컴파일러   | TLS 라이브러리 |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2.7 | openssl     |
| Windows Server 2016 | x64  | Python 2.7 | openssl     |
| Windows Server 2016 | x86  | Python 3.5 | openssl     |
| Windows Server 2016 | x64  | Python 3.5 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 2.7 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 3.4 | openssl     |
| MacOS High Sierra   | x64  | Python 2.7 | openssl     |

### <a name="net-sdk"></a>.NET SDK
| OS                  | 아키텍처 | 프레임워크            | Standard          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET Core 2.1        | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Core 2.1        | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.7   | .NET Standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.5.1 | 해당 없음               |

### <a name="nodejs-sdk"></a>Node.js SDK
| OS                                           | 아키텍처 | 노드 버전 |
|----------------------------------------------|------|--------------|
| Ubuntu 16.04 LTS(Node 6 Docker 이미지 사용) | X64  | Node 6       |
| Windows Server 2016                          | X64  | Node 6       |

### <a name="java-sdk"></a>Java SDK
| OS                  | 아키텍처 | Java 버전 |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |

## <a name="partner-supported-platforms"></a>파트너 지원 플랫폼
| 파트너             | 장치                            | 링크                     | 지원 |
|---------------------|------------------------------------|--------------------------|---------|
| Qualcomm            | Qualcomm MDM9206 LTE IoT 모뎀     | [IoT SDK용 Qualcomm LTE](https://developer.qualcomm.com/software/lte-iot-sdk) | [포럼](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | STM32L4 시리즈, STM32F4 시리즈      | [X-CUBE-CLOUD](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html)             | [지원](https://www.st.com/content/st_com/en/support/support-home.html) |
|                     | STM32F7 시리즈                     | [X-CUBE-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html)             |         |
|                     | IoT 노드용 STM32L4 Discovery Kit | [P-NUCLEO-AZURE](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html)          |         |
|                     |                                    | [FP-CLD-AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            |         |
| Espressif           | ESP32                              | [Esp-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  |

## <a name="next-steps"></a>다음 단계
- [장치 및 서비스 SDK](iot-hub-devguide-sdks.md)
- [이식 지침](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)