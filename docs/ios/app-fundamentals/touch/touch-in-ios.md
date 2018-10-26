---
title: 터치 이벤트와 Xamarin.iOS에서 제스처
description: 이 문서에는 터치 이벤트, 멀티 터치, 제스처, 여러 제스처 및 Xamarin.iOS 응용 프로그램에서 사용자 지정 제스처를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: f7160c48e1b1ac85f4aa0173c0eb9f42b8fefca2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114771"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>터치 이벤트와 Xamarin.iOS에서 제스처

반드시 터치 이벤트를 이해 하 고 iOS 응용 프로그램의 Api를 터치 장치를 사용 하 여 모든 물리적 상호 작용의 중심으로 합니다. 포함 하는 모든 터치 상호 작용을 `UITouch` 개체입니다. 이 문서에서 사용 하는 방법을 배웁니다는 `UITouch` 클래스 및 해당 Api에 터치를 지원 합니다. 나중에 제스처를 지원 하는 방법에 대 한 지식을 확장 될 예정입니다.

## <a name="enabling-touch"></a>터치를 사용 하도록 설정

컨트롤 `UIKit` – 해당 서브클래싱된 UIControl에서 – 제스처 UIKit에 기본 제공 된 사용자 상호 작용에 따라서 종속 되며 되지 않으므로 해당 터치를 사용 하도록 설정 해야 합니다. 이미 활성화 됩니다.

