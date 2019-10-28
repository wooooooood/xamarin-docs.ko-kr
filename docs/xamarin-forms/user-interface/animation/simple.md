---
title: Xamarin.ios의 간단한 애니메이션
description: ViewExtensions 클래스는 간단한 애니메이션을 생성 하는 데 사용할 수 있는 확장 메서드를 제공 합니다. 이 문서에서는 ViewExtensions 클래스를 사용 하 여 애니메이션을 만들고 취소 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2019
ms.openlocfilehash: 116911787db128b103fb555554076704a0549db5
ms.sourcegitcommit: f8583585c501607fdfa061b95e9a9f385ed1d591
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2019
ms.locfileid: "72959178"
---
# <a name="simple-animations-in-xamarinforms"></a>Xamarin.ios의 간단한 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)

_ViewExtensions 클래스는 간단한 애니메이션을 생성 하는 데 사용할 수 있는 확장 메서드를 제공 합니다. 이 문서에서는 ViewExtensions 클래스를 사용 하 여 애니메이션을 만들고 취소 하는 방법을 보여 줍니다._

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스는 간단한 애니메이션을 만드는 데 사용할 수 있는 다음과 같은 확장 메서드를 제공 합니다.

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성에 애니메이션을 적용 합니다.
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 애니메이션을 적용 합니다.
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 애니메이션이 적용 된 증분 증가 또는 감소를 적용 합니다.
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성에 애니메이션을 적용 합니다.
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성에 애니메이션이 적용 된 증분 증가 또는 감소를 적용 합니다.
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) 속성에 애니메이션을 적용 합니다.
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) 속성에 애니메이션을 적용 합니다.
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 속성에 애니메이션을 적용 합니다.

