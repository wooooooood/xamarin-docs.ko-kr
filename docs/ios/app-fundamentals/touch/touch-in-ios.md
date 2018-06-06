---
title: 터치 이벤트 및 Xamarin.iOS에서 제스처
description: 이 문서에서는 터치 이벤트, 멀티 터치, 제스처, 여러 제스처 및 Xamarin.iOS 응용 프로그램에서 사용자 지정 제스처를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 34073474ef3ef74f2fddbf487b3377224dc1aa3e
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784591"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>터치 이벤트 및 Xamarin.iOS에서 제스처

반드시 터치 이벤트를 이해 하 고 iOS 응용 프로그램에서 Api 터치 장치에서 모든 물리적 상호 작용의 중심으로 합니다. 모든 터치 상호 작용을 포함 한 `UITouch` 개체입니다. 이 문서에서 사용 하는 방법을 배웁니다는 `UITouch` 클래스 및 해당 Api 터치를 지원 하도록 합니다. 나중 제스처를 지 원하는 방법에 자세한 기술에 확장 됩니다.

## <a name="enabling-touch"></a>터치 사용

컨트롤에 `UIKit` – 해당 서브클래싱된 UIControl에서 – UIKit에서 기본적으로 제공 하는 제스처 권한이 있는 사용자 상호 작용에 따라서 종속 됩니다와 터치 사용 하는 데 필요한 이므로 합니다. 이미 활성화 된 경우.

