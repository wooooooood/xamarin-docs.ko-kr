---
title: Xamarin.Forms에서 간단한 애니메이션
description: ViewExtensions 클래스는 간단한 애니메이션을 만드는 데 사용할 수 있는 확장 메서드를 제공 합니다. 이 문서에서는 만들고 ViewExtensions 클래스를 사용 하 여 애니메이션을 취소 하는 방법을 보여 줍니다.
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
# <a name="simple-animations-in-xamarinforms"></a>Xamarin.Forms에서 간단한 애니메이션

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)

_ViewExtensions 클래스는 간단한 애니메이션을 만드는 데 사용할 수 있는 확장 메서드를 제공 합니다. 이 문서에서는 만들고 ViewExtensions 클래스를 사용 하 여 애니메이션을 취소 하는 방법을 보여 줍니다._


합니다 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스는 간단한 애니메이션을 만드는 데 사용할 수 있는 다음 확장 메서드를 제공 합니다.

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 합니다 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.
- [`ScaleTo`](xref:Xamarin.Forms.VisualElement.Scale) 애니메이션을 적용 합니다 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션된 증분 증가 또는 감소를 적용 합니다 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.
- [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 합니다 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.
- [`RelRotateTo`](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션된 증분 증가 또는 감소를 적용 합니다 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.
- [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 합니다 [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.
- [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 합니다 [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.
- [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 합니다 [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) 의 속성을 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)합니다.

기본적으로 각 애니메이션 250 밀리초가 소요 됩니다. 그러나 애니메이션을 만들 때 각 애니메이션에 대 한 기간을 지정할 수 있습니다.

합니다 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스도 포함 되어 있습니다를 [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) 모든 애니메이션을 취소 하는 메서드.

> [!NOTE]
> [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스는 제공 된 [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)) 확장 메서드. 그러나이 메서드는 레이아웃 크기를 포함 하 고 위치를 변경 하는 레이아웃 상태 간의 전환에 애니메이션 효과를 사용할 것입니다. 따라서에서 해당만 사용 해야 [ `Layout` ](xref:Xamarin.Forms.Layout) 하위 클래스입니다.

애니메이션 확장 메서드는 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 클래스는 모든 비동기 및 반환을 `Task<bool>` 개체입니다. 반환 값은 `false` 애니메이션이 완료 되 면 및 `true` 애니메이션을 취소 합니다. 사용 하 여 애니메이션 메서드를 일반적으로 사용 해야 하므로 `await` 애니메이션이 완료 된 시기를 쉽게 확인할 수 있도록 하는 연산자가 있습니다. 또한 다음 되기 이전 메서드가 완료 된 후 실행 하는 후속 애니메이션 메서드를 사용 하 여 순차 애니메이션을 만들 수 있습니다. 자세한 내용은 [복합 애니메이션](#compound)합니다.

애니메이션을 사용 하는 요구 사항이 없는 경우 백그라운드에서 완료 하면 `await` 연산자를 생략할 수 있습니다. 이 시나리오에서는 애니메이션 확장 메서드는 신속 하 게 백그라운드에서 발생 하는 애니메이션을 사용 하 여 애니메이션을 시작한 후 반환 됩니다. 이 작업 복합 애니메이션을 만들 때의 이점은 수행할 수 있습니다. 자세한 내용은 [복합 애니메이션](#composite)합니다.

에 대 한 자세한 내용은 합니다 `await` 연산자를 참조 하세요 [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

## <a name="single-animations"></a>단일 애니메이션

각 확장 메서드는 [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) 변경 하는 점진적으로 속성 값이 하나에서 다른 값으로 시간 동안의 단일 애니메이션 작업을 구현 합니다. 이 섹션에서는 각 애니메이션 작업을 알아봅니다.

### <a name="rotation"></a>회전

다음 코드 예제를 사용 하 여 보여 줍니다.는 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 하는 방법을 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 속성을 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

이 코드에 애니메이션을 적용 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 2 초 (2000 밀리초) 이상 최대 360도 회전 하 여 인스턴스. 합니다 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 현재 가져옵니다 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 속성 애니메이션의 시작 값 및 다음 첫 번째 인수 (360)를 해당 값에서 회전 합니다. 애니메이션 되 면 완료를 이미지의 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 속성이 0으로 다시 설정 됩니다. 이렇게 하면는 `Rotation` 속성 추가 회전 방지 하는 애니메이션 종료 후 360 유지 하지 않습니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 회전을 보여 줍니다.

![](simple-images/rotateto.png "회전 애니메이션")

### <a name="relative-rotation"></a>상대 회전

다음 코드 예제를 사용 하 여 보여 줍니다.는 [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 점진적으로 증가 또는 감소 하려면 메서드를 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 속성을 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelRotateTo (360, 2000);
```

이 코드에 애니메이션을 적용 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 2 초 (2000 밀리초) 이상 시작 위치부터 360도 회전 하 여 인스턴스. 합니다 [ `RelRotateTo` ](xref:Xamarin.Forms.ViewExtensions.RelRotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 현재 가져옵니다 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 속성 애니메이션의 시작 값 및 다음 첫 번째 인수 (360) 값에 해당 값에서 회전 합니다. 이렇게 하면 각 애니메이션의 시작 위치에서 360도 회전을 항상 되도록 합니다. 따라서 새 애니메이션을 애니메이션 이미 진행에서 하는 동안 호출 되는 경우 현재 위치에서 시작 되 고 360도 증가 하지 않은 위치에서 종료 될 수 있습니다.

다음 스크린샷에서 각 플랫폼에서 진행에서 상대 회전을 보여 줍니다.

![](simple-images/relrotateto.png "상대 회전 애니메이션")

### <a name="scaling"></a>배율 조정

다음 코드 예제를 사용 하 여 보여 줍니다.는 [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) 애니메이션을 적용 하는 방법을 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 속성을 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.ScaleTo (2, 2000);
```

이 코드에 애니메이션을 적용 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스 크기의 두 배를 2 초 (2000 밀리초) 이상 확장 하 여 합니다. 합니다 [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) 메서드를 현재 가져옵니다 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 애니메이션 및 해당 첫 번째 인수 (2)에 해당 값에서 다음 눈금의 시작에 대 한 속성 값 (기본값은 1). 이 이미지의 크기를 두 배로 확장 하는 효과가 있습니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 크기 조정을 보여 줍니다.

![](simple-images/scaleto.png "애니메이션을 크기 조정")

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) 클래스도 정의 [ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX) 및 [ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY) 확장 될 수 있는 속성을 `VisualElement` 에서 다르게는 가로 및 세로 방향입니다. 이러한 속성에 애니메이션을 사용 하 여 적용할 수는 [ `Animation` ](xref:Xamarin.Forms.Animation) 클래스입니다. 자세한 내용은 [Xamarin.Forms에서 사용자 지정 애니메이션](custom.md)합니다.

### <a name="relative-scaling"></a>상대 크기 조정

다음 코드 예제를 사용 하 여 보여 줍니다.는 [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 하는 방법을 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 속성을 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
await image.RelScaleTo (2, 2000);
```

이 코드에 애니메이션을 적용 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스 크기의 두 배를 2 초 (2000 밀리초) 이상 확장 하 여 합니다. 합니다 [ `RelScaleTo` ](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 현재 가져옵니다 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 를 시작 하 고 애니메이션 된 값과 첫 번째 인수 (2)에 해당 값에서 다음 확장 속성 값입니다. 이렇게 하면 각 애니메이션 항상 되도록 2 시작 위치에서 크기 조정 합니다.

### <a name="scaling-and-rotation-with-anchors"></a>크기 조정 및 회전 앵커를 사용 하 여

[ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) 하 고 [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) 속성이 설정 크기 조정 또는 회전의 중심을 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) 및 [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) 속성입니다. 따라서 해당 값도 영향을 줄을 [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 하 고 [ `ScaleTo` ](xref:Xamarin.Forms.VisualElement.Scale) 메서드.

지정 된 [ `Image` ](xref:Xamarin.Forms.Image) 레이아웃의 가운데에 배치 된는, 다음 코드 예제에서는 설정 하 여 레이아웃의 가운데를 기준으로 이미지를 회전 하는 방법을 보여 줍니다 해당 [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) 속성:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

회전 하는 [ `Image` ](xref:Xamarin.Forms.Image) 중심 레이아웃, 인스턴스의 합니다 [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) 및 [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) 속성 값을 설정 해야 합니다 너비와 높이 기준으로 `Image`합니다. 이 예의 중심에는 `Image` 레이아웃의 중심이 되도록 정의 되어 등 기본 `AnchorX` 값이 0.5 변경할 필요가 없습니다. 그러나 합니다 `AnchorY` 맨 위에서 값으로 재정의 된 속성을 `Image` 레이아웃의 중심점에 합니다. 이렇게 하면는 `Image` 다음 스크린샷과에서 같이 전체 레이아웃의 중심점 360도 회전 하면:

![](simple-images/rotate-anchors.png "앵커를 사용 하 여 회전 애니메이션")

### <a name="translation"></a>변환

다음 코드 예제에서는 합니다 [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 하는 방법의 [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) 및 [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) 속성을 [ `Image`](xref:Xamarin.Forms.Image):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

이 코드에 애니메이션을 적용 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 인스턴스 변환 하 여이 가로 및 세로로 1 초 (1000 밀리초)입니다. 합니다 [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 동시에 왼쪽에 이미지가 100 픽셀이 고 100 픽셀 위쪽으로 변환 합니다. 첫 번째와 두 번째 인수는 둘 다 음수 때문입니다. 양수를 제공 하는 오른쪽 및 아래쪽 이미지를 변환 합니다.

다음 스크린샷에서 각 플랫폼에서 진행에서 번역을 보여 줍니다.

![](simple-images/translateto.png "변환 애니메이션")

> [!NOTE]
> 요소는 화면 밖으로 처음에 배치 되어 화면 번역 한 다음, 변환 후 요소의 입력된 레이아웃 화면 밖으로 유지 하 고 사용자와 상호 작용할 수 없습니다. 따라서 보기의 최종 위치에 배치 해야 하 고 수행 되는 변환 후 필요한 것이 좋습니다.

### <a name="fading"></a>페이딩

다음 코드 예제를 사용 하 여 보여 줍니다.는 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 애니메이션을 적용 하는 방법을 [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) 속성을 [ `Image` ](xref:Xamarin.Forms.Image):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

이 코드에 애니메이션을 적용 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 4 초 (4000 밀리초) 이상에 페이딩 인스턴스. 합니다 [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) 메서드는 현재 가져옵니다 [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) 해당 값에 해당 첫 번째 인수 (1)로 다음 페이드 애니메이션을 시작 하 여 속성 값입니다.

다음 스크린샷은 각 플랫폼에서 진행에서을 페이드를 보여 줍니다.

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