그러나의 보기 중 많은 `UIKit` 터치 기본적으로 사용할 수 없습니다. 두 가지 방법으로 터치 컨트롤에 사용할 수 있도록 합니다. 첫 번째 방법은 다음과 같습니다. iOS 디자이너의 속성 패드에서 사용자 상호 작용 사용 확인란을 확인 하려면 다음 스크린샷에 표시 된 것 처럼

 [![](touch-in-ios-images/image1.png "IOS 디자이너의 속성 패드에서 사용자 상호 작용 사용을 확인란합니다")](touch-in-ios-images/image1.png#lightbox)

또한 컨트롤러 설정에 사용할 수는 `UserInteractionEnabled` 속성에서 true로는 `UIView` 클래스입니다. 코드에서 UI를 만들 경우 이것이 필요 합니다.

다음 코드 줄은 예:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>터치 이벤트

터치 사용자 화면을 터치가 손가락을 이동 또는 해당 손가락을 제거 하는 경우 발생 하는 세 가지 단계가 있습니다. 에 정의 된 이러한 메서드가 `UIResponder`,이 UIView에 대 한 기본 클래스입니다. iOS에서 연결된 된 메서드를 재정의 합니다는 `UIView` 하며 `UIViewController` 터치를 처리 하려면:

-  `TouchesBegan` –이 화면 처음 연결 하는 경우 호출 됩니다.
-  `TouchesMoved` –이 위치가 터치 변경 될 사용자가 손가락을 화면 상대 (sliding) 하는 경우 호출 됩니다.
-  `TouchesEnded` 또는 `TouchesCancelled` – `TouchesEnded` 화면에 사용자의 손가락 리프팅되며 때 호출 됩니다.  `TouchesCancelled` 사용자가 자신의 손가락을 키를 눌러 취소 단추에서 슬라이드 하는 경우 iOS가 터치 – 예를 들어, 취소를 호출 됩니다.


View 개체의 범위 내 터치 이벤트 인지 확인 하려면 UIViews 스택을 통해 이벤트 여행 재귀적으로 터치 합니다. 이 종종 이라고 _적중 테스트_합니다. 먼저 최상위에서 호출 됩니다 `UIView` 또는 `UIViewController` 다음에서 호출 되는 및를 `UIView` 및 `UIViewControllers` 아래 뷰 계층 구조에서 해당 합니다.

`UITouch` 개체는 사용자가 화면을 터치 할 때마다 생성 됩니다. `UITouch` 터치 발생 했을 때 발생 한 위치, 터치 등 안쪽으로 살짝 밀어 되었으면 같은 터치에 대 한 데이터를 포함 하는 개체입니다. 터치 이벤트는 터치 속성 – 전달 되는 `NSSet` 하나 이상의 터치를 포함 합니다. 터치에 대 한 참조를 얻으려면이 속성을 사용 하 고 응용 프로그램의 응답을 확인할 수 있습니다.

터치 이벤트 중 하나를 재정의 하는 클래스는 기본 구현을 먼저 가져온 후의 `UITouch` 이벤트와 연결 된 개체입니다. 첫 번째 터치에 대 한 참조를 가져오려면 호출 합니다 `AnyObject` 속성으로 캐스팅을 `UITouch` 다음 예와에서 같이:

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

iOS 연속 빠른 화면의 터치 및 단일에서 한 번 탭으로 모두 수집 됩니다 자동으로 인식 `UITouch` 개체입니다. 이렇게 하면 확인 하는 것 만큼 쉽습니다를 두 번 탭에 대 한 검사는 `TapCount` 속성을 다음 코드 에서처럼:

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

멀티 터치 컨트롤에서 기본적으로 사용 되지 않습니다. 다음 스크린샷에 표시 된 것 처럼 iOS 디자이너에서에서 멀티 터치를 활성화할 수 있습니다.

 [![](touch-in-ios-images/image2.png "IOS 디자이너에서에서 사용 하도록 설정 하는 멀티 터치")](touch-in-ios-images/image2.png#lightbox)

멀티 터치를 설정 하 여 프로그래밍 방식으로 설정할 수 이기도 합니다 `MultipleTouchEnabled` 코드의 다음 줄에 표시 된 것 처럼 속성:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

얼마나 많은 손가락 터치 화면을 확인 하려면 사용 합니다 `Count` 속성에는 `UITouch` 속성:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>터치 위치를 결정합니다.

메서드가 `UITouch.LocationInView` 지정 된 뷰 내에서 터치의 좌표를 포함 하는 CGPoint 개체를 반환 합니다. 또한 메서드를 호출 하 여 컨트롤 내에서 해당 위치를 테스트할 수 있습니다 `Frame.Contains`합니다. 다음 코드 조각은이 예를 보여 줍니다.

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

IOS에서 터치 이벤트의 이해를 만들었으므로 이제 해당 제스처 인식기에 대해 알아보겠습니다.

## <a name="gesture-recognizers"></a>제스처 인식기

제스처 인식기 크게 단순화 하 고 응용 프로그램에서 터치를 지원 하기 위해 프로그래밍 노력을 줄일 수 있습니다. iOS 제스처 인식기를 단일 터치 이벤트로는 일련의 터치 이벤트를 집계합니다.

클래스를 제공 하는 Xamarin.iOS `UIGestureRecognizer` 다음 기본 제공 제스처 인식기에 대 한 기본 클래스로:

-  *UITapGestureRecognizer* – 하나 이상의 탭입니다.
-  *UIPinchGestureRecognizer* – Pinching 및 떨어진 손가락을 분산 합니다.
-  *UIPanGestureRecognizer* – 패닝 또는 끌기.
-  *UISwipeGestureRecognizer* – 살짝 점에서 모든 방향입니다.
-  *UIRotationGestureRecognizer* – 시계 방향 또는 시계 반대 방향으로 진행으로 돌려 두 손가락을 회전 합니다.
-  *UILongPressGestureRecognizer* – 키를 누른를 눌러 긴 또는 장기 원클릭 라고도 합니다.


제스처 인식기를 사용 하 여 기본 패턴은 다음과 같습니다.

1.  **제스처 인식기를 인스턴스화하고** – 먼저 인스턴스화하는 `UIGestureRecognizer` 하위 클래스입니다. 인스턴스화된 개체를 보기에 의해 연결 될 하 고 보기의 삭제 될 때 수집 가비지 됩니다. 클래스 수준 변수를이 뷰를 만드는 데 필요한 것입니다.
1.  **모든 제스처 설정을 구성** – 제스처 인식기를 구성 하는 다음 단계입니다. Xamarin의 설명서를 참조 하세요 `UIGestureRecognizer` 와의 동작을 제어를 설정할 수 있는 속성의 목록을 하위 클래스를 `UIGestureRecognizer` 인스턴스.
1.  **대상 구성** – 해당 Objective-c 특성으로 인해 Xamarin.iOS 제스처 인식기가 제스처와 일치 하는 경우 이벤트를 발생 하지 않습니다.  `UIGestureRecognizer` 메서드가 – `AddTarget` – 익명 대리자 또는 코드를 사용 하 여 Objective-c 선택기를 제스처 인식기에서 일치 하는 경우 실행을 허용할 수입니다.
1.  **제스처 인식기를 사용 하도록 설정** 터치 이벤트를 사용 하 여 제스처만 인식 되는 터치 상호 작용을 사용 하는 경우와 마찬가지로 – 합니다.
1.  **제스처 인식기 뷰에 추가** – 제스처를 호출 하 여 보기에 추가 하는 최종 단계입니다 `View.AddGestureRecognizer` , 제스처 인식기 개체를 전달 합니다.

참조 된 [제스처 인식기 샘플](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) 코드에서 구현 하는 방법에 대 한 자세한 내용은 합니다.

제스처의 대상 호출 될 때 발생 하는 제스처에 대 한 참조가 전달 됩니다. 이 제스처 대상을 제스처 발생에 대 한 정보를 얻을 수 있습니다. 사용할 수 있는 정보의 범위 사용 된 제스처 인식기의 유형에 따라 달라 집니다. 각각에 대해 사용 가능한 데이터에 대 한 자세한 내용은 Xamarin 설명서를 참조 하십시오 `UIGestureRecognizer` 하위 클래스입니다.

제스처 인식기가 뷰에 추가 되 면 뷰 (및 그 아래의 모든 뷰)는 이벤트를 수신 하지 터치를 기억 하는 것이 반드시 합니다. 제스처와 동시에 터치 이벤트를 허용 하는 `CancelsTouchesInView` 다음 코드에 나온 것 처럼 속성을 false로 설정 해야 합니다.

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

각 `UIGestureRecognizer` 제스처 인식기의 상태에 대 한 중요 한 정보를 제공 하는 상태 속성이 있습니다. 이 속성의 값이 변경 될 때마다 iOS 업데이트를 제공 하는 구독 메서드를 호출 합니다. 사용자 지정 제스처 인식기 상태 속성을 업데이트 하지, 하는 경우 구독자가 호출 되지 제스처 인식기를 쓸모 없게 렌더링 합니다.

제스처는 다음과 같은 두 가지 유형 중 하나로 요약할 수 있습니다.

1.  *불연속* -인식 되는 이러한 제스처만 첫 번째 실행 시간입니다.
1.  *연속* – 이러한 제스처 인식 되는으로 실행을 계속 합니다.


제스처 인식기 상태는 다음 중 하나에 있습니다.

-  *가능한* – 모든 제스처 인식기의 초기 상태입니다. 이것이 기본값 State 속성입니다.
-  *시작* – 연속 제스처는 먼저 인식 하는 경우 상태는 시작으로 설정 됩니다. 이렇게 하면 제스처 인식 시작 될 때 변경 되 면 사이의 구분을 구독할 수 있습니다.
-  *Changed* – 연속 제스처 조치가 시작 되었지만 완료 되지 않았습니다, 후 상태에 설정할 Changed 터치 이동 되거나 변경 될 때마다 제스처의 예상된 매개 변수 내에서 여전히 것으로 합니다.
-  *취소* – 인식기 전환 시작에서 변경 및 다음 이러한 방식으로 변경에 더 이상 터치 제스처의 패턴에 맞지 하는 경우이 상태로 설정 됩니다.
-  *인식* – 제스처 인식기 터치 집합과 일치 하 고 알려줍니다 구독자 제스처가 완료 되었음을 나타내는 상태 설정 됩니다.
-  *종료* -인식 상태에 대 한 별칭입니다.
-  *실패 한* – 제스처 인식기는 더 이상 상태는 실패로 변경에 대 한 수신 하 고 터치를 일치 시킬 수 없습니다.


Xamarin.iOS에서 이러한 값을 나타내는 `UIGestureRecognizerState` 열거형입니다.

## <a name="working-with-multiple-gestures"></a>여러 제스처 작업

기본적으로 iOS에서는 기본 제스처 동시에 실행할 수 없습니다. 대신, 각 제스처 인식기 비 결정적인 순서에서 터치 이벤트를 받게 됩니다. 다음 코드 조각은 동시에 실행 하는 제스처 인식기를 확인 하는 방법을 보여 줍니다.

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

IOS에서 제스처를 사용 하지 않도록 설정할 이기도 합니다. 두 가지 대리자 속성을 제스처 인식기는 방법에 대 한 의사 결정 및 제스처를 인식 해야 하는 경우에 현재 터치 이벤트와 응용 프로그램의 상태를 검사할 수 있도록 합니다. 두 이벤트 다음과 같습니다.

1.  *ShouldReceiveTouch* – 터치 이벤트를 전달 하 고 터치를 검토 하 고는 터치 제스처 인식기에서 처리 되는 결정 기회를 제공 하는 제스처 인식기 직전이 대리자가 호출 됩니다.
1.  *ShouldBegin* –이 인식기를 다른 상태에서 가능한 상태를 변경 하려고 하는 때 호출 됩니다. False를 반환 실패 하도록 변경 제스처 인식기의 상태를 강제로 됩니다.


강력한 형식의 이러한 메서드를 재정의할 수 있습니다 `UIGestureRecognizerDelegate`, 약한 대리자 또는 다음 코드 조각에 표시 된 것과 같이 이벤트 처리기 구문을 통해 바인딩:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

마지막으로 제스처 인식기 실패 하면 다른 제스처 인식기만 성공할 수 있도록 큐에 대기 하는 것이 같습니다. 예를 들어, 단일 탭 제스처 인식기를 두 번 탭 제스처 인식기를 실패 한 경우에 성공 해야 합니다. 다음 코드 조각은이 예제를 제공 합니다.

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>사용자 지정 제스처 만들기

IOS는 몇 가지 기본 제스처 인식기를 제공 하지만 특정 한 경우에 사용자 지정 제스처 인식기를 만들어야 할 수 있습니다. 사용자 지정 제스처 인식기를 만드는 단계는 다음과 같습니다.

1.  서브 클래스 `UIGestureRecognizer` 합니다.
1.  적절 한 터치 이벤트 메서드를 재정의 합니다.
1.  기본 클래스의 상태 속성을 통해 인식 상태 위로 버블링 됩니다.


이 실용적인 예제에서 설명 합니다 [iOS의 터치를 사용 하 여](ios-touch-walkthrough.md) 연습 합니다.
