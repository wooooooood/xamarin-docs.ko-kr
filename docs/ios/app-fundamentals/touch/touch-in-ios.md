---
title: Xamarin.ios의 터치 이벤트 및 제스처
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 터치 이벤트, 다중 터치, 제스처, 다중 제스처 및 사용자 지정 제스처를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 70c46282c9eebfed45bbdae75fdb2216e7f4c889
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526605"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>Xamarin.ios의 터치 이벤트 및 제스처

IOS 응용 프로그램에서 터치 이벤트 및 터치 Api를 이해 하는 것은 장치에 대 한 모든 물리적 상호 작용의 핵심 요소입니다. 모든 터치 상호 작용에 `UITouch` 는 개체가 포함 됩니다. 이 문서에서는 `UITouch` 클래스 및 해당 api를 사용 하 여 터치를 지 원하는 방법에 대해 설명 합니다. 나중에이 정보를 확장 하 여 제스처를 지 원하는 방법을 알아보세요.

## <a name="enabling-touch"></a>터치 사용

`UIKit` UIControl에서 서브클래싱된 컨트롤은 uikit에 기본 제공 되는 제스처를 사용 하는 사용자 상호 작용에 따라 달라 지 며, 따라서 터치를 사용 하도록 설정할 필요가 없습니다. 이미 사용 하도록 설정 되어 있습니다.

