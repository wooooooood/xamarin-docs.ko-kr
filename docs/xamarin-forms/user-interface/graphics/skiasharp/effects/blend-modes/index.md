---
title: SkiaSharp blend 모드
description: Blend 모드를 사용 하 여 그래픽 개체가 서로 쌓이는 경우 수행할 작업을 정의 합니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CE1B222E-A2D0-4016-A532-EC1E59EE3D6B
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 829d764f03dd77c6126c2f4bced750ae570a3bc6
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83425691"
---
# <a name="skiasharp-blend-modes"></a>SkiaSharp blend 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

이러한 문서는의 속성에 중점을 둡니다 [`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode) [`SKPaint`](xref:SkiaSharp.SKPaint) . `BlendMode`속성은 형식이 며 [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) 29 개 멤버를 포함 하는 열거형입니다.

`BlendMode`속성은 그래픽 개체 (일반적으로 _소스가_라고도 함)가 기존 그래픽 개체 ( _대상_이라고 함) 위에 렌더링 되는 경우 발생 하는 결과를 결정 합니다. 일반적으로 새 그래픽 개체는 그 아래의 개체를 숨기는 것으로 간주 됩니다. 하지만이는 기본 blend 모드가 이기 때문에 발생 합니다 `SKBlendMode.SrcOver` . 즉, 소스가 대상 _위에_ 그려집니다. 의 다른 28 멤버는 `SKBlendMode` 다른 효과를 발생 시킵니다. 그래픽 프로그래밍에서는 다양 한 방법으로 그래픽 개체를 결합 하는 기술을 _합성_이라고 합니다.

## <a name="the-skblendmodes-enumeration"></a>서 용 Blendmode 열거형

SkiaSharp blend 모드는 W3C [**합성 및 혼합 수준 1**](https://www.w3.org/TR/compositing-1/) 사양에 설명 된 것과 유사 합니다. 기능 및 기능에 대 한 [**개요**](https://skia.org/user/api/SkBlendMode_Overview) 도 제공 하는 배경 정보를 제공 합니다. Blend 모드를 일반적으로 소개 하기 위해 위키백과의 [**blend 모드**](https://en.wikipedia.org/wiki/Blend_modes) 문서를 시작 하는 것이 좋습니다. Blend 모드는 Adobe Photoshop에서 지원 되므로 해당 컨텍스트의 blend 모드에 대 한 추가 온라인 정보가 많이 있습니다.

열거형의 29 개 멤버는 `SKBlendMode` 다음과 같은 세 가지 범주로 나눌 수 있습니다.

| Porter-Duff | 분리 가능한    | 분리 가능 하지 않음 |
| ----------- | ------------ | ------------- |
| `Clear`     | `Modulate`   | `Hue`         |
| `Src`       | `Screen`     | `Saturation`  |
| `Dst`       | `Overlay`    | `Color`       |
| `SrcOver`   | `Darken`     | `Luminosity`  |
| `DstOver`   | `Lighten`    |               |
| `SrcIn`     | `ColorDodge` |               |
| `DstIn`     | `ColorBurn`  |               |
| `SrcOut`    | `HardLight`  |               |
| `DstOut`    | `SoftLight`  |               |
| `SrcATop`   | `Difference` |               |
| `DstATop`   | `Exclusion`  |               |
| `Xor`       | `Multiply`   |               |
| `Plus`      |              |               |

이러한 세 범주의 이름은 다음에 나오는 토론에서 더 많은 의미를 갖습니다. 멤버가 나열 되는 순서는 열거형의 정의에서와 동일 합니다 `SKBlendMode` . 첫 번째 열의 13 개 열거형 멤버는 0에서 12 사이의 정수 값을 갖습니다. 두 번째 열은 정수 13 ~ 24에 해당 하는 열거형 멤버이 고, 세 번째 열의 멤버는 25에서 28 사이의 값을 갖습니다.

이러한 blend 모드는 W3C **합성 및 혼합 수준 1** 문서에서 _거의_ 동일한 순서로 설명 되지만 몇 가지 차이점이 있습니다 .이 `Src` 모드를 w3c 문서에서 _복사_ 라고 하며, `Plus` _더 가벼운_이라고 합니다. W3C 문서는 _Normal_ `SKBlendModes` 와 동일 하기 때문에에 포함 되지 않은 일반적인 blend 모드를 정의 합니다 `SrcOver` . `Modulate`첫 번째 열 맨 위에 있는 blend 모드는 W3C 문서에 포함 되지 않으며, 해당 모드에 대 한 설명은 `Multiply` 앞에 나옵니다 `Screen` .

`Modulate`Blend 모드는가 나 ia에서 고유 하므로 추가 Porter-Duff 모드 및 분리 가능 모드로 설명 됩니다.

## <a name="the-importance-of-transparency"></a>투명도의 중요도

지금까지 합성은 _알파 채널_개념과 함께 개발 되었습니다. `SKCanvas`개체 및 전체 색 비트맵과 같은 디스플레이 화면에서 각 픽셀은 4 바이트로 구성 됩니다. 각 픽셀은 빨강, 녹색 및 파랑 구성 요소에 대 한 1 바이트와 투명도를 위한 추가 바이트로 구성 됩니다. 이 알파 구성 요소는 전체 투명도의 경우 0이 고, 전체 불투명도의 경우 0xFF 이며, 해당 값 사이에 서로 다른 수준의 투명도가 있습니다.

대부분의 blend 모드는 투명도를 사용 합니다. 일반적으로 `SKCanvas` 처리기에서를 처음 가져올 때 `PaintSurface` 또는 `SKCanvas` 비트맵에서 그리기 위해를 만들 때 첫 번째 단계는 다음 호출입니다.

```csharp
canvas.Clear();
```

이 메서드는 캔버스의 모든 픽셀을 또는 정수 0x00000000과 동일한 투명 한 검정색 픽셀로 바꿉니다 `new SKColor(0, 0, 0, 0)` . 모든 픽셀의 모든 바이트는 0으로 초기화 됩니다.

`SKCanvas`처리기에서 가져온의 그리기 화면은 `PaintSurface` 흰색 배경을 포함 하는 것 처럼 보일 수 있지만 `SKCanvasView` 자체는 투명 한 배경을 가지 며 페이지에는 흰색 배경이 있습니다. `BackgroundColor`의 xamarin.ios 속성을 xamarin.ios 색으로 설정 하 여이 사실을 직접 보여 줄 수 있습니다 `SKCanvasView` .

```csharp
canvasView.BackgroundColor = Color.Red;
```

또는에서 파생 되는 클래스에서 `ContentPage` 페이지 배경색을 설정할 수 있습니다.

```csharp
BackgroundColor = Color.Red;
```

SkiaSharp canvas 자체가 투명 하기 때문에 SkiaSharp 그래픽 뒤에이 빨간색 배경이 표시 됩니다.

[**SkiaSharp 투명도**](../../basics/transparency.md) 문서에서는 투명도를 사용 하 여 여러 그래픽을 복합 이미지에 정렬 하는 몇 가지 기본적인 기법을 보여 주었습니다. Blend 모드는 그 이상으로 보이지만 blend 모드에는 투명도가 매우 중요 합니다.

## <a name="skiasharp-porter-duff-blend-modes"></a>[SkiaSharp Porter-Duff blend 모드](porter-duff.md)

Porter-Duff blend 모드를 사용 하 여 원본 및 대상 이미지를 기반으로 장면을 작성 합니다.

## <a name="skiasharp-separable-blend-modes"></a>[SkiaSharp 분리 가능 블렌드 모드](separable.md)

빨강, 녹색 및 파랑 색을 변경 하려면 분리 가능 blend 모드를 사용 합니다.

## <a name="skiasharp-non-separable-blend-modes"></a>[SkiaSharp 비 분리 혼합 모드](non-separable.md)

분리 되지 않은 혼합 모드를 사용 하 여 색상, 채도 또는 광도를 변경 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