그러나의 보기 중 많은 `UIKit` 기본적으로 사용 되는 터치를 갖지 않습니다. 컨트롤에 대 한 터치 사용 하는 방법은 두 가지가 있습니다. 다음 스크린샷에 표시 된 것 처럼 ios 디자이너 속성 패드에서 사용자 상호 작용 사용 확인란을 선택 하 여 첫 번째 방법은 무엇입니까?

 [![](touch-in-ios-images/image1.png "Ios 디자이너 속성 패드에서 사용자 상호 작용 사용 확인란을 선택합니다")](touch-in-ios-images/image1.png#lightbox)

또한 컨트롤러를 설정 하려면 사용할 수는 `UserInteractionEnabled` 속성을 true로는 `UIView` 클래스입니다. 이 코드에서 UI를 만들 경우에 필요 합니다.

다음 코드 줄은 예.

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>터치 이벤트

터치의 사용자 화면을 터치, 자신의 손가락으로 이동 하거나 해당 손가락을 제거할 때 발생 하는 세 단계는 합니다. 에 정의 된 이러한 메서드가 `UIResponder`,이 UIView에 대 한 기본 클래스입니다. iOS에 연결된 된 메서드를 재정의 합니다는 `UIView` 및 `UIViewController` 터치 처리:

-  `TouchesBegan` –이 화면 처음에 접촉 될 때 호출 됩니다.
-  `TouchesMoved` –이 사용자로 터치 변경의 위치는 화면 손가락 상대 (sliding) 하는 경우 호출 됩니다.
-  `TouchesEnded` 또는 `TouchesCancelled` – `TouchesEnded` 화면에 사용자의 손가락 사항은 제거 될 때 호출 됩니다.  `TouchesCancelled` 사용자가 자신의 손가락을 취소 하는 단추 밖 슬라이드 경우 iOS가 터치-예를 들어 취소를 호출을 가져옵니다.


이벤트 여행 재귀적으로 터치 이벤트 view 개체의 범위에 있는지 확인 하려면 UIViews 스택을 통해 터치 합니다. 이 이라고 하는데 _적중 테스트_합니다. 맨 위의 처음 호출 됩니다 `UIView` 또는 `UIViewController` 다음에서 호출 되 고는 `UIView` 및 `UIViewControllers` 뷰 계층 구조에서 그 아래 합니다.

A `UITouch` 개체는 사용자가 화면을 터치 될 때마다 생성 됩니다. `UITouch` 개체 터치 발생 했을 때 발생 한 위치, 터치 등의 통과 했으면 같은 터치에 대 한 데이터를 포함 합니다. 터치 이벤트 작업 속성 – 전달 되는 `NSSet` 하나 이상의 작업을 포함 합니다. 터치에 대 한 참조를 얻으려고이 속성을 사용할 수 있으며 응용 프로그램의 응답을 결정 했습니다.

터치 이벤트 중 하나를 재정의 하는 클래스는 먼저 기본 구현을 호출 하 고 가져온 후 해야는 `UITouch` 이벤트와 연결 된 개체입니다. 첫 번째 터치에 대 한 참조를 가져오려면 호출는 `AnyObject` 속성으로 캐스팅 하 고는 `UITouch` 다음 예에서 같이:

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

iOS 연속 빠른 화면에 및 모두 단일에서 한 번의 탭으로 수집 합니다를 자동으로 인식 `UITouch` 개체입니다. 이렇게 하면 확인 하는 것 만큼 쉽게를 두 번 탭에 대 한 검사는 `TapCount` 속성을 다음 코드 에서처럼:

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

멀티 터치 컨트롤에서 기본적으로 사용 되지 않습니다. 다음 스크린샷에 표시 된 것 처럼 멀티 터치 iOS 디자이너에서에서 활성화할 수 있습니다.

 [![](touch-in-ios-images/image2.png "IOS 디자이너에서에서 사용 하도록 설정 하는 멀티 터치")](touch-in-ios-images/image2.png#lightbox)

설정 하 여 멀티 터치를 프로그래밍 방식으로 설정할 수 이기도 `MultipleTouchEnabled` 코드의 다음 줄에 표시 된 것 처럼 속성:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

개수 손가락 화면 액세스를 확인 하려면 사용 하 여는 `Count` 속성에는 `UITouch` 속성:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>터치 위치 확인

메서드가 `UITouch.LocationInView` 지정된 된 보기 내에서 터치 좌표를 저장 하는 CGPoint 개체를 반환 합니다. 또한 메서드를 호출 하 여 해당 위치가 컨트롤 내에서 있는지 테스트할 수 있습니다 `Frame.Contains`합니다. 다음 코드 조각에서는 이러한 예제를 보여 줍니다.

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

IOS에서 터치 이벤트의 이해를 만들었으므로 이제 해당 제스처 인식기에 대해 알아보겠습니다.

## <a name="gesture-recognizers"></a>제스처 인식기

제스처 인식기 크게 단순화 하 고 응용 프로그램에서 터치를 지원 하기 위해 프로그래밍 작업을 줄일 수 있습니다. iOS 제스처 인식기를 단일 터치 이벤트로는 일련의 터치 이벤트를 집계합니다.

클래스를 제공 하는 Xamarin.iOS `UIGestureRecognizer` 다음 기본 제공 제스처 인식기에 대 한 기본 클래스로:

-  *UITapGestureRecognizer* – 하나 이상의 탭입니다.
-  *UIPinchGestureRecognizer* – Pinching 및 더 많이 떨어져 있는 손가락을 분배 합니다.
-  *UIPanGestureRecognizer* – 패닝 또는 드래그 합니다.
-  *UISwipeGestureRecognizer* – 어느 방향으로든에서 넘기기가 합니다.
-  *UIRotationGestureRecognizer* – 시계 반대 방향으로 또는 시계 반대 방향으로 동작에 두 손가락의 회전 합니다.
-  *UILongPressGestureRecognizer* – 키를 눌러 길게 누르기, 장기 누름 또는 장기 클릭 라고도 합니다.


제스처 인식기를 사용 하 여 기본 패턴은 다음과 같습니다.

1.  **제스처 인식기 인스턴스화할** – 먼저 인스턴스화하는 `UIGestureRecognizer` 하위 클래스입니다. 뷰로 인스턴스화된 개체는 연관 된 하 고의 뷰를 삭제할 때 수집 가비지 됩니다. 클래스 수준 변수로이 뷰를 만들 필요는 없습니다.
1.  **모든 제스처 설정을 구성** – 제스처 인식기를 구성 하는 다음 단계입니다. Xamarin의 설명서를 참조 하십시오. `UIGestureRecognizer` 와의 동작을 제어 하는로 설정할 수 있는 속성의 목록에 대 한 하위 클래스는 `UIGestureRecognizer` 인스턴스.
1.  **대상 구성** – 해당 Objective-c 특성으로 인해 Xamarin.iOS 제스처 인식기는 제스처를 일치 하는 경우 이벤트를 발생 하지 않습니다.  `UIGestureRecognizer` 에 메서드가 – `AddTarget` – 제스처 인식기가 일치 하는 때를 실행 하는 익명 대리자 또는 코드와 Objective-c 선택 기가 있는 받아들일 수 있는 합니다.
1.  **제스처 인식기를 사용 하도록 설정** 터치 이벤트와 함께 제스처만 인식 되는 터치 상호 작용을 사용 하는 것 처럼 – 합니다.
1.  **제스처 인식기에서 뷰에 추가할** – 마지막 단계를 호출 하 여 보기에는 제스처를 추가 하는 `View.AddGestureRecognizer` , 제스처 인식기 개체를 전달 합니다.

참조는 [제스처 인식기 샘플](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) 코드에서 구현 하는 방법에 대 한 자세한 내용은 합니다.

제스처의 대상 호출 될 때 발생 하는 제스처에 대 한 참조가 전달 됩니다. 따라서 제스처 대상을 제스처 발생에 대 한 정보를 얻을 수 있습니다. 정보를 사용할 수 정도 사용 된 제스처 인식기의 유형에 따라 달라 집니다. 각각에 대해 사용할 수 있는 데이터에 대 한 정보에 대 한 Xamarin 설명서를 참조 하십시오 `UIGestureRecognizer` 하위 클래스입니다.

제스처 인식기가 뷰에 추가 되 면 뷰 (및 그 아래의 모든 뷰)는 이벤트를 수신 하지 터치를 기억 하는 것이 유용 합니다. 동시에 제스처를 사용 하 여 터치 이벤트를 허용 하는 `CancelsTouchesInView` 다음 코드에 표시 된 것 처럼 속성을 false로 설정 해야 합니다.

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

각 `UIGestureRecognizer` 에 제스처 인식기의 상태에 대 한 중요 한 정보를 제공 하는 상태 속성이 있습니다. 이 속성의 값이 변경 될 때마다 iOS에서는 업데이트를 받으면 구독 메서드를 호출 합니다. 사용자 지정 제스처 인식기 State 속성을 업데이트 하지, 하는 경우 구독자가 호출 되지 렌더링 제스처 인식기를 사용할 수 없습니다.

두 가지 형식 중 하나로 제스처를 요약할 수 있습니다.

1.  *불연속* – 인식 되는 이러한 제스처만 첫 번째 실행 시간입니다.
1.  *연속* – 인식 되는으로 발생 하도록 이러한 제스처를 계속 합니다.


제스처 인식기 상태는 다음 중 하나에 존재 합니다.

-  *가능한* – 모든 제스처 인식기의 초기 상태입니다. 이것이 기본값 State 속성입니다.
-  *Began* 연속 제스처 인식 되 면 먼저, – 상태 Began로 설정 됩니다. 이렇게 하면 구독 제스처를 인식 시작 될 때 변경 될 때와 구분할 수 있습니다.
-  *Changed* – 연속 제스처 조치가 시작 되었지만 완료 되지 않았습니다, 후 상태로 설정 됩니다 Changed 터치 이동 되거나 변경 될 때마다 제스처의 예상된 매개 변수와 내 여전히으로 합니다.
-  *취소* – 인식기에서 Began에서 Changed 하 고 다음으로 하는 방식에 더 이상으로 변경 합니다. 터치 제스처의 패턴에 맞는 경우이 상태로 설정 됩니다.
-  *인식* – 제스처 인식기 터치 집합과 일치 하 고 제스처 되었음을 구독자 알려줄 것 상태가 설정 됩니다.
-  *종료* -인식 상태에 대 한 별칭입니다.
-  *Failed* – 제스처 인식기 상태는 실패로 변경에 대 한 수신 대기 중인 작업을 더 이상 일치할 수 없습니다.


Xamarin.iOS에서 이러한 값을 나타내는 `UIGestureRecognizerState` 열거형입니다.

## <a name="working-with-multiple-gestures"></a>여러 제스처 사용

기본적으로 iOS를 동시에 실행할 기본 제스처를 허용 하지 않습니다. 대신, 각 제스처 인식기가 명확 하지 않은 순서에서 터치 이벤트를 받게 됩니다. 다음 코드 조각에는 동시에 실행 하는 제스처 인식기를 확인 하는 방법을 보여 줍니다.

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

IOS에서 제스처를 사용 하지 않도록 설정 하는 것도 가능 합니다. 방법에 대 한 사항을 결정 하 고는 제스처를 인식 해야 하는 경우 현재 터치 이벤트와 응용 프로그램의 상태를 검사 하는 제스처 인식기를 사용할 수 있는 두 개의 대리자 속성이 있습니다. 두 이벤트는 같습니다.

1.  *ShouldReceiveTouch* –이 대리자는 제스처 인식기가 터치 이벤트를 전달 하 고 터치를 검사 하 고 어떤 터치 제스처 인식기에서 처리 되는 결정 기회를 제공 하기 전에 바로 호출 됩니다.
1.  *ShouldBegin* – 인식기 다른 상태에서 가능한 상태를 변경 하려고 할 때이 이라고 합니다. False를 반환 하면 실패로 변경 하는 제스처 인식기 상태 시작.


강력한 형식의 이러한 메서드를 재정의할 수 `UIGestureRecognizerDelegate`, 약한 대리자 또는 다음 코드 조각에서와 같이 이벤트 처리기 구문을 통해 바인딩:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

마지막으로 실패 하면 다른 제스처 인식기만 성공할 수 있도록 제스처 인식기를 늘어놓고 수입니다. 예를 들어 단일 탭 제스처 인식기 두 번 탭 제스처 인식기 실패 한 경우에 성공 해야 합니다. 다음 코드 조각에서는 이러한 예제를를 제공합니다.

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>사용자 지정 제스처 만들기

IOS는 몇 가지 기본 제스처 인식기를 제공 하지만 일부 경우에서 사용자 지정 제스처 인식기를 만들어야 할 수 있습니다. 사용자 지정 제스처 인식기를 만드는 단계는 다음과 같습니다.

1.  하위 클래스 `UIGestureRecognizer` 합니다.
1.  적절 한 터치 이벤트 메서드를 재정의 합니다.
1.  기본 클래스의 상태 속성을 통해 인식 상태를 생성 합니다.


이 실제 예를 설명 합니다.는 [터치 iOS를 사용 하 여](ios-touch-walkthrough.md) 연습 합니다.
