---
title: SkiaSharp blend 모드
description: 그래픽 개체는 서로 누적 하는 경우 수행 하는 작업을 정의 하는 모드를 혼합 하는 사용 됩니다.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CE1B222E-A2D0-4016-A532-EC1E59EE3D6B
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 3ea05563ecbca95d26d692d5424c30e961229ac5
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53051045"
---
# <a name="skiasharp-blend-modes"></a>SkiaSharp blend 모드

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

이러한 문서에 집중 합니다 [ `BlendMode` ](xref:SkiaSharp.SKPaint.BlendMode) 속성을 [ `SKPaint` ](xref:SkiaSharp.SKPaint)합니다. 합니다 `BlendMode` 형식의 속성은 [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode), 29 멤버로 구성 된 열거형입니다.

`BlendMode` 속성 결정 하는 경우 그래픽 개체를 (라고도 합니다 _원본_) 기존 그래픽 개체 맨 위에 렌더링 됩니다 (호출 합니다 _대상_). 일반적으로 새 그래픽 개체 아래에 있는 개체를가 려 서 예정입니다. 기본 blend 모드 이므로 이런 하지만 `SKBlendMode.SrcOver`, 즉, 원본 그려집니다 _를 통해_ 대상입니다. 다른 28 멤버 `SKBlendMode` 다른 작업이 발생 합니다. 그래픽 프로그래밍에서는 다양 한 방법으로 그래픽 개체를 결합 하는 기술을 이라고 _합치기_합니다.

## <a name="the-skblendmodes-enumeration"></a>SKBlendModes 열거형

SkiaSharp blend 모드는 W3C에서 설명 하는 것을 정확 하 게 일치 [ **합성 하 고 수준 1 혼합** ](https://www.w3.org/TR/compositing-1/) 사양입니다. Skia [ **SkBlendMode 참조** ](https://skia.org/user/api/SkBlendMode_Reference) 도 유용한 배경 정보를 제공 합니다. 한 모드를 혼합 하려면 일반적인 소개는 [ **혼합 모드** ](https://en.wikipedia.org/wiki/Blend_modes) Wikipedia의 문서는 좋은 출발 합니다. 해당 컨텍스트에서 blend 모드에 대 한 훨씬 추가적인 온라인 정보 이므로 Adobe Photoshop에서 혼합 모드가 지원 됩니다.

29 멤버는 `SKBlendMode` 열거형 세 가지 범주로 나눌 수 있습니다.

| Porter 임신 | 분리 가능한    | 분리 가능한 비 |
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

이러한 세 가지 범주의 이름이 나오는 토론에서 더 많은 의미 수행 합니다. 멤버 여기 나열 된 순서와 동일의 정의 `SKBlendMode` 열거형입니다. 첫 번째 열에서 13 열거형 멤버는 0 ~ 12 정수 값을 갖습니다. 두 번째 열은 정수 13 ~ 24에 해당 하는 열거형 멤버 있고 세 번째 열에 있는 멤버 25 ~ 28의 값입니다.

이러한 혼합 모드에 대해서는 설명 _약_ 과 동일한 순서로 W3C **합성 하 고 수준 1 혼합** 문서는 몇 가지 차이점이 있지만:는 `Src` 모드 라고합니다._복사본_ W3C 문서에서와 `Plus` 라고 _밝은_합니다. W3C 문서 정의 _정규_ 에 포함 되지 않은 혼합 모드 `SKBlendModes` 것이 동일 하기 때문에 `SrcOver`입니다. `Modulate` 혼합 모드 (첫 번째 열의 맨 위에 있음) W3C 문서와의 토론에 포함 되지 합니다 `Multiply` 모드 앞에 오는 `Screen`합니다.

때문에 `Modulate` blend 모드가 Skia에 고유한, 추가 Porter 임신 모드를와 분리 가능 모드를 설명 합니다.

## <a name="the-importance-of-transparency"></a>투명도의 중요성

합성 개념이와 함께에서 지금까지 개발한 합니다 _알파 채널_합니다. 표시에서 화면 같은 `SKCanvas` 빨간색, 녹색 및 파랑 구성 요소에 대 한 각 4 바이트: 1 바이트 및 투명도 대 한 추가 바이트의 각 픽셀 구성 전체 색 비트맵 및 개체입니다. 이 알파 구성 요소에는 완전 한 투명성에 대 한 0에서 0xFF 투명도 값 간의 차이 사용 하 여 전체 불투명도 합니다.

다양 한 혼합 모드 투명도에 의존합니다. 일반적으로 경우는 `SKCanvas` 에 처음으로 가져올를 `PaintSurface` 처리기 때나는 `SKCanvas` 만들어집니다 비트맵에 그릴 첫 번째 단계는이 호출:

```csharp
canvas.Clear();
```

이 메서드 투명 검정 픽셀에 해당 하는 캔버스의 모든 픽셀 바꿉니다 `new SKColor(0, 0, 0, 0)` 또는 0x00000000 정수입니다. 모든 픽셀의 모든 바이트를 0으로 초기화 됩니다.

그리기 화면을 `SKCanvas` 에서 가져온를 `PaintSurface` 처리기 흰색 배경이 있는 것으로 나타날 수 있지만 때문에는 `SKCanvasView` 자체에 투명 한 배경이 있으며 페이지 흰색 배경. Xamarin.Forms를 설정 하 여 자신에 게이 팩트를 시연할 수 있습니다 `BackgroundColor` 속성의 `SKCanvasView` Xamarin.Forms 색:

```csharp
canvasView.BackgroundColor = Color.Red;
```

또는에서 파생 된 클래스 `ContentPage`, 페이지 배경색을 설정할 수 있습니다.

```csharp
BackgroundColor = Color.Red;
```

SkiaSharp 그래픽 뒤 빨간색 배경이이 자체는 SkiaSharp 캔버스 이므로 투명 하 게 볼 수 있습니다.

이 문서 [ **SkiaSharp 투명도** ](../../basics/transparency.md) 투명도 사용 하 여 복합 이미지에 여러 그래픽을 정렬 하려면 몇 가지 기본적인 방법에 설명 했습니다. 그 외에도 혼합 모드 이동 그대로 투명도 blend 모드 하는 데 중요 합니다. 

## <a name="skiasharp-porter-duff-blend-modesporter-duffmd"></a>[SkiaSharp Porter 임신 blend 모드](porter-duff.md)

원본 및 대상 이미지를 기반으로 하는 백그라운드에서 작성 하는 Porter 임신 blend 모드를 사용 합니다.

## <a name="skiasharp-separable-blend-modesseparablemd"></a>[SkiaSharp 분리 가능한 blend 모드](separable.md)

빨강, 녹색 및 파랑 색을 변경 하려면 분리 가능한 혼합 모드를 사용 합니다.

## <a name="skiasharp-non-separable-blend-modesnon-separablemd"></a>[SkiaSharp 분리 되지 않은 혼합 모드](non-separable.md)

색상, 채도 또는 명도 변경 하려면 분리 되지 않은 혼합 모드를 사용 합니다.

## <a name="related-links"></a>관련 링크

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (샘플)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
