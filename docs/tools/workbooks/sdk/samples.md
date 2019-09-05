---
title: 샘플 통합
description: 이 문서에서는 Xamarin Workbooks 통합을 보여 주는 샘플에 연결 합니다. 연결 된 샘플은 표현 렌더링 및 SkiaSharp 사용 됩니다.
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: fbe471aa7f08d85a870d68505cf2c983b7e442e9
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292808"
---
# <a name="sample-integrations"></a>샘플 통합

통합의 작업 예제는 [부엌 싱크][KitchenSink] 샘플을 참조 하세요. Mac용 Visual Studio 또는 `KitchenSink.sln` Visual Studio에서 빌드한 다음를 엽니다. `KitchenSink.workbook`

[![부엌 싱크 통합 스크린샷](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

부엌 Sink 샘플에서는 두 가지 개념 집합을 보여 줍니다.

* 표현 부분에서는를 사용 `RepresentationManager` 하 여 기본 제공 표현을 사용 하 여 렌더링을 향상 시키는 방법을 보여 줍니다.
* 개체 `Person` 및 연결 된 JavaScript 렌더러에서는 표현 공급자 `ISerializableObject` 를 통하지 않고를 사용 하는 방법을 보여 줍니다.

또한 Xamarin Workbooks에서 제공 하는 기존 [표현을](~/tools/workbooks/sdk/representations.md) 사용 하 여 해당 유형을 렌더링 하는 통합의 실제 예는 [SkiaSharp][skiasharp] 을 참조 하세요.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks
