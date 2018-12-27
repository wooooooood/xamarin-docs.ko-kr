---
title: Xamarin.Forms에서 간단한 애니메이션 만들기
description: ViewExtensions 클래스는 간단한 애니메이션을 만드는 데 사용할 수 있는 확장 메서드를 제공합니다. 이 문서에서는 ViewExtensions 클래스를 사용하여 애니메이션을 생성하고 취소하는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 93bb138b3a78b6052715ab987a4634a18cdce317
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054504"
---
# <a name="simple-animations-in-xamarinforms"></a>Xamarin.Forms에서 간단한 애니메이션 만들기

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)

_ViewExtensions 클래스는 간단한 애니메이션을 만드는 데 사용할 수 있는 확장 메서드를 제공합니다. 이 문서에서는 ViewExtensions 클래스를 사용하여 애니메이션을 생성하고 취소하는 방법을 보여 줍니다._


[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스는 아래와 같이 간단한 애니메이션을 만드는 데 사용할 수 있는 확장 메서드를 제공합니다.

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)와 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)의 속성에 애니메이션을 적용합니다.
- [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale)는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 애니메이션을 적용합니다.
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 증가분과 감소분에 대한 애니메이션을 적용합니다.
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))는 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)의 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 속성에 애니메이션을 적용합니다.
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))는 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)의 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 속성에 증가분과 감소분에 대한 애니메이션을 적용합니다.
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) 속성에 애니메이션을 적용합니다.
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))는 [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)의 [`VisualElement`](xref:Xamarin.Forms.VisualElement) 속성에 애니메이션을 적용합니다.
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))는 [`VisualElement`](xref:Xamarin.Forms.VisualElement)의 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 속성에 애니메이션을 적용합니다.

기본적으로 각 애니메이션의 동작 시간은 250 밀리초입니다. 그렇지만 애니메이션을 만들 때 각 애니메이션에 시간을 지정할 수 있습니다.

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)는 또한 동작하고 있는 어떤 애니메이션도 취소할 수 있는 [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 메서드를 가지고 있습니다.

> [!NOTE]
> [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스는 [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing))라는 확장 메서드를 제공합니다. 그러나 이 메서드는 크기와 위치를 변경하는 레이아웃의 상태를 전환하는 애니메이션을 구현하는 데 사용합니다. 따라서 [`Layout`](xref:Xamarin.Forms.Layout)의 하위 클래스에서만 사용할 수 있습니다.

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) 클래스에 있는 애니메이션 확장 메서드는 모두 비동기적으로 처리되고 `Task<bool>` 개체를 반환합니다. 반환 값은 애니메이션이 정상적으로 완료되었으면 `false`를, 애니메이션이 취소되었으면 `true`를 반환합니다. 그렇기 때문에 애니메이션은 일반적으로 애니메이션이 끝났는지 쉽게 확인할 수 있도록 `await` 연산자와 함께 사용해야 합니다. 또한 이를 통해 이전 메서드가 종료된 후 다음 애니메이션을 실행시키는 순차적인 애니메이션 생성이 가능합니다. 자세한 내용은 [복합 애니메이션](#compound)을 참조합니다.

애니메이션을 사용 하는 요구 사항이 없는 경우 백그라운드에서 완료 하면 `await` 연산자를 생략할 수 있습니다. 이 시나리오에서는 애니메이션 확장 메서드는 신속 하 게 백그라운드에서 발생 하는 애니메이션을 사용 하 여 애니메이션을 시작한 후 반환 됩니다. 이 작업 복합 애니메이션을 만들 때의 이점은 수행할 수 있습니다. 자세한 내용은 [복합 애니메이션](#composite)을 참조합니다.

`await` 연산자에 대한 더 많은 정보는 [비동기 지원 개요](~/cross-platform/platform/async.md)를 참조합니다.

## <a name="single-animations"></a>단일 애니메이션

[`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions)에 있는 각 확장 메서드는 점진적으로 속성을 하나의 값에서 다른 값으로 변경하는 애니메이션 동작을 구현합니다. 이 섹션에서는 애니메이션을 하나씩 살펴봅니다.

### <a name="rotation"></a>회전

다음 코드 예제는 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 이용해 [`Image`](xref:Xamarin.Forms.Image)의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성에 애니메이션을 적용하는 방법을 살펴봅니다.

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

이 코드는 [`Image`](xref:Xamarin.Forms.Image) 인스턴스가 2초 동안 360도 회전하는 애니메이션을 작동합니다. [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 애니메이션이 시작할 때의 현재 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성 값을 가져온 후 그 값에서 첫 번째 인수(360) 값까지 회전합니다. 애니메이션이 완료되면 해당 이미지의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성은 다시 0이 됩니다. 이는 애니메이션이 종료된 후 `Rotation` 속성이 360에 머물지 않도록 하여 추가로 회전하는 것을 방지합니다.

다음 스크린샷은 각 플랫폼에서의 회전 애니메이션 동작을 보여줍니다.

![](simple-images/rotateto.png "회전 애니메이션")

### <a name="relative-rotation"></a>상대 회전

다음 코드 예제는 [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드가 [`Image`](xref:Xamarin.Forms.Image)의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성을 점진적으로 증가 혹은 감소시키는 것을 보여줍니다.

```csharp
await image.RelRotateTo (360, 2000);
```

이 코드는 [`Image`](xref:Xamarin.Forms.Image) 인스턴스를 2초(2000 밀리초)에 걸쳐 시작 위치에서 360도 회전시킵니다. [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 애니메이션이 시작할 때의 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 속성 값을 가져온 후 그 값에서 그 값에서 첫 번째 인수(360)를 더한 값까지 회전합니다. 각 애니메이션은 항상 시작 지점에서 360를 회전한 상태가 됩니다. 만약 애니메이션이 진행 중인 상태에서 새로운 애니메이션을 시작하는 경우, 현재 위치에서 시작되며 360도가 아닌 다른 증분인 위치에서 끝날 수 있습니다.

다음 스크린샷은 각 플랫폼에서 진행 중인 상대 회전 애니메이션 동작을 보여줍니다.

![](simple-images/relrotateto.png "상대 회전 애니메이션")

### <a name="scaling"></a>크기 조정

다음 코드 예제는 [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) 메서드를 이용해 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)의 [`Image`](xref:Xamarin.Forms.Image) 속성에 애니메이션을 적용하는 방법을 살펴봅니다.

```csharp
await image.ScaleTo (2, 2000);
```

이 코드는 [`Image`](xref:Xamarin.Forms.Image) 인스턴스를 2초(2000 밀리초) 동안 2배 확장하는 애니메이션을 적용합니다. [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) 메서드는 시작할 때의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성 값(기본값인 1)에서 첫 번째 인수 값(2)으로 확장시킵니다. 이는 이미지의 크기를 두 배로 확장하는 효과가 있습니다.

다음 스크린샷은 각 플랫폼에서의 크기 조정 애니메이션 동작을 보여줍니다.

![](simple-images/scaleto.png "애니메이션을 크기 조정")

> [!NOTE]
> [`VisualElement`](xref:Xamarin.Forms.VisualElement) 클래스는 `VisualElement`의 가로와 세로를 다르게 확장할 수 있는 [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY) 속성을 제공합니다. 이러한 속성은 [`Animation`](xref:Xamarin.Forms.Animation) 클래스를 통해 애니메이션 적용될 수 있습니다. 자세한 내용은 [Xamarin.Forms에서의 사용자 지정 애니메이션](custom.md)을 참고합니다.

### <a name="relative-scaling"></a>상대 크기 조정

다음 코드 예제는 [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 이용해 [`Image`](xref:Xamarin.Forms.Image)의 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성에 애니메이션을 적용하는 방법을 살펴봅니다.

```csharp
await image.RelScaleTo (2, 2000);
```

이 코드는 [`Image`](xref:Xamarin.Forms.Image) 인스턴스를 2초(2000 밀리초) 동안 2배 확장하는 애니메이션을 적용합니다. 합니다 [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 현재 가져옵니다 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 를 시작 하 고 애니메이션 된 값과 첫 번째 인수 (2)에 해당 값에서 다음 확장 속성 값입니다. 이렇게 하면 각 애니메이션 항상 되도록 2 시작 위치에서 크기 조정 합니다.

### <a name="scaling-and-rotation-with-anchors"></a>앵커를 사용한 크기 조정 및 회전

[`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)와 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성으로 [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) 및 [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) 속성으로 조정하는 크기 조정과 회전의 중심을 설정할 수 있습니다. 따라서 해당 값은 [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))와 [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) 메서드에도 영향을 줍니다.

아래의 코드 예제에서는 [`Image`](xref:Xamarin.Forms.Image)가 레이아웃의 가운데에 배치되었을 때 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)를 설정하여 레이아웃의 중심을 기준으로 이미지를 회전시키는 것을 보여줍니다

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

[`Image`](xref:Xamarin.Forms.Image)를 레이아웃의 가운데를 중심으로 회전하기 위해서는 [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) 속성 값을 `Image`의 너비와 높이에 상대적으로 설정해야 합니다. 이 예제에서 `Image`의 중심은 레이아웃의 중심으로 설정되어 있기 때문에 0.5가 기본 값인 `AnchorX`는 변경할 필요가 없습니다. 그러나 `AnchorY` 속성은 `Image`의 맨 위에서 중심으로 값을 변경해야 합니다. 이렇게 하면 `Image`는 다음 스크린샷에서와 같이 레이아웃의 중심을 기준으로 360도 회전합니다.

![](simple-images/rotate-anchors.png "앵커를 사용한 크기 조정 및 회전")

### <a name="translation"></a>이동

다음 코드 예제는 [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 사용해 [`Image`](xref:Xamarin.Forms.Image)의 [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) 속성에 애니메이션을 적용하는 방법을 살펴봅니다.

```csharp
await image.TranslateTo (-100, -100, 1000);
```

이 코드는 [`Image`](xref:Xamarin.Forms.Image) 인스턴스를 1초(1000 밀리초) 동안 가로와 세로로 이동시킵니다. [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 이미지를 왼쪽으로 100픽셀, 그리고 위쪽으로 100픽셀 이동시킵니다. 이는 첫 번째와 두 번째 인수 모두 다 음수이기 때문입니다. 값이 양수면 이미지는 오른쪽 및 아래쪽으로 이동합니다.

다음 스크린샷은 각 플랫폼에서의 이동 애니메이션 동작을 보여줍니다.

![](simple-images/translateto.png "이동 애니메이션")

> [!NOTE]
> 만약 요소가 화면 바깥에 있다가 화면 안으로 이동하는 경우, 이동 후 요소의 입력 레이아웃은 스크린 밖에 위치하고 사용자는 상호작용할 수 없습니다. 그렇기 때문에 뷰는 최종 위치에 놓아야 하고 그런 다음에 이동을 진행하는 것이 좋습니다.

### <a name="fading"></a>페이딩

다음 코드 예제는 [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 이용해 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)의 [`Image`](xref:Xamarin.Forms.Image) 속성에 애니메이션을 적용하는 방법을 살펴봅니다.

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

이 코드는 [`Image`](xref:Xamarin.Forms.Image)인스턴스를 4초(4000 밀리초)동안 페이드 인합니다. [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 시작할 때의 [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) 값을 가져와 그 값에서 첫 번째 인수 값인 (1)로 페이드 인합니다.

다음 스크린샷은 각 플랫폼에서의 페이드 인 애니메이션 동작을 보여줍니다.

![](simple-images/fadeto.png "페이딩 애니메이션")

<a name="compound" />

## <a name="compound-animations"></a>복합 애니메이션

복합 애니메이션은 애니메이션의 순차적 조합 및 사용 하 여 만들 수 있습니다는 `await` 연산자를 사용 하면 다음 코드 예제에서 보여 줍니다.

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

이 예제는 [ `Image` ](xref:Xamarin.Forms.Image) 6 초 이상 (6000 밀리초)을 변환 됩니다. 변환 합니다 `Image` 5 애니메이션을 사용 합니다 `await` 각 애니메이션을 순차적으로 실행을 나타내는 연산자입니다. 따라서 후속 애니메이션 메서드는 이전 메서드와 완료 된 후 실행 합니다.

<a name="composite" />

## <a name="composite-animations"></a>복합 애니메이션

복합 애니메이션을 애니메이션을 두 개 이상 동시에 실행할 애니메이션의 조합입니다. 다음 코드 예제에서 설명한 것 처럼 대기 및 비 대기 애니메이션을 혼합 하 여 복합 애니메이션을 만들 수 있습니다.

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

이 예제는 [ `Image` ](xref:Xamarin.Forms.Image) 크기가 조정 되 고 동시에 4 초 이상 (4000 밀리초)을 회전 합니다. 크기 조정은 `Image` 회전으로 동시에 발생 하는 두 개의 연속 애니메이션을 사용 합니다. [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드를 실행 하지 않고는 `await` 연산자 하 고 첫 번째를 사용 하 여 즉시 반환 [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) 애니메이션을 시작 합니다. 합니다 `await` 첫 번째 연산자 `ScaleTo` 메서드 호출에는 두 번째 지연 `ScaleTo` 까지 첫 번째 메서드 호출 `ScaleTo` 메서드 호출이 완료 합니다. 이 시점에서 `RotateTo` 애니메이션은 완료 된 방식으로 절반 정도 및 `Image` 180도 회전된 됩니다. 마지막 2 초 (2000 밀리초) 동안 두 번째 `ScaleTo` 애니메이션 및 `RotateTo` 애니메이션 모두 완료 합니다.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>여러 비동기 메서드를 동시에 실행

합니다 `static` `Task.WhenAny` 고 `Task.WhenAll` 메서드 동시에 여러 비동기 메서드를 실행 하는 데 사용 되 고 하므로 복합 애니메이션을 만드는 데 사용할 수 있습니다. 두 메서드는 반환을 `Task` 개체 및 메서드의 컬렉션을 받아서 각 반환을 `Task` 개체입니다. `Task.WhenAny` 메서드가 완료 되 면 해당 컬렉션의 모든 메서드가 실행을 완료 하는 경우 다음 코드 예제에서 설명한 것 처럼:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

이 예제는 `Task.WhenAny` 메서드 호출에 두 가지 작업이 포함 되어 있습니다. 첫 번째 작업 이미지 4 초 이상 (4000 밀리초)을 회전 사이인 두 번째 작업 이미지 2 초 (2000 밀리초) 이상. 두 번째 작업이 완료 되 면를 `Task.WhenAny` 메서드 호출이 완료 합니다. 그러나도 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 계속 실행 두 번째 [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) 메서드가 시작할 수 있습니다.

`Task.WhenAll` 메서드가 완료 되 면 해당 컬렉션의 모든 메서드를 완료 하는 경우 다음 코드 예제에서 설명한 것 처럼:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

이 예제는 `Task.WhenAll` 메서드 호출에 세 가지 작업을 10 분 넘게 실행 각각 포함 되어 있습니다. 각 `Task` 360도 회전-307 회전에 대 한 다른 여러 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))에 대 한 251 회전 [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), 및 199 회전에 대 한 [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)). 이러한 값은 소수에 따라서는 회전 동기화 되지 않았습니다 하 고 반복적인 패턴이 있으므로 발생 하지 않습니다을 보장 합니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 여러 회전을 보여 줍니다.

![](simple-images/multiple-rotations.png "복합 애니메이션")

## <a name="canceling-animations"></a>애니메이션을 취소합니다.

응용 프로그램에 대 한 호출을 사용 하 여 하나 이상의 애니메이션을 취소할 수는 `static` [ `ViewExtensions.CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 메서드를 다음 코드 예제에서 설명한 것 처럼:

```csharp
ViewExtensions.CancelAnimations (image);
```

현재 실행 중인 모든 애니메이션을 즉시 취소는이 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스.

## <a name="summary"></a>요약

이 문서에서는 설명 만들기 및 사용 하 여 애니메이션을 취소 합니다 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스입니다. 이 클래스는 회전, 크기 조정, 변환 및 페이드 인 되는 간단한 애니메이션을 만드는 데 사용할 수 있는 확장 메서드를 제공 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 인스턴스.


## <a name="related-links"></a>관련 링크

- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [기본 애니메이션 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](xref:Xamarin.Forms.ViewExtensions)