기본적으로 각 애니메이션에는 250 밀리초가 소요 됩니다. 그러나 애니메이션을 만들 때 각 애니메이션에 대 한 기간을 지정할 수 있습니다.

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스에는 애니메이션을 취소 하는 데 사용할 수 있는 [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 메서드도 포함 되어 있습니다.

> [!NOTE]
> [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스는 [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드를 제공 합니다. 그러나이 메서드는 레이아웃에서 크기 및 위치 변경 내용이 포함 된 레이아웃 상태 간의 전환에 애니메이션 효과를 주기 위해 사용 됩니다. 따라서 [`Layout`](xref:Xamarin.Forms.Layout) 서브 클래스 에서만 사용 해야 합니다.

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스의 애니메이션 확장 메서드는 모두 비동기 이며 `Task<bool>` 개체를 반환 합니다. 애니메이션이 완료 되 면 반환 값은 `false`이 고 애니메이션이 취소 되 면 `true`입니다. 따라서 애니메이션 메서드는 일반적으로 `await` 연산자와 함께 사용 해야 애니메이션이 완료 된 시기를 쉽게 확인할 수 있습니다. 또한 이전 메서드가 완료 된 후 실행 되는 후속 애니메이션 메서드를 사용 하 여 순차적 애니메이션을 만들 수 있습니다. 자세한 내용은 [복합 애니메이션](#compound)(영문)을 참조 하세요.

배경에서 애니메이션을 완료 해야 하는 요구 사항이 있는 경우 `await` 연산자를 생략할 수 있습니다. 이 시나리오에서 애니메이션 확장 메서드는 애니메이션을 시작한 후 백그라운드에서 발생 하는 애니메이션을 통해 빠르게 반환 됩니다. 복합 애니메이션을 만들 때이 작업을 활용할 수 있습니다. 자세한 내용은 [복합 애니메이션](#composite)(영문)을 참조 하세요.

`await` 연산자에 대 한 자세한 내용은 [Async 지원 개요](~/cross-platform/platform/async.md)를 참조 하세요.

## <a name="single-animations"></a>단일 애니메이션

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 의 각 확장 메서드는 일정 시간 동안 속성을 한 값에서 다른 값으로 점진적으로 변경 하는 단일 애니메이션 연산을 구현 합니다. 이 섹션에서는 각 애니메이션 작업을 살펴봅니다.

### <a name="rotation"></a>회전

다음 코드 예제에서는 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image)의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

이 코드는 2 초 (2000 밀리초) 이상으로 360도를 회전 하 여 [`Image`](xref:Xamarin.Forms.Image) 인스턴스에 애니메이션 효과를 적용 합니다. [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 애니메이션의 시작에 대 한 현재 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성 값을 가져온 다음 해당 값에서 첫 번째 인수 (360)로 회전 합니다. 애니메이션이 완료 되 면 이미지의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성이 0으로 다시 설정 됩니다. 이렇게 하면 애니메이션이 종료 된 후 `Rotation` 속성이 360에 유지 되지 않으므로 추가 회전이 발생 하지 않습니다.

다음 스크린샷에는 각 플랫폼에서 진행 중인 회전이 나와 있습니다.

![](simple-images/rotateto.png "Rotation Animation")

### <a name="relative-rotation"></a>상대 회전

다음 코드 예제에서는 [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image)의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성을 점차적으로 증가 또는 감소 시키는 방법을 보여 줍니다.

```csharp
await image.RelRotateTo (360, 2000);
```

이 코드는 360도를 시작 위치에서 2 초 (2000 밀리초) 이상으로 회전 하 여 [`Image`](xref:Xamarin.Forms.Image) 인스턴스에 애니메이션 효과를 적용 합니다. [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 애니메이션의 시작에 대 한 현재 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성 값을 가져온 다음 해당 값에서 첫 번째 인수 (360)를 더한 값으로 회전 합니다. 이렇게 하면 각 애니메이션이 항상 시작 위치에서 360도 회전 하 게 됩니다. 따라서 애니메이션이 이미 진행 중인 동안 새 애니메이션을 호출 하는 경우 현재 위치에서 시작 하 고 360도의 증분이 아닌 위치에서 끝날 수 있습니다.

다음 스크린샷에는 각 플랫폼에서 진행 중인 상대적인 회전이 나와 있습니다.

![](simple-images/relrotateto.png "Relative Rotation Animation")

### <a name="scaling"></a>배율 조정

다음 코드 예제에서는 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) 메서드를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image)의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
await image.ScaleTo (2, 2000);
```

이 코드는 2 초 (2000 밀리초) 이상으로 크기를 2 배 확장 하 여 [`Image`](xref:Xamarin.Forms.Image) 인스턴스에 애니메이션 효과를 적용 합니다. [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) 메서드는 애니메이션의 시작에 대 한 현재 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성 값 (기본값 1)을 가져온 다음 해당 값에서 첫 번째 인수 (2)로 크기를 조정 합니다. 이렇게 하면 이미지 크기를 두 배 크기로 확장 하는 효과가 있습니다.

다음 스크린샷에는 각 플랫폼에서 진행 중인 확장이 나와 있습니다.

![](simple-images/scaleto.png "Scaling Animation")

> [!NOTE]
> 또한 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스는 `VisualElement`의 크기를 가로 및 세로 방향으로 다르게 확장할 수 있는 [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) 속성도 정의합니다. 이러한 속성은 [`Animation`](xref:Xamarin.Forms.Animation) 클래스로 애니메이션 효과를 적용할 수 있습니다. 자세한 내용은 [xamarin.ios의 사용자 지정 애니메이션](custom.md)을 참조 하세요.

### <a name="relative-scaling"></a>상대적 크기 조정

다음 코드 예제에서는 [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image)의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
await image.RelScaleTo (2, 2000);
```

이 코드는 2 초 (2000 밀리초) 이상으로 크기를 2 배 확장 하 여 [`Image`](xref:Xamarin.Forms.Image) 인스턴스에 애니메이션 효과를 적용 합니다. [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 애니메이션의 시작에 대 한 현재 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성 값을 가져온 다음 해당 값에서 첫 번째 인수 (2)를 더한 값으로 크기를 조정 합니다. 이렇게 하면 각 애니메이션의 시작 위치에서 항상 2의 크기가 조정 됩니다.

### <a name="scaling-and-rotation-with-anchors"></a>앵커를 사용 하 여 크기 조정 및 회전

[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성은 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 및 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 대 한 크기 조정 또는 회전 중심을 설정 합니다. 따라서 해당 값은 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 및 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) 메서드에도 영향을 줍니다.

레이아웃의 중심에 배치 된 [`Image`](xref:Xamarin.Forms.Image) 경우 다음 코드 예제에서는 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성을 설정 하 여 레이아웃의 중심 주위로 이미지를 회전 하는 방법을 보여 줍니다.

```csharp
double radius = Math.Min(absoluteLayout.Width, absoluteLayout.Height) / 2;
image.AnchorY = radius / image.Height;
await image.RotateTo(360, 2000);
```

레이아웃의 중심을 기준으로 [`Image`](xref:Xamarin.Forms.Image) 인스턴스를 회전 하려면 [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성을 `Image`의 너비 및 높이에 대 한 값으로 설정 해야 합니다. 이 예제에서 `Image`의 중심은 레이아웃의 중심에 있도록 정의 되므로 0.5의 기본 `AnchorX` 값은 변경할 필요가 없습니다. 그러나 `AnchorY` 속성은 `Image` 위쪽에서 레이아웃의 중심점 까지의 값으로 다시 정의 됩니다. 이렇게 하면 다음 스크린샷에 표시 된 것 처럼 `Image`에서 레이아웃 중심점을 기준으로 360도 전체 회전 합니다.

![](simple-images/rotate-anchors.png "Rotation Animation with Anchors")

### <a name="translation"></a>변환

다음 코드 예제에서는 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image)의 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
await image.TranslateTo (-100, -100, 1000);
```

이 코드는 1 초 (1000 밀리초) 이상 가로 및 세로로 변환 하 여 [`Image`](xref:Xamarin.Forms.Image) 인스턴스에 애니메이션 효과를 적용 합니다. [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 이미지 100 픽셀을 왼쪽으로, 100 픽셀을 위쪽으로 변환 합니다. 이는 첫 번째 인수와 두 번째 인수가 모두 음수 이기 때문입니다. 양수를 제공 하면 이미지가 오른쪽으로, 아래쪽으로 변환 됩니다.

다음 스크린샷에는 각 플랫폼에서 진행 중인 번역이 나와 있습니다.

![](simple-images/translateto.png "Translation Animation")

> [!NOTE]
> 요소가 처음에 화면에 배치 된 다음 화면으로 변환 된 경우 변환 후 요소의 입력 레이아웃은 화면을 벗어나 그대로 유지 되 고 사용자는 조작할 수 없습니다. 따라서 뷰를 최종 위치에 배치 하 고 필요한 모든 번역을 수행 하는 것이 좋습니다.

### <a name="fading"></a>페이딩

다음 코드 예제에서는 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 사용 하 여 [`Image`](xref:Xamarin.Forms.Image)의 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 속성에 애니메이션 효과를 주는 방법을 보여 줍니다.

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

이 코드는 4 초 (4000 밀리초) 이상으로 [`Image`](xref:Xamarin.Forms.Image) 인스턴스에 애니메이션 효과를 적용 합니다. [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 애니메이션의 시작에 대 한 현재 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 속성 값을 가져온 다음 해당 값에서 첫 번째 인수 (1)로 페이드 인 합니다.

다음 스크린샷에는 각 플랫폼에서 진행 중인 작업이 표시 됩니다.

![](simple-images/fadeto.png "Fading Animation")

<a name="compound" />

## <a name="compound-animations"></a>복합 애니메이션

복합 애니메이션은 애니메이션의 순차적 조합이 며, 다음 코드 예제에서 보여 주는 것 처럼 `await` 연산자를 사용 하 여 만들 수 있습니다.

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

이 예제에서는 [`Image`](xref:Xamarin.Forms.Image) 6 초 (6000 밀리초) 이상으로 변환 됩니다. `Image`의 변환은 5 개의 애니메이션을 사용 하 고 각 애니메이션이 순차적으로 실행 됨을 나타내는 `await` 연산자를 사용 합니다. 따라서 이전 메서드가 완료 된 후 후속 애니메이션 메서드가 실행 됩니다.

<a name="composite" />

## <a name="composite-animations"></a>복합 애니메이션

복합 애니메이션은 둘 이상의 애니메이션이 동시에 실행 되는 애니메이션의 조합입니다. 다음 코드 예제에서 보여 주는 것 처럼 대기 및 비 대기 애니메이션을 혼합 하 여 복합 애니메이션을 만들 수 있습니다.

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

이 예에서는 [`Image`](xref:Xamarin.Forms.Image) 크기를 조정 하 고 동시에 4 초 (4000 밀리초) 이상 회전 합니다. `Image`의 크기 조정은 회전과 동시에 발생 하는 두 개의 순차적인 애니메이션을 사용 합니다. [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 `await` 연산자 없이 실행 되며, 첫 번째 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) 애니메이션이 시작 된 후 즉시 반환 됩니다. 첫 번째 `ScaleTo` 메서드 호출의 `await` 연산자는 첫 번째 `ScaleTo` 메서드 호출이 완료 될 때까지 두 번째 `ScaleTo` 메서드 호출을 지연 시킵니다. 이 시점에서 `RotateTo` 애니메이션은 절반으로 완료 되 고 `Image`는 180도 회전 됩니다. 마지막 2 초 (2000 밀리초) 동안 두 번째 `ScaleTo` 애니메이션과 `RotateTo` 애니메이션이 모두 완료 됩니다.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>동시에 여러 비동기 메서드 실행

`static` `Task.WhenAny` 및 `Task.WhenAll` 메서드는 여러 비동기 메서드를 동시에 실행 하는 데 사용 되므로 복합 애니메이션을 만드는 데 사용할 수 있습니다. 두 메서드는 모두 `Task` 개체를 반환 하 고 각각 `Task` 개체를 반환 하는 메서드 컬렉션을 허용 합니다. `Task.WhenAny` 메서드는 다음 코드 예제에서 보여 주는 것 처럼 컬렉션의 메서드가 실행을 완료 하면 완료 됩니다.

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

이 예제에서 `Task.WhenAny` 메서드 호출에는 두 개의 태스크가 포함 되어 있습니다. 첫 번째 작업은 4 초 (4000 밀리초) 이상 이미지를 회전 하 고 두 번째 작업은 이미지를 2 초 (2000 밀리초) 이상으로 조정 합니다. 두 번째 작업이 완료 되 면 `Task.WhenAny` 메서드 호출이 완료 됩니다. 그러나 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드가 아직 실행 되 고 있는 경우에도 두 번째 [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo*) 메서드를 시작할 수 있습니다.

`Task.WhenAll` 메서드는 다음 코드 예제에서 보여 주는 것 처럼 컬렉션의 모든 메서드가 완료 되 면 완료 됩니다.

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

이 예제에서 `Task.WhenAll` 메서드 호출에는 각각 10 분 이상 실행 되는 세 개의 태스크가 포함 되어 있습니다. 각 `Task`는 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))에 대 한 251 회전 및 [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))에 대 한 199 회전의 307 360 회전 수를 서로 다르게 만듭니다. 이러한 값은 소수 이므로 회전이 동기화 되지 않으므로 반복 패턴을 초래 하지 않습니다.

다음 스크린샷에는 각 플랫폼에서 진행 중인 여러 회전이 나와 있습니다.

![](simple-images/multiple-rotations.png "Composite Animation")

## <a name="canceling-animations"></a>애니메이션 취소

응용 프로그램은 다음 코드 예제에서 보여 주는 것 처럼 `static` [`ViewExtensions.CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 메서드를 호출 하 여 하나 이상의 애니메이션을 취소할 수 있습니다.

```csharp
ViewExtensions.CancelAnimations (image);
```

그러면 [`Image`](xref:Xamarin.Forms.Image) 인스턴스에서 현재 실행 중인 모든 애니메이션이 즉시 취소 됩니다.

## <a name="summary"></a>요약

이 문서에서는 [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스를 사용 하 여 애니메이션을 만들고 취소 하는 방법을 보여 주었습니다. 이 클래스는 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 인스턴스를 회전, 크기 조정, 변환 및 페이드 인 하는 간단한 애니메이션을 생성 하는 데 사용할 수 있는 확장 메서드를 제공 합니다.

## <a name="related-links"></a>관련 링크

- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [기본 애니메이션 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-basic)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
