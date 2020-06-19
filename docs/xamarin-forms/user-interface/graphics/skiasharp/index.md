---
title: SkiaSharp 그래픽Xamarin.Forms
description: 'SkiaSharp는 Google 제품에서 광범위 하 게 사용 되는 오픈 소스 Kia 그래픽 엔진으로 구동 되는 .NET 및 c # 용 2D 그래픽 시스템입니다. 이 가이드에서는 응용 프로그램에서 2D 그래픽에 SkiaSharp를 사용 하는 방법을 설명 합니다 Xamarin.Forms .'
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 855bd0d357950b019487b3ea05e379915f54b9d4
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127636"
---
# <a name="skiasharp-graphics-in-xamarinforms"></a>SkiaSharp 그래픽Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_응용 프로그램에서 2D 그래픽에 SkiaSharp 사용 Xamarin.Forms_

SkiaSharp는 Google 제품에서 광범위 하 게 사용 되는 오픈 소스 Kia 그래픽 엔진으로 구동 되는 .NET 및 c # 용 2D 그래픽 시스템입니다. 응용 프로그램에서 SkiaSharp를 사용 Xamarin.Forms 하 여 2d 벡터 그래픽, 비트맵 및 텍스트를 그릴 수 있습니다. SkiaSharp 라이브러리 및 기타 자습서에 대 한 일반적인 정보는 [2D 그리기](~/graphics-games/skiasharp/index.md) 가이드를 참조 하세요.

이 가이드에서는 사용자가 프로그래밍에 대해 잘 알고 있다고 가정 Xamarin.Forms 합니다.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**웹 세미나: SkiaSharpXamarin.Forms**

## <a name="skiasharp-preliminaries"></a>SkiaSharp Preliminaries

SkiaSharp Xamarin.Forms 는 NuGet 패키지로 패키지 됩니다. Xamarin.FormsVisual Studio 또는 Mac용 Visual Studio에서 솔루션을 만든 후에는 NuGet 패키지 관리자를 사용 하 여 **SkiaSharp** 패키지를 검색 하 고 솔루션에 추가할 수 있습니다. SkiaSharp를 추가한 후 각 프로젝트의 **참조** 섹션을 확인 하는 경우 솔루션의 각 프로젝트에 다양 한 **SkiaSharp** 라이브러리가 추가 된 것을 볼 수 있습니다.

Xamarin.Forms응용 프로그램이 ios를 대상으로 하는 경우 프로젝트 속성 페이지를 사용 하 여 최소 배포 대상을 ios 8.0로 변경 합니다.

SkiaSharp를 사용 하는 c # 페이지에서 `using` 네임 스페이스에 대 한 지시문을 포함 하 여 [`SkiaSharp`](xref:SkiaSharp) 그래픽 프로그래밍에 사용할 모든 SkiaSharp 클래스, 구조체 및 열거형을 포함 시킬 수 있습니다. 또한 `using` [`SkiaSharp.Views.Forms`](xref:SkiaSharp.Views.Forms) 에 관련 된 클래스에 대 한 네임 스페이스에 대 한 지시문이 필요 Xamarin.Forms 합니다. 이는 훨씬 작은 네임 스페이스 이며, 가장 중요 한 클래스는 [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) 입니다. 이 클래스는 클래스에서 파생 Xamarin.Forms `View` 되며 SkiaSharp 그래픽 출력을 호스팅합니다.

> [!IMPORTANT]
> `SkiaSharp.Views.Forms`네임 스페이스에는 `SKGLView` 에서 파생 되는 클래스도 포함 되어 `View` 있지만 그래픽 렌더링에는 OpenGL을 사용 합니다. 간단한 설명을 위해이 가이드는를로 제한 `SKCanvasView` 하지만 대신를 사용 하 `SKGLView` 는 것은 매우 유사 합니다.

## <a name="skiasharp-drawing-basics"></a>[SkiaSharp 그리기 기본 사항](basics/index.md)

SkiaSharp으로 그릴 수 있는 가장 간단한 그래픽 그림 중 일부는 원, 타원 및 사각형입니다. 이러한 수치를 표시 하는 경우 SkiaSharp 좌표, 크기 및 색에 대해 알아봅니다. 텍스트와 비트맵을 표시 하는 것이 더 복잡 하지만 이러한 문서도 이러한 기술을 소개 합니다.

## <a name="skiasharp-lines-and-paths"></a>[SkiaSharp 선 및 경로](paths/index.md)

그래픽 경로는 일련의 연결 된 직선 및 곡선입니다. 패스는 스트로크, 채우기 또는 둘 다 사용할 수 있습니다. 이 문서에서는 스트로크 끝 및 조인을 포함 하 여 선 그리기의 여러 측면을 포함 하 고, 파선 및 점선으로, 곡선 기 하 도형의 짧은 부분을 중지 합니다.

## <a name="skiasharp-transforms"></a>[SkiaSharp 변환](transforms/index.md)

변환을 사용 하면 그래픽 개체를 균등 하 게 변환, 크기 조정, 회전 또는 기울일 수 있습니다. 또한이 문서에서는 표준 3 x 3 변환 매트릭스를 사용 하 여 비 상관 변환을 만들고 경로에 변환을 적용 하는 방법을 보여 줍니다.

## <a name="skiasharp-curves-and-paths"></a>[SkiaSharp 곡선 및 경로](curves/index.md)

경로 탐색은 경로 개체에 곡선을 추가 하 고 다른 강력한 경로 기능을 이용 하 여 계속 됩니다. 간결한 텍스트 문자열에서 전체 경로를 지정 하는 방법, 경로 효과를 사용 하는 방법 및 경로 내부를 자세히 설명 하는 방법을 확인할 수 있습니다.

## <a name="skiasharp-bitmaps"></a>[SkiaSharp 비트맵](bitmaps/index.md)

비트맵은 디스플레이 장치의 픽셀에 해당 하는 비트의 사각형 배열입니다. 이 문서 시리즈에서는 SkiaSharp 비트맵의 비트를 로드, 저장, 표시, 만들기, 그리기 켜기, 애니메이션 및 액세스 하는 방법을 보여 줍니다.

## <a name="skiasharp-effects"></a>[SkiaSharp 효과](effects/index.md)

효과는 선형 및 원형 그라데이션을 비롯 한 그래픽의 일반적인 표시, 비트맵 바둑판식 배열, 혼합 모드, 흐림 효과 등을 변경 하는 속성입니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [SkiaSharp with Xamarin.Forms 웹 세미나 (비디오)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
