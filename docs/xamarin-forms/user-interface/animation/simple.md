---
title: 간단한 애니메이션
description: ViewExtensions 클래스 간단한 애니메이션을 생성 하는 데 사용할 수 있는 확장 메서드를 제공 합니다. 이 문서 만들고 ViewExtensions 클래스를 사용 하 여 애니메이션을 취소 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 4A6FAE5A-848F-4CE0-BFA1-22A6309B5225
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/27/2017
ms.openlocfilehash: 0d2cc30f9bc1ae5602394b8ca2d8e75517a01b54
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="simple-animations"></a>간단한 애니메이션

_ViewExtensions 클래스 간단한 애니메이션을 생성 하는 데 사용할 수 있는 확장 메서드를 제공 합니다. 이 문서 만들고 ViewExtensions 클래스를 사용 하 여 애니메이션을 취소 하는 방법을 보여 줍니다._


[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 간단한 애니메이션을 만드는 데 사용할 수 있는 다음 확장 메서드를 제공 합니다.

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과 적용는 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) 및 [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) 의 속성을 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.
- [`ScaleTo`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 애니메이션 효과 적용 된 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성은 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과 준된 증분 증가 또는 감소를 적용 하는 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성의는 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.
- [`RotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과 적용 된 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성은 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.
- [`RelRotateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과 준된 증분 증가 또는 감소를 적용 하는 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성의는 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.
- [`RotateXTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과 적용 된 [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) 속성은 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.
- [`RotateYTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과 적용 된 [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) 속성은 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.
- [`FadeTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과 적용 된 [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) 속성은 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)합니다.

기본적으로 각 애니메이션 250 밀리초 걸립니다. 그러나 애니메이션을 만들 때 각 애니메이션에 대 한 기간을 지정할 수 있습니다.

[ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스도 포함 되어는 [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) 애니메이션을 취소 하는 데 사용 될 방법입니다.

> [!NOTE]
> [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스를 제공는 [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/) 확장 메서드. 그러나이 메서드를 사용 하 여 레이아웃 크기를 포함 하 고 위치를 변경 하는 레이아웃 상태 간의 전환 애니메이션 효과를 사용 합니다. 따라서만 사용 해야 하 여 [ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) 하위 클래스입니다.

애니메이션 확장 메서드는 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스는 모두 비동기적이 고 반환 된 `Task<bool>` 개체입니다. 반환 값은 `false` 애니메이션이 완료 된 경우 및 `true` 애니메이션을 취소 합니다. 따라서 애니메이션 메서드 일반적으로 것은 `await` 애니메이션이 완료 된 시기를 쉽게 확인할 수 있도록 하는 연산자입니다. 또한 다음 되기 이전 메서드와 완료 된 후 실행 하는 후속 애니메이션 메서드로 연속 애니메이션을 만들 수 있습니다. 자세한 내용은 참조 [복합 애니메이션](#compound)합니다.

애니메이션에 있도록 요구 사항이 있는 경우 백그라운드에서 완료 하면 `await` 연산자를 생략할 수 있습니다. 이 시나리오에서 애니메이션 확장 메서드 신속 하 게 백그라운드에서 발생 하는 애니메이션으로 애니메이션을 초기화 한 후 반환 됩니다. 이 작업 복합 애니메이션을 만들 때의 장점은 가져올 수 있습니다. 자세한 내용은 참조 [복합 애니메이션](#composite)합니다.

에 대 한 자세한 내용은 `await` 연산자 참조 [비동기 지원 개요](~/cross-platform/platform/async.md)합니다.

## <a name="single-animations"></a>단일 애니메이션

각 확장 메서드는 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 변경 하는 점진적으로 속성 값이 두 개에서 다른 값으로 시간 동안 단일 애니메이션 작업을 구현 합니다. 이 섹션에는 각 애니메이션 작업을 탐색합니다.

### <a name="rotation"></a>회전

다음 코드 예제는 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과를 줄 메서드는 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성의는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RotateTo (360, 2000);
image.Rotation = 0;
```

애니메이션 효과 적용이 코드는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 360도 최대 2 초 (2000 밀리초) 이상 회전 하 여 인스턴스. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드가 가져오면 현재 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성 애니메이션의 시작에 대 한 값과 첫 번째 인수 (360)를 해당 값에서 회전 합니다. 애니메이션 되 면 완료, 이미지의 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성이 0으로 다시 설정 됩니다. 이렇게 하면는 `Rotation` 추가 회전 방지 하는 애니메이션 완료 한 후 속성 360에 유지 하지 않습니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 회전을 보여 줍니다.

![](simple-images/rotateto.png "회전 애니메이션")

### <a name="relative-rotation"></a>상대 회전

다음 코드 예제는 [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 증분 늘리거나 줄이려면 메서드는 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성의는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelRotateTo (360, 2000);
```

애니메이션 효과 적용이 코드는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 2 초 (2000 밀리초) 이상 시작 위치부터 360도 회전 하 여 인스턴스. [ `RelRotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelRotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드가 가져오면 현재 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 속성 애니메이션의 시작에 대 한 값과 첫 번째 인수 (360)를 더한 값을 해당 값에서 회전 합니다. 이렇게 하면 각 애니메이션의 시작 위치에서 360도 회전 항상 되도록 합니다. 따라서 새 애니메이션을 애니메이션 이미 진행 중인 동안에 호출 되 면 현재 위치에서 시작 되 고 360도 씩 증가 하지 않은 위치에 종료 될 수 있습니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 상대 회전을 보여 줍니다.

![](simple-images/relrotateto.png "상대 회전 애니메이션")

### <a name="scaling"></a>배율 조정

다음 코드 예제는 [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 애니메이션 효과를 줄 메서드는 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성의는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.ScaleTo (2, 2000);
```

애니메이션 효과 적용이 코드는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스 크기의 두 배를 2 초 (2000 밀리초) 이상 강화 하 여 합니다. [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 메서드가 가져오면 현재 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 애니메이션 및 (2)의 첫 번째 인수에 해당 값에서 다음 눈금의 시작에 대 한 속성 값 (기본값 1). 이미지의 크기를 두 배로 확장 것과 효과가 있습니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 크기 조정 보여 줍니다.

![](simple-images/scaleto.png "애니메이션을 크기 조정")

### <a name="relative-scaling"></a>상대 크기 조정

다음 코드 예제는 [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과를 줄 메서드는 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성의는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.RelScaleTo (2, 2000);
```

애니메이션 효과 적용이 코드는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스 크기의 두 배를 2 초 (2000 밀리초) 이상 강화 하 여 합니다. [ `RelScaleTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드가 가져오면 현재 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 애니메이션 및 해당 값에서 후 눈금에 값을 더한 첫 번째 인수 (2)의 시작에 대 한 속성 값입니다. 이렇게 하면 각 애니메이션 항상 되도록 2 시작 위치에서 크기 조정.

### <a name="scaling-and-rotation-with-anchors"></a>확장 및 앵커를 사용 하 여 회전

[ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) 및 [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) 속성 설정 크기 조정 또는 회전의 중심은 [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) 및 [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 속성입니다. 따라서 해당 값에 영향을 줄의 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 및 [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 메서드.

지정 된 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 레이아웃의 가운데에 배치 된, 다음 코드 예제에서는 설정 하 여 이미지 레이아웃의 중심 주위를 회전 하는 방법을 보여 줍니다 해당 [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) 속성:

```csharp
image.AnchorY = (Math.Min (absoluteLayout.Width, absoluteLayout.Height) / 2) / image.Height;
await image.RotateTo (360, 2000);
```

회전 하는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 스 레이아웃의 중심 인스턴스는 [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) 및 [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) 속성을 값으로 설정 되어야 너비와 높이 기준으로 `Image`합니다. 이 예제에서는 중앙은 `Image` 정의 됩니다. 스 레이아웃의 가운데에 사용 하 고 있어서 기본 `AnchorX` 값이 0.5 변경할 필요가 없습니다. 그러나는 `AnchorY` 속성 값의 위쪽에서 재정의 된는 `Image` 레이아웃의 중심점에 있습니다. 이렇게 하면는 `Image` 다음 스크린샷에 표시 된 것 처럼 360도 스 레이아웃의 중심점의 전체 표시를 만듭니다.

![](simple-images/rotate-anchors.png "앵커와 회전 애니메이션")

### <a name="translation"></a>변환

다음 코드 예제는 [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과를 줄 메서드는 [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) 및 [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) 는 의속성[ `Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
await image.TranslateTo (-100, -100, 1000);
```

애니메이션 효과 적용이 코드는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스 변환 하 여이 가로 세로로 1 초 넘게 (1000 밀리초)입니다. [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드를 왼쪽으로 이미지 100 픽셀이 고 100 픽셀 위쪽으로 동시에 변환 합니다. 첫 번째 및 두 번째 인수는 모두 음수 때문입니다. 양수를 제공 하는 이미지를 오른쪽, 아래쪽으로 변환 합니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 번역을 보여 줍니다.

![](simple-images/translateto.png "번역 애니메이션")

> [!NOTE]
> 요소 화면 밖으로 처음 배치을 화면에 다음 변환 하는 경우에 변환 후 요소의 입력된 레이아웃 화면 밖 남아 있으며 사용자와 상호 작용할 수 없습니다. 따라서 뷰는 최종 위치에 배치 해야 하 고 다음 수행 되는 변환 필요한 좋습니다.

### <a name="fading"></a>페이딩

다음 코드 예제는 [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 애니메이션 효과를 줄 메서드는 [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) 속성의는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/):

```csharp
image.Opacity = 0;
await image.FadeTo (1, 4000);
```

애니메이션 효과 적용이 코드는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 4 초 (4, 000 밀리초) 이상에 페이딩 인스턴스. [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드가 가져오면 현재 [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) 애니메이션 및 해당 값에 해당 첫 번째 인수 (1)에 다음 페이드의 시작에 대 한 속성 값입니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인는 페이드를 보여 줍니다.

![](simple-images/fadeto.png "페이드 애니메이션")

<a name="compound" />

## <a name="compound-animations"></a>복합 애니메이션

복합 애니메이션을 애니메이션의 순차적 조합 하 고 만들 수 있습니다는 `await` 연산자를 사용 하면 다음 코드 예제에서 보여 줍니다.

```csharp
await image.TranslateTo (-100, 0, 1000);    // Move image left
await image.TranslateTo (-100, -100, 1000); // Move image up
await image.TranslateTo (100, 100, 2000);   // Move image diagonally down and right
await image.TranslateTo (0, 100, 1000);     // Move image left
await image.TranslateTo (0, 0, 1000);       // Move image up
```

이 예제는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 6 초 이상 (6000 밀리초)을 변환 됩니다. `Image` 와 5 개의 애니메이션을 사용 하 여의 `await` 각 애니메이션을 순차적으로 실행을 나타내는 연산자. 따라서 다음 애니메이션 메서드는 이전 메서드와 완료 된 후 실행 합니다.

<a name="composite" />

## <a name="composite-animations"></a>복합 애니메이션

복합 애니메이션을 두 개 이상의 애니메이션 동시에 실행할 애니메이션의 조합입니다. 다음 코드 예제에서와 같이 대기 중인 및 비-대기 애니메이션을 혼합 하 여 복합 애니메이션을 만들 수 있습니다.

```csharp
image.RotateTo (360, 4000);
await image.ScaleTo (2, 2000);
await image.ScaleTo (1, 2000);
```

이 예제는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 크기가 조정 되 고 동시에 4 초 이상 (4, 000 밀리초)을 회전 합니다. 크기 조정을 `Image` 회전와 같은 시간에 발생 하는 두 개의 연속 애니메이션을 사용 합니다. [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드가 없이 실행 프로그램 `await` 연산자 첫 번째 즉시 결과 반환 하 고 [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 시작 하 고 다음 애니메이션 합니다. `await` 연산자의 첫 번째 `ScaleTo` 메서드 호출에 두 번째 지연 `ScaleTo` 까지 첫 번째 메서드 호출 `ScaleTo` 메서드 호출이 완료 합니다. 이 시점에서 `RotateTo` 완료 된 방식으로 애니메이션은 및 `Image` 180도 회전된 됩니다. 마지막 2 초 (2000 밀리초) 동안 두 번째 `ScaleTo` 애니메이션 및 `RotateTo` 애니메이션 모두 완료 합니다.

### <a name="running-multiple-asynchronous-methods-concurrently"></a>여러 비동기 메서드를 동시에 실행

`static` `Task.WhenAny` 및 `Task.WhenAll` 메서드 여러 비동기 메서드를 동시에 실행 하는 데 사용 되 고 복합 애니메이션을 만드는 사용할 수 있습니다. 두 메서드 모두 반환는 `Task` 개체 क र च 메서드 컬렉션을 각각 반환는 `Task` 개체입니다. `Task.WhenAny` 메서드가 완료 되는 컬렉션의 모든 메서드가 실행을 완료 하는 경우 다음 코드 예제에서와 같이:

```csharp
await Task.WhenAny<bool>
(
  image.RotateTo (360, 4000),
  image.ScaleTo (2, 2000)
);
await image.ScaleTo (1, 2000);
```

이 예제는 `Task.WhenAny` 메서드 호출에 두 작업이 포함 되어 있습니다. 첫 번째 작업은 이미지 4 초 (4, 000 밀리초) 이상 회전 하 고 두 번째 작업은 이미지의 크기가 조정 2 초 (2000 밀리초) 이상 합니다. 두 번째 작업이 완료 되 면는 `Task.WhenAny` 메서드 호출이 완료 합니다. 그러나 경우에는 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) 메서드가 계속 실행 되 고 두 번째 [ `ScaleTo` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) 메서드가 시작할 수 있는 합니다.

`Task.WhenAll` 메서드가 완료 되는 컬렉션의 모든 메서드를 완료 했을 때 다음 코드 예제에서와 같이:

```csharp
// 10 minute animation
uint duration = 10 * 60 * 1000;

await Task.WhenAll (
  image.RotateTo (307 * 360, duration),
  image.RotateXTo (251 * 360, duration),
  image.RotateYTo (199 * 360, duration)
);
```

이 예제는 `Task.WhenAll` 메서드 호출에는 세 가지 작업을 10 분 동안 실행 되는 각각 포함 되어 있습니다. 각 `Task` 에서는 360도 회전-307 회전에 대 한 다른 많은 [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 251 회전에 대 한 [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), 및에 대 한 199 회전 [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/). 이러한 값은 소수, 따라서으로 지정 되도록는 회전 동기화 되지 않았습니다 반복적인 패턴 따라서 바뀌지는지 않습니다.

다음 스크린샷에서 각 플랫폼에서 진행 중인 여러 회전을 보여 줍니다.

![](simple-images/multiple-rotations.png "복합 애니메이션")

## <a name="canceling-animations"></a>애니메이션을 취소합니다.

응용 프로그램을 호출 하 여 하나 이상의 애니메이션을 취소할 수는 `static` [ `ViewExtensions.CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) 메서드를 다음 코드 예제에서와 같이:

```csharp
ViewExtensions.CancelAnimations (image);
```

현재 실행 중인 모든 애니메이션이 즉시 취소 합니다.이 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 인스턴스.

## <a name="summary"></a>요약

이 문서를 만들고 사용 하 여 애니메이션 취소에 명시 된 [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) 클래스입니다. 이 클래스는 간단한 애니메이션 회전, 크기 조정, 변환 및 페이드을 생성 하는 데 사용할 수 있는 확장 메서드 제공 [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) 인스턴스.


## <a name="related-links"></a>관련 링크

- [비동기 지원 개요](~/cross-platform/platform/async.md)
- [기본 애니메이션 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/basic/)
- [ViewExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/)
