---
title: Xamarin.forms에서 SkiaSharp를 사용 하 여
description: SkiaSharp Xamarin.Forms 응용 프로그램에서 2 차원 그래픽을 사용 하 여
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
ms.openlocfilehash: 2183601ca6e642b2fb21f640ed4fddfe2c300dc8
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="using-skiasharp-in-xamarinforms"></a>Xamarin.forms에서 SkiaSharp를 사용 하 여

_SkiaSharp Xamarin.Forms 응용 프로그램에서 2 차원 그래픽을 사용 하 여_

SkiaSharp는.NET 및 C# Google 제품에서 광범위 하 게 사용 되는 오픈 소스 Skia 그래픽 엔진을 통해 수행에 대 한 2 차원 그래픽 시스템입니다. 2 차원 벡터 그래픽, 비트맵 및 텍스트를 그리는 데 SkiaSharp Xamarin.Forms 응용 프로그램에서 사용할 수 있습니다. 참조는 [2D 드로잉](~/graphics-games/skiasharp/index.md) SkiaSharp 라이브러리에 대 한 자세한 내용 및 일부 다른 자습서에 대 한 가이드입니다.

이 가이드에서는 Xamarin.Forms 프로그래밍에 익숙한 가정 합니다.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Xamarin.Forms에 대 한 웹 세미나: SkiaSharp**

## <a name="skiasharp-preliminaries"></a>SkiaSharp 준비 단계

Xamarin.Forms에 대해 SkiaSharp NuGet 패키지로 패키지 됩니다. Mac 용 Visual Studio 또는 Visual Studio에서 Xamarin.Forms 솔루션을 만든, 다음을 찾으려고 NuGet 패키지 관리자를 사용할 수 있습니다는 **SkiaSharp.Views.Forms** 패키지 및 솔루션에 추가 합니다. 선택 하는 경우는 **참조** 섹션 SkiaSharp를 추가한 후 각 프로젝트의 함을 확인할 수 있습니다 다양 한 **SkiaSharp** 라이브러리 각 솔루션의 프로젝트에 추가 되었습니다.

Xamarin.Forms 응용 프로그램에서 iOS를 대상으로 하는 경우 프로젝트 속성 페이지를 사용 하 여 iOS 8.0에 최소 배포 대상을 변경할 수 있습니다.

SkiaSharp를 사용 하는 C# 페이지에 포함 해야는 `using` 에 대 한 지시문은 [ `SkiaSharp` ](https://developer.xamarin.com/api/namespace/SkiaSharp/) 모든 SkiaSharp 클래스, 구조체 및 그래픽에서 사용 하는 열거형을 포괄 하는 네임 스페이스 프로그래밍합니다. 도 한 `using` 지시문에 대 한는 [ `SkiaSharp.Views.Forms` ](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) xamarin.forms 특정 클래스에 대 한 네임 스페이스입니다. 이 훨씬 더 작은 네임 스페이스 되 고 가장 중요 한 클래스는 [ `SKCanvasView` ](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/)합니다. 이 클래스는 Xamarin.Forms에서 파생 `View` 클래스 및 SkiaSharp 그래픽 출력을 호스트 합니다.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms` 네임 스페이스에 포함 된 `SKGLView` 에서 파생 된 클래스 `View` 이지만 그래픽 렌더링을 위해 OpenGL을 사용 합니다. 에 대 한 간단한 설명을 위해가이 가이드를 자체 제한 `SKCanvasView`, 하지만 사용 하 여 `SKGLView` 대신 매우 비슷합니다.

## <a name="skiasharp-drawing-basicsbasicsindexmd"></a>[SkiaSharp 그리기 기본 사항](basics/index.md)

일부 SkiaSharp를 그릴 수는 가장 간단한 그래픽 수치 원, 타원 및 사각형은 있습니다. 이러한 수치를 표시할 SkiaSharp 좌표, 크기 및 색에 대 한 배우게 됩니다.

## <a name="skiasharp-lines-and-pathspathsindexmd"></a>[SkiaSharp 선 및 경로](paths/index.md)

그래픽 경로 일련의 연결 된 직선 및 곡선입니다. 경로 수 스트로크를 가득 차면 또는 둘 다 합니다. 이 항목은 선 끝과 조인을 포함 하 고 파선 선 그리기 및 점선을 하지만 곡선 기 하 도형 있게 하지는 않습니다의 여러 측면을 포함 합니다.

## <a name="skiasharp-transformstransformsindexmd"></a>[SkiaSharp 변환](transforms/index.md)

변환을 균일 하 게 변환, 조정, 회전 또는 일치 하지 않아 그래픽 개체를 허용 합니다. 이 문서에는 유사 형식이 아닌 변형 만들고을 경로로 변환 적용 하기 위한 표준 3-3 변환 매트릭스를 사용 하는 방법을 보여 줍니다.

## <a name="skiasharp-curves-and-pathscurvesindexmd"></a>[SkiaSharp 곡선 및 경로](curves/index.md)

경로의 탐색 커브 경로 개체에 추가 되 고 다른 강력한 경로 기능을 이용 계속 됩니다. 간결한 텍스트 문자열의 전체 경로 지정 하는 방법, 경로 효과 사용 하는 방법 및 경로 내부 자세히 검토 하는 방법을 볼 수 있습니다.


## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Xamarin.Forms 웹 세미나 (비디오)와 SkiaSharp](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
