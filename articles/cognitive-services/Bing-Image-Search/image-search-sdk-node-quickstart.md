---
title: Image search SDK Node 빠른 시작 | Microsoft Docs
description: Image Search SDK 콘솔 응용 프로그램을 설치합니다.
titleSuffix: Azure cognitive services
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/12/2018
ms.author: v-gedod
ms.openlocfilehash: e4c8303e39accbb7caec15c0ef47d701971ce632
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35377902"
---
# <a name="image-search-sdk-node-quickstart"></a>Image search SDK Node 빠른 시작

Bing Image Search SDK는 이미지 쿼리 및 구문 분석 결과에 대한 REST API 기능을 포함하고 있습니다. 

[Node Bing Image Search SDK 소스 코드 샘플](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js)은 Git Hub에서 얻을 수 있습니다.

## <a name="application-dependencies"></a>응용 프로그램 종속성

Bing Image Search SDK를 사용하여 콘솔 응용 프로그램을 설치하려면 개발 환경에서 `npm install azure-cognitiveservices-imagesearch` 명령을 실행합니다.

## <a name="image-search-client"></a>Image Search 클라이언트
*검색* 아래에서 [Cognitive Services 액세스 키](https://azure.microsoft.com/try/cognitive-services/)를 가져옵니다. `CognitiveServicesCredentials` 인스턴스를 만듭니다.
```
const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
```
그런 다음, 클라이언트를 인스턴스화합니다.
```
const ImageSearchAPIClient = require('azure-cognitiveservices-imagesearch');
let client = new ImageSearchAPIClient(credentials);
```
이 클라이언트를 사용하여 쿼리 텍스트(이 경우 'El Capitan')로 검색합니다.
```
client.imagesOperations.search('El Capitan', function (err, result, request, response) {
    if (err) throw err;
    console.log(result.value);
});

```
<!-- Need to sanitize result
The code prints `result.value` items to the console without parsing any text. The results will be:
- _type: 'ImageObjectElementType'

![Imageresults](media/node-sdk-quickstart-image-results.png)
-->

## <a name="next-steps"></a>다음 단계

[Cognitive Services Node.js SDK 샘플](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)