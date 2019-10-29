---
title: 샘플 통합
description: 이 문서에서는 Xamarin Workbooks 통합을 보여 주는 샘플에 연결 합니다. 연결 된 샘플은 표현 렌더링 및 SkiaSharp 사용 됩니다.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: fa66ee2b2b469900381e2d31dc5dec49eb42c4f6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018117"
---
# <a name="sample-integrations"></a>샘플 통합

통합의 작업 예제는 [부엌 싱크][KitchenSink] 샘플을 참조 하세요. Mac용 Visual Studio 또는 Visual Studio에서 `KitchenSink.sln`를 빌드한 다음 `KitchenSink.workbook`를 엽니다.

[![주방 싱크 통합 스크린샷](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

부엌 Sink 샘플에서는 두 가지 개념 집합을 보여 줍니다.

* 표현 부분에서는 `RepresentationManager`를 사용 하 여 기본 제공 표현을 사용 하 여 렌더링을 향상 시키는 방법을 보여 줍니다.
* `Person` 개체 및 관련 JavaScript 렌더러에서는 표현 공급자를 통하지 않고 `ISerializableObject`를 사용 하는 방법을 보여 줍니다.

또한 Xamarin Workbooks에서 제공 하는 기존 [표현을](~/tools/workbooks/sdk/representations.md) 사용 하 여 해당 유형을 렌더링 하는 통합의 실제 예는 [SkiaSharp][skiasharp] 을 참조 하세요.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