그러나의 `UIKit` 많은 보기에서는 기본적으로 터치가 사용 하도록 설정 되어 있지 않습니다. 컨트롤에서 터치를 사용 하도록 설정 하는 방법에는 두 가지가 있습니다. 첫 번째 방법은 다음 스크린샷에 표시 된 것 처럼 iOS 디자이너의 속성 패드에서 사용자 상호 작용 사용 확인란을 선택 하는 것입니다.

 [![](touch-in-ios-images/image1.png "IOS 디자이너의 속성 패드에서 사용자 상호 작용 사용 확인란을 선택 합니다.")](touch-in-ios-images/image1.png#lightbox)

또한 `UserInteractionEnabled` 클래스`UIView` 에서 컨트롤러를 사용 하 여 속성을 true로 설정할 수 있습니다. 이는 UI가 코드에서 생성 되는 경우에 필요 합니다.

다음 코드 줄은 예제입니다.

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>터치 이벤트

사용자가 화면에 접촉 하거나 손가락을 이동 하거나 손가락을 제거 하는 경우 발생 하는 세 가지 터치 단계가 있습니다. 이러한 메서드는 uiview `UIResponder`의 기본 클래스인에 정의 되어 있습니다. iOS는 `UIView` `UIViewController` 및에서 연결 된 메서드를 재정의 하 여 터치를 처리 합니다.

- `TouchesBegan`– 화면이 처음으로 작업 될 때 호출 됩니다.
- `TouchesMoved`– 사용자가 화면 주위에서 손가락을 이동할 때 터치 위치가 변경 될 때 호출 됩니다.
- `TouchesEnded`또는 `TouchesCancelled` –`TouchesEnded` 사용자의 손가락이 화면에서 리프트 될 때 호출 됩니다.  `TouchesCancelled`iOS가 터치를 취소 하는 경우 호출 됩니다. 예를 들어 사용자가 단추에서 손가락을 이동 하 여 누르기를 취소 하는 경우를 예로 들 수 있습니다.


터치 이벤트는 UIViews의 스택에서 재귀적으로 이동 하 여 터치 이벤트가 뷰 개체의 범위 내에 있는지 확인 합니다. 이를 종종 _적중 테스트_라고 합니다. 이러한 항목은 `UIView` 먼저 맨 위에 있는 또는 `UIViewController` 에서 호출 된 `UIView` 다음 뷰 계층 구조에 있는 `UIViewControllers` 및에서 호출 됩니다.

사용자가 화면에 닿을 때마다 개체가생성됩니다.`UITouch` 개체 `UITouch` 에는 터치가 발생 한 경우, 터치가 살짝 밀기 인 경우 등 터치에 대 한 데이터가 포함 됩니다. 터치 이벤트는 하나 이상의 터치를 `NSSet` 포함 하는 터치 속성을 전달 받습니다. 이 속성을 사용 하 여 터치에 대 한 참조를 가져오고 응용 프로그램의 응답을 확인할 수 있습니다.

터치 이벤트 중 하나를 재정의 하는 클래스는 먼저 기본 구현을 호출한 다음 이벤트와 연결 `UITouch` 된 개체를 가져와야 합니다. 첫 번째 터치에 대 한 참조를 가져오려면 `AnyObject` 속성을 호출 하 고 다음 예제에 표시 된 `UITouch` 대로로 캐스팅 합니다.

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS는 화면에서 연속 된 빠른 접촉을 자동으로 인식 하 고 단일 `UITouch` 개체에서 한 번의 탭으로 모두 수집 합니다. 그러면 다음 코드에 나와 있는 것 처럼 `TapCount` 속성을 확인 하는 것 처럼 두 번의 탭을 쉽게 확인할 수 있습니다.

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>멀티 터치

다중 터치는 컨트롤에 대해 기본적으로 사용 되지 않습니다. 다음 스크린샷에 나와 있는 것 처럼 iOS 디자이너에서 멀티 터치를 사용 하도록 설정할 수 있습니다.

 [![](touch-in-ios-images/image2.png "IOS 디자이너에서 멀티 터치 사용")](touch-in-ios-images/image2.png#lightbox)

다음 코드 줄에 표시 된 대로 속성을 `MultipleTouchEnabled` 설정 하 여 프로그래밍 방식으로 다중 터치를 설정할 수도 있습니다.

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

화면에 `Count` `UITouch` 대 한 손가락의 수를 확인 하려면 속성의 속성을 사용 합니다.

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>터치 위치 결정

메서드 `UITouch.LocationInView` 는 지정 된 뷰 내에서 터치의 좌표를 포함 하는 CGPoint 개체를 반환 합니다. 또한 메서드 `Frame.Contains`를 호출 하 여 해당 위치가 컨트롤 내에 있는지 여부를 테스트할 수 있습니다. 다음 코드 조각에서는이에 대 한 예를 보여 줍니다.

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

이제 iOS에서 touch 이벤트를 이해 했으므로 제스처 인식기에 대해 알아보겠습니다.

## <a name="gesture-recognizers"></a>제스처 인식기

제스처 인식기는 응용 프로그램의 터치를 지 원하는 프로그래밍 활동을 크게 단순화 하 고 줄일 수 있습니다. iOS 제스처 인식기는 일련의 터치 이벤트를 단일 터치 이벤트로 집계 합니다.

Xamarin.ios는 다음과 같은 기본 제공 `UIGestureRecognizer` 제스처 인식기에 대 한 기본 클래스로 클래스를 제공 합니다.

- *UITapGestureRecognizer* – 하나 이상의 탭에 대 한 것입니다.
- *UIPinchGestureRecognizer* – 손가락을 집기 하 고 분산 합니다.
- *UIPanGestureRecognizer* – 패닝 또는 끌기.
- *UISwipeGestureRecognizer* – 모든 방향으로 살짝 밀기.
- *UIRotationGestureRecognizer* – 시계 방향으로 두 손가락을 회전 하거나 시계 반대 방향으로 동작 합니다.
- *UILongPressGestureRecognizer* – 길게 누르기 또는 긴 클릭이 라고도 하는 누르고 있습니다.


제스처 인식기를 사용 하는 기본 패턴은 다음과 같습니다.

1. **제스처 인식기 인스턴스화** – 먼저 `UIGestureRecognizer` 서브 클래스를 인스턴스화합니다. 인스턴스화된 개체는 뷰에 연결 되며 뷰가 삭제 되 면 가비지 수집 됩니다. 이 뷰를 클래스 수준 변수로 만들 필요는 없습니다.
1. **제스처 설정 구성** – 다음 단계는 제스처 인식기를 구성 하는 것입니다. 인스턴스의`UIGestureRecognizer` 동작을 제어 하기 `UIGestureRecognizer` 위해 설정할 수 있는 속성 목록은 및 해당 서브 클래스에 대 한 Xamarin 설명서를 참조 하세요.
1. 목표 **구성** -C heritage 때문에 xamarin.ios는 제스처 인식기가 제스처와 일치할 때 이벤트를 발생 시 키 지 않습니다.  `UIGestureRecognizer`에는 메서드 ( `AddTarget` )가 있습니다 .이 메서드는 코드를 사용 하 여 무명 대리자 또는 목표-C 선택기를 받아들일 수 있으며 제스처 인식기가 일치 하는 경우에 실행할 수 있습니다.
1. **제스처 인식기 사용** – 터치 이벤트와 마찬가지로 터치 상호 작용을 사용 하는 경우에만 제스처가 인식 됩니다.
1. **뷰에 제스처 인식기 추가** – 최종 단계는를 호출 `View.AddGestureRecognizer` 하 고 제스처 인식기 개체를 전달 하 여 보기에 제스처를 추가 하는 것입니다.

코드에서 구현 하는 방법에 대 한 자세한 내용은 [제스처 인식기 샘플](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) 을 참조 하세요.

제스처의 대상이 호출 되 면 발생 하는 제스처에 대 한 참조가 전달 됩니다. 이렇게 하면 제스처 대상이 발생 한 제스처에 대 한 정보를 가져올 수 있습니다. 사용 가능한 정보의 범위는 사용 된 제스처 인식기의 유형에 따라 달라 집니다. 각 `UIGestureRecognizer` 하위 클래스에 사용할 수 있는 데이터에 대 한 자세한 내용은 Xamarin 설명서를 참조 하세요.

보기에 제스처 인식기를 추가 하 고 나면 뷰 (및 그 아래 뷰)가 터치 이벤트를 수신 하지 않는다는 점을 기억해 야 합니다. 터치 이벤트를 제스처 `CancelsTouchesInView` 와 동시에 허용 하려면 다음 코드에 나와 있는 것 처럼 속성을 false로 설정 해야 합니다.

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

각 `UIGestureRecognizer` 에는 제스처 인식기의 상태에 대 한 중요 한 정보를 제공 하는 State 속성이 있습니다. 이 속성 값이 변경 될 때마다 iOS는 구독 메서드를 호출 하 여 업데이트를 제공 합니다. 사용자 지정 제스처 인식기가 상태 속성을 업데이트 하지 않는 경우에는 구독자가 호출 되지 않으며 제스처 인식기 렌더링 기능이 쓸모 없게 됩니다.

제스처는 다음 두 가지 유형 중 하나로 요약할 수 있습니다.

1. *불연속* – 이러한 제스처는 처음 인식 될 때만 발생 합니다.
1. *연속* – 이러한 제스처는 인식 되는 동안 계속 해 서 발생 합니다.


제스처 인식기는 다음 상태 중 하나에 있습니다.

- *가능* -모든 제스처 인식기의 초기 상태입니다. 상태 속성의 기본값입니다.
- *시작* 됨 – 연속 제스처가 먼저 인식 되 면 상태가 시작 됨으로 설정 됩니다. 이렇게 하면 구독에서 제스처 인식이 시작 될 때와 변경 될 때를 구분할 수 있습니다.
- *변경 됨* – 연속 제스처를 시작 했지만 완료 하지 않은 후에는 터치가 이동 하거나 변경 될 때마다 상태가 변경 됨으로 설정 되 고, 계속 해 서 제스처의 예상 매개 변수 내에 있습니다.
- *취소 됨* – 인식기가 시작부터 변경 된 후에는이 상태가 설정 되며,이는 더 이상 제스처 패턴에 맞지 않는 방식으로 변경 됩니다.
- *인식* 됨 – 제스처 인식기가 접촉 집합과 일치 하는 경우 상태가 설정 되며, 제스처가 완료 되었음을 구독자에 게 알립니다.
- *종료* 됨 – 인식 된 상태에 대 한 별칭입니다.
- *실패* -제스처 인식기가 수신 하는 터치와 더 이상 일치 하지 않을 경우 상태가 Failed로 변경 됩니다.


Xamarin.ios는 `UIGestureRecognizerState` 열거형의 이러한 값을 나타냅니다.

## <a name="working-with-multiple-gestures"></a>여러 제스처 작업

기본적으로 iOS에서는 기본 제스처를 동시에 실행할 수 없습니다. 대신 각 제스처 인식기는 명확 하지 않은 순서로 터치 이벤트를 수신 합니다. 다음 코드 조각에서는 제스처 인식기를 동시에 실행 하는 방법을 보여 줍니다.

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

IOS에서 제스처를 사용 하지 않도록 설정할 수도 있습니다. 제스처 인식기가 응용 프로그램 및 현재 터치 이벤트의 상태를 검사 하 여 제스처를 인식 하는 방법과 시기에 대 한 결정을 내릴 수 있는 두 가지 대리자 속성이 있습니다. 두 이벤트는 다음과 같습니다.

1. *ShouldReceiveTouch* –이 대리자는 제스처 인식기가 터치 이벤트를 전달 하기 바로 전에 호출 되며 터치를 검사 하 고 제스처 인식기에서 처리할 터치를 결정할 수 있는 기회를 제공 합니다.
1. *Shouldbegin* – 인식기가 상태를 가능한 다른 상태로 변경 하려고 할 때 호출 됩니다. False를 반환 하면 제스처 인식기의 상태가 실패로 변경 됩니다.


다음 코드 조각에 나와 있는 것 처럼 강력한 `UIGestureRecognizerDelegate`형식의 약한 대리자 또는 이벤트 처리기 구문을 통해 바인딩할 수 있습니다.

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

마지막으로, 다른 제스처 인식기가 실패할 경우에만 성공 하도록 제스처 인식기를 큐에 대기 시킬 수 있습니다. 예를 들어 단일 탭 제스처 인식기는 이중 탭 제스처 인식기가 실패할 경우에만 성공 해야 합니다. 다음 코드 조각에서는이에 대 한 예제를 제공 합니다.

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>사용자 지정 제스처 만들기

IOS에서는 몇 가지 기본 제스처 인식자를 제공 하지만 특정 경우에 사용자 지정 제스처 인식자를 만들어야 할 수도 있습니다. 사용자 지정 제스처 인식기를 만들려면 다음 단계를 수행 해야 합니다.

1. 서브 `UIGestureRecognizer` 클래스입니다.
1. 적절 한 터치 이벤트 메서드를 재정의 합니다.
1. 기본 클래스의 State 속성을 통해 인식 상태를 버블링 합니다.


이에 대 한 실질적인 예는 [iOS에서 Touch 사용](ios-touch-walkthrough.md) 연습에서 설명 합니다.
