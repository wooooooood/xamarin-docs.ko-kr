---
title: Xamarin의 watchOS 3에 대 한 빠른 상호 작용 기술
description: 이 문서에서는 watchOS 3에 추가 된 빠른 상호 작용 기술에 대해 설명 하 고, Apple Watch를 위해 Xamarin.ios에서이를 구현 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 8b851721aa5b2b993ad64b89d90d02b5f2bd0ee3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028183"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Xamarin의 watchOS 3에 대 한 빠른 상호 작용 기술

_이 문서에서는 watchOS 3에 추가 된 빠른 상호 작용 기술에 대해 설명 하 고, Apple Watch를 위해 Xamarin.ios에서이를 구현 하는 방법에 대해 설명 합니다._

빠른 사용자 상호 작용을 제공 하는 것은 뛰어난 Apple Watch 앱 및 복잡 한 문제를 만드는 데 필수적입니다. WatchOS 3에 새로 추가 된 Apple에는 제스처 인식기, Digital Crown에 대 한 액세스 및 새로운 사용자 알림과 탐색 기술에 대 한 지원이 추가 되었습니다. SceneKit 및 SpriteKit 모두에 대 한 추가 지원과 함께 개발자는 빠르고 응답성이 뛰어난 풍부한 glanceable 인터페이스를 쉽게 만들 수 있습니다.

## <a name="what-are-quick-interactions"></a>빠른 상호 작용 이란?

IOS 또는 macOS 용 응용 프로그램을 만드는 데 사용 하는 개발자의 경우 (사용자가 앱과 상호 작용 하는 데 걸린 시간을 분 또는 시간 단위로 측정) Apple Watch에 대 한 성공적인 응용 프로그램을 디자인 하는 것은 어려울 수 있으며 다른 요구 사항이 필요 합니다. 접근.

WatchOS에서 일반적으로 사용자는 손목를 올리고 앱과 신속 하 게 상호 작용 하 고 (일반적으로 몇 초 정도), 손목를 삭제 하 고 수행 하는 모든 작업을 계속 하려고 합니다.

다음은 Apple Watch의 일반적인 빠른 상호 작용에 대 한 몇 가지 예입니다.

- 타이머를 시작 합니다.
- 날씨를 확인 하는 중입니다.
- 항목을 todo 목록에서 표시 하지 않습니다.

이러한 목표를 달성 하려면 Apple Watch 앱은 다음과 같아야 합니다.

- **Glanceable** -사용자가 필요한 정보를 신속 하 게 파악할 수 있음을 의미 합니다. 
- 작업 **가능-사용자** 가 신속 하 고 정확한 의사 결정을 내릴 수 있음을 의미 합니다.
- **응답성** -사용자가 필요한 정보를 수신 하거나 원하는 작업을 얻기 위해 대기 하지 않아야 합니다.

### <a name="quick-interactions-length"></a>빠른 조작 길이

Apple Watch 앱의 glanceable 특성 때문에 Apple은 빠른 상호 작용의 이상적인 길이가 2 초이 하 여야 함을 제안 합니다. 이 두 번째 제한의 결과로 개발자는 Apple Watch 앱을 설계 하 고 구현 하는 데 상당한 시간을 소비 해야 합니다. 

## <a name="new-watchos-3-features-and-apis"></a>New watchOS 3 기능 및 Api

Apple은 개발자가 Apple Watch 앱에 신속 하 게 상호 작용 하는 데 도움을 주는 몇 가지 새로운 기능과 Api를 WatchKit에 추가 했습니다.

- watchOS 3은 다음과 같은 새로운 종류의 사용자 입력에 대 한 액세스를 제공 합니다.
  - 제스처 인식기
  - Digital Crown 회전 
- watchOS 3은 다음과 같이 정보를 표시 하 고 업데이트 하는 새로운 방법을 제공 합니다.
  - 향상 된 테이블 탐색
  - 새 사용자 알림 프레임 워크 지원
  - SpriteKit 및 SceneKit 통합

개발자는 이러한 새로운 기능을 구현 하 여 watchOS 3 앱이 Glanceable, 실행 가능 하 고 응답성을 보장할 수 있습니다.

### <a name="gesture-recognizer-support"></a>제스처 인식기 지원

개발자가 iOS에서 제스처 인식기를 구현한 경우 watchOS 3에서 제스처 인식기가 작동 하는 방식에 대해 잘 알고 있어야 합니다. 새로 고치려면 제스처 인식기는 하위 수준 터치 이벤트를 인식할 수 있는 미리 정의 된 제스처로 구문 분석 하는 개체입니다.

watchOS 3은 다음 네 가지 제스처 인식자를 지원 합니다.

- 불연속 제스처 유형:
  - 살짝 밀기 제스처 (`WKSwipeGestureRecognizer`)입니다.
  - 탭 제스처 (`WKTapGestureRecognizer`)입니다.
- 연속 제스처 유형:
  - 이동 제스처 (`WKPanGestureRecognizer`)입니다.
  - 길게 누르기 제스처 (`WKLongPressGestureRecognizer`)입니다.

새 제스처 인식기 중 하나를 구현 하려면 Mac용 Visual Studio에서 iOS 디자이너의 디자인 화면으로 끌어 놓고 해당 속성을 구성 하면 됩니다.

코드에서 인식기의 작업에 응답 하 여 사용자가 트리거하는 제스처를 처리 합니다. 이는 iOS에서 처리 되는 것과 동일한 방식으로 수행 됩니다.

#### <a name="discrete-gesture-states"></a>불연속 제스처 상태

불연속 제스처의 경우 동작은 제스처가 인식 되 고 상태 (`WKGestureRecognizerState`)는 다음과 같이 할당 될 때 호출 됩니다.

[![](quick-interaction-techniques-images/quick01.png "Discrete Gesture States")](quick-interaction-techniques-images/quick01.png#lightbox)

모든 불연속 제스처는 `Possible` 상태에서 시작 하 여 `Failed` 또는 `Recognized` 상태로 전환 합니다. 불연속 제스처를 사용 하는 경우 개발자는 일반적으로 상태를 직접 처리 하지 않습니다. 대신 제스처가 인식 될 때 호출 되는 작업에 의존 합니다.

#### <a name="continuous-gesture-states"></a>연속 제스처 상태

연속 제스처는 제스처가 인식 될 때 동작을 여러 번 호출 하는 불연속 제스처와 약간 다릅니다.

[![](quick-interaction-techniques-images/quick02.png "Continuous Gesture States")](quick-interaction-techniques-images/quick02.png#lightbox)

다시, 연속 제스처가 `Possible` 상태에서 시작 되지만 여러 업데이트를 통해 진행 됩니다. 여기서 개발자는 제스처의 상태를 고려 하 고 제스처를 마지막으로 `Recognized` 하거나 `Canceled`때까지 `Changed` 단계에서 앱의 UI를 업데이트 해야 합니다.

#### <a name="gesture-recognizer-usage-tips"></a>제스처 인식기 사용 팁

WatchOS 3에서 제스처 인식기를 사용 하 여 작업 하는 경우 Apple에서 다음을 제안 합니다.

- 개별 컨트롤 대신 요소를 그룹화 하는 제스처 인식기를 추가 합니다. Apple Watch의 실제 화면 크기가 작으므로 그룹 요소는 사용자가 적중 하는 데 더 크고 더 쉬운 대상이 될 수 있습니다. 또한 제스처 인식자는 네이티브 UI 컨트롤에 이미 있는 기본 제공 제스처와 충돌할 수 있습니다.
- Watch 앱의 스토리 보드에서 종속성 관계를 설정 합니다.
- 일부 제스처는 다음과 같은 다른 제스처 형식 보다 우선적으로 적용 됩니다.
  - 스크롤
  - Force Touch

### <a name="digital-crown-rotation"></a>Digital Crown 회전

개발자는 watchOS 3 앱에서 Digital Crown 지원을 구현 하 여 사용자에 게 향상 된 탐색 속도 및 전체 자릿수 상호 작용을 제공할 수 있습니다.

WatchOS 2에서 Apple Watch 앱은 `WKInterfacePicker` 개체를 사용 하 여 `WKPickerItems` 목록과 선택기 스타일 (목록, 누적 또는 이미지 시퀀스)을 제공 하 여 Digital Crown에 액세스할 수 있습니다. watchOS 사용자가 Digital Crown를 사용 하 여 목록에서 항목을 선택할 수 있습니다.

`WKInterfacePicker`사용 하는 경우 WatchKit는 다음을 수행 하 여 대부분의 작업을 처리 합니다.

- 목록과 개별 인터페이스 요소를 그립니다.
- Digital Crown 이벤트를 처리 하 고 있습니다.
- 항목을 선택할 때 동작을 호출 합니다.

WatchOS 3의 새로운 기능으로 개발자는 회전 값에 응답 하는 자체 UI 요소를 만들 수 있도록 하는 Digital Crown 회전 이벤트에 직접 액세스할 수 있습니다.

Digital Crown 액세스는 다음 요소에 의해 제공 됩니다.

- `WKCrownSequencer`-초당 회전에 대 한 액세스를 제공 합니다.
- `WKCrownDelegate`-회전 델타 이벤트에 대 한 액세스를 제공 합니다.

#### <a name="rotations-per-second"></a>초당 회전

Digital Crown에서 초당 회전에 액세스 하면 물리학 기반 애니메이션을 사용할 때 유용 합니다. 초당 회전에 액세스 하려면 조사식 확장 `WKInterfaceController`의 `CrownSequencer` 속성을 사용 합니다. 예를 들면,

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>회전 델타

Digital Crown의 회전 델타를 사용 하 여 회전 수를 계산 합니다. `WKCrownDelegate`의 `CrownDidRotate` override 메서드를 사용 하 여 회전 델타에 액세스 합니다. 예를 들면,

```csharp
using System;
using WatchKit;
using Foundation;

namespace MonkeyWatch.MonkeySeeExtension
{
  public class CrownDelegate : WKCrownDelegate
  {
    #region Computed Properties
    public double AccumulatedRotations { get; set;}
    #endregion

    #region Constructors
    public CrownDelegate ()
    {
    }
    #endregion

    #region Override Methods
    public override void CrownDidRotate (WKCrownSequencer crownSequencer, double rotationalDelta)
    {
      base.CrownDidRotate (crownSequencer, rotationalDelta);

      // Accumulate rotations
      AccumulatedRotations += rotationalDelta;
    }
    #endregion
  }
}
```

여기서 앱은 회전 수를 결정 하는 누적 (`AccumulatedRotations`)을 유지 관리 합니다. Digital Crown의 전체 회전 중 하나가 누적 된 델타 `1.0`와 같으며 절반 회전이 `0.5`됩니다.

Apple은 업데이트 되는 UI 요소의 변경 내용 민감도에 따라 회전 수가 어떻게 달라 지는 지를 개발자에 게 나갔습니다.

회전 델타의 부호 (`+/-`)는 사용자가 Digital Crown를 설정 하는 방향을 나타냅니다.

[![](quick-interaction-techniques-images/quick03.png "The sign of the Rotational Delta indicates the direction that the user is turning the Digital Crown")](quick-interaction-techniques-images/quick03.png#lightbox)

사용자가 스크롤하면 WatchKit는 긍정 델타를 반환 하 고 아래로 스크롤하면 사용자가 감시를 제공 하는 방향에 관계 없이 음수 델타가 반환 됩니다.

#### <a name="digital-crown-focus"></a>포커스 Digital Crown

다른 인터페이스 요소와 마찬가지로 Digital Crown에는 포커스 개념이 있습니다. 사용자가 watch와 상호 작용 하는 방법에 따라이 포커스를 Digital Crown에서 다른 인터페이스 요소로 이동할 수 있습니다. 

예를 들어 다음 컨트롤은 Digital Crown 포커스를 도용할 수 있습니다.

- 선택기
- 슬라이더
- 스크롤 컨트롤러

개발자는 사용자 지정 인터페이스 요소가 Digital Crown 포커스 여야 하는 시기를 결정 해야 합니다. Apple은 새 제스처 인식기를 사용 하 여 사용자 지정 UI 요소에 포커스를 얻는 것을 제안 합니다.

### <a name="vertical-paging"></a>세로 페이징

사용자가 watchOS 앱의 테이블 뷰를 탐색 하는 표준 방법은 원하는 데이터 조각으로 스크롤하고 특정 행을 탭 하 여 자세한 보기를 표시 하 고 세부 정보를 볼 때 뒤로 단추를 탭 한 다음,이 프로세스를 반복 하 여 y는 테이블 내에서에 관심이 있습니다.

[![](quick-interaction-techniques-images/quick04.png "Moving between a table and the Detail view")](quick-interaction-techniques-images/quick04.png#lightbox)

WatchOS 3의 새로운 기능으로, 개발자는 해당 테이블 뷰 컨트롤에서 수직 페이징을 사용 하도록 설정할 수 있습니다. 이 기능을 사용 하도록 설정 하면 사용자는 스크롤하여 테이블 뷰 행을 찾고 행을 탭 하 여 이전과 같이 세부 정보를 볼 수 있습니다. 그러나 이제는 먼저 테이블 뷰로 돌아갈 필요 없이 테이블의 다음 행을 선택 하거나 아래로 이동 하 여 이전 행을 선택 하거나 Digital Crown를 사용할 수 있습니다.

[![](quick-interaction-techniques-images/quick05.png "Moving between a table and the Detail view and swiping up and down to move between the other rows")](quick-interaction-techniques-images/quick05.png#lightbox)

이 모드를 사용 하도록 설정 하려면 Xcode에서 편집할 watchOS 앱의 스토리 보드를 열고, 테이블 뷰를 선택 하 고, **세로 세부 정보 페이징** 확인란을 선택 합니다.

[![](quick-interaction-techniques-images/quick06.png "Check the Vertical Detail Paging checkbox")](quick-interaction-techniques-images/quick06.png#lightbox)

테이블에서 Segue를 사용 하 여 자세히 보기를 표시 하 고 스토리 보드에 변경 내용을 저장 한 다음 Mac용 Visual Studio로 반환 하 여 동기화 해야 합니다.

개발자는 테이블 뷰에 대해 다음 코드를 사용 하 여 프로그래밍 방식으로 특정 행에 대 한 수직 페이징을 수행할 수 있습니다.

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

수직 페이징을 사용 하는 경우 개발자는 WatchKit가 컨트롤러의 미리 로드를 자동으로 처리 하 고 결과적으로 UI가 실제로 표시 되기 전에 일부 컨트롤러 수명 주기 메서드를 호출할 수 있음을 알고 있어야 합니다.

### <a name="notification-enhancements"></a>알림 기능 향상

알림은 사용자가 일반적으로 watchOS에서 경험 하 고 첫 번째 Apple Watch watchOS 1부터 사용할 수 있는 빠른 상호 작용의 기본 형식입니다.

일반적인 알림 빠른 상호 작용은 다음과 같습니다.

1. 새 알림이 수신 될 때 사용자에 게 알림이 햅.
2. 손목를 발생 시켜 알림에 대 한 간단한 모양 인터페이스를 확인 합니다.
3. 손목 계속 해 서 계속 발생 하는 경우 watchOS가 자동으로 긴 모양 알림 인터페이스로 전환 됩니다.

사용자가 알림에 응답 하는 방법에는 여러 가지가 있습니다.

- 잘 정의 되 고 표시 된 알림의 경우에는 사용자가 아무 작업도 수행 하지 않고 알림을 해제 하면 됩니다.
- WatchOS 앱을 시작 하는 알림을 탭 할 수도 있습니다.
- 사용자 지정 작업을 지 원하는 알림의 경우 사용자가 사용자 지정 작업 중 하나를 선택할 수 있습니다. 다음 중 하나일 수 있습니다.
  - **포그라운드 작업** -앱을 시작 하 여 작업을 수행 합니다.
  - **백그라운드 작업** -항상 watchOS 2의 iPhone으로 라우팅되고 watchOS 3의 watchApp로 라우팅할 수 있습니다.

WatchOS 3의 새로운 작업:

- 알림은 모든 플랫폼 (iOS, watchOS, tvOS 및 macOS)에서 비슷한 API를 사용 합니다.
- Apple Watch에서 로컬 알림을 예약할 수 있습니다.
- 백그라운드 알림은 Apple Watch에 예약 된 경우 앱의 확장으로 라우팅됩니다.

#### <a name="notification-scheduling-and-delivery"></a>알림 예약 및 배달

사용자의 iPhone에서 알림은 다음과 같은 경우에 Apple Watch 전달 됩니다.

- IPhone의 화면이 꺼져 있습니다.
- Apple Watch 마모 되었으며 잠금이 해제 되었습니다.

WatchOS 3에서 로컬 알림은 Apple Watch 예약할 수 있으며, 시계로만 제공 됩니다. 앱에 필요한 경우 해당 iPhone 알림을 예약 하는 것은 개발자의 작업입니다.

알림의 Apple Watch 및 iPhone 버전 모두에 대해 동일한 알림 식별자를 포함 하 여, 조사식에 중복 알림이 표시 되지 않도록 합니다. 알림의 Apple Watch 버전은 iPhone 버전 보다 우선적으로 적용 됩니다.

WatchOS 3은 iOS 10과 동일한 `UINotification` API 프레임 워크를 사용 하므로 자세한 내용은 iOS 10 [사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md) 설명서를 참조 하세요.

### <a name="using-spritekit-and-scenekit"></a>SpriteKit 및 SceneKit 사용

WatchOS 3의 새로운 기능으로 개발자는 이제 앱의 사용자 인터페이스 디자인에서 SpritKit 및 SceneKit 개체를 모두 사용 하 여 2D 및 3D 그래픽을 모두 표시할 수 있습니다.

이 기능을 지원 하기 위해 새 인터페이스 클래스 두 개가 추가 되었습니다.

- `WKInterfaceSKScene`-SpriteKit 2D 그래픽을 사용 하는 데 사용 됩니다.
- `WKInterfaceSCNScene`-SceneKit 3D 그래픽을 사용 하는 데 사용 됩니다.

이러한 개체를 사용 하려면 Xcode의 Interface Builder에서 watch 앱의 스토리 보드 안쪽에 있는 디자인 화면으로 끌고 **특성 검사자** 를 사용 하 여 구성 하면 됩니다.

이 시점에서 SpriteKit 또는 SceneKit 장면을 사용 하는 작업은 iOS 앱 내에서와 동일 하 게 작동 합니다. Watch 앱은 `Present` 메서드 중 하나를 호출 하 여 `WKInterfaceSKScene`을 제공 합니다. SceneKit의 경우 `WKInterfaceSCNScene` 개체의 `Scene` 속성을 설정 하기만 하면 됩니다.

## <a name="actionable-complications"></a>조치 가능한 문제

WatchOS 2에서 Apple은 타사 앱에 대 한 복잡 한 문제를 도입 했습니다. WatchOS 3에서 Apple은 개발자가 WatchKit 복잡 한 기능을 포함할 수 있는 기능을 확장 했습니다. 

또한 이제는 기본 제공 되는 조사식에 더 많은 복잡성이 포함 될 수 있습니다.

또한 새로운 기능은 사용자가 Apple Watch에 설치 된 모든 조사식 얼굴을 전환 하기 위해 왼쪽 또는 오른쪽으로 신속 하 게 이동할 수 있는 기능입니다. 사용자는 Apple Watch의 자매 iPhone 앱에서 새 갤러리를 사용 하 여 새 조사식 얼굴을 추가 하 고 사용자 지정할 수 있으며,이에 포함 될 수 있는 모든 문제를 사용자 지정할 수 있습니다.

이러한 새로운 기능으로 인해 Apple은 Apple Watch의 모든 앱에 하나 이상의 복잡성이 포함 되어야 함을 제안 하므로 이제 모든 네이티브 Apple Watch 앱에는 복잡 한 문제가 있습니다.

앱에는 다음과 같은 기능을 제공 합니다.

- 이는 항상 watch 면에 표시 되기 때문에 매우 glanceable.
- WatchOS에서 자주 업데이트 되는 문제가 있습니다. 사용자의 현재 표시 된 조사식 얼굴에 대 한 복잡 한 내용이 포함 된 앱은 1 시간 이상 업데이트 됩니다.
- 사용자의 현재 표시 된 조사식 얼굴에 대 한 복잡 한 앱은 앱을 신속 하 게 시작 하 고 앱의 응답 속도를 개선 하는 메모리에 유지 됩니다.
- 사용자가 watchOS 앱에서 특정 기능을 쉽게 시작할 수 있습니다.

## <a name="glanceable-notification"></a>Glanceable 알림

Apple Watch 알림은 사용자에 게 이벤트 또는 들어오는 메시지와 같은 새 정보를 신속 하 게 알리거나 진행 중인 앱에서 목표를 달성할 수 있는 뛰어난 사용자 지정 가능한 방법을 제공 합니다.

알림을 사용 하 여 중요 한 정보를 사용자에 게 신속 하 게 제공할 수 있습니다. 대부분의 경우 잘 설계 된 알림은 사용자가 실제로 앱을 시작 하는 데 필요한 기능을 제거할 수 있습니다.

WatchOS 3의 새로운 기능으로, 모든 알림이 이제를 지원 합니다.

- SpriteKit
- SceneKit
- 인라인 비디오

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>SpriteKit 및 SceneKit가 포함 된 향상 된 UI

일반적으로 개발자는 SpriteKit 및 SceneKit가 언급 될 때 게임 UI를 생각할 수 있습니다. 그러나 SpriteKit 및 SceneKit는 모두 WatchKit 단독으로 가능 하지 않은 사용자 지정 된 레이아웃, 콘텐츠 및 애니메이션을 포함 하는 비 게임 Ui를 만드는 데 유용할 수 있습니다.

예를 들어 사진 공유 앱의 사용자 알림은 SpriteKit를 사용 하 여 사용자 환경을 강화 하는 실제 이미지 및 기타 사용자 지정 정보와 함께 사진을 게시 한 사용자를 포함 하 여 풍부한 사용자 환경을 제공할 수 있습니다.

또한 SpriteKit와 SceneKit는 모두 앱의 사용자 인터페이스 디자인에서 표준 WatchKit UI 요소와 혼합할 수 있습니다.

## <a name="simple-navigation"></a>간단한 탐색

watchOS 3은 개발자가 위에 제공 된 새 [수직 페이징](#vertical-paging), [제스처 인식기 지원](#gesture-recognizer-support) 및 [Digital Crown 회전](#digital-crown-rotation) 기능과 같은 watchOS 앱 내에서 탐색을 단순화할 수 있는 여러 가지 방법을 제공 합니다.

Digital Crown은 Apple Watch에 고유 하며 탐색을 간소화 하는 여러 가지 방법으로 사용할 수 있습니다. 예를 들어 타이머 응용 프로그램은 Digital Crown를 사용 하 여 사용 가능한 타이머 길이를 제거할 수 있습니다.

사용자 지정 제스처는 사용자가 시청 앱과 상호 작용할 수 있는 새롭고 고유한 방법을 제공할 수 있으며 앱 탐색을 간소화 하는 데 사용할 수도 있습니다.

WatchOS 3에 추가 된 새로운 빠른 상호 작용 기능을 결합 하 여 다양 하 고 간편 하 고 빠른 watchOS 앱 인터페이스를 제공 하는 방법을 찾는 Apple 제안

## <a name="finishing-the-quick-interaction"></a>빠른 상호 작용 마무리

잘 디자인 된 빠른 상호 작용 환경을 사용 하면 사용자가 현재 상호 작용을 완료 했을 때 손목 (그리고 앱과 분리 됨)를 삭제할 수 있습니다.

이는 감시 앱이 모든 유형의 네트워크 연결을 수행 하는 경우 또는 자매 iPhone 앱과 정보를 공유 하는 경우에 특히 문제가 됩니다. 이 경우 트랜잭션이 수행 되는 동안 대기 표시기가 나타날 수 있으며,이는 빠른 상호 작용 중에는 바람직하지 않습니다. 다음 예제를 참조하세요.

[![](quick-interaction-techniques-images/quick07.png "Diagram of the watch app doing a network connection and sharing information with its companion iPhone app")](quick-interaction-techniques-images/quick07.png#lightbox)

1. 사용자가 시계에서 구매할 항목을 선택 합니다.
2. 구입 단추를 탭 합니다.
3. 앱은 네트워크 트랜잭션을 시작 하 고 로딩 표시기를 표시 합니다.
4. 잠시 후에 트랜잭션이 완료 되 고 앱이 구매를 표시 합니다.
5. 사용자가 앱을 사용 하 여 손목 및 disengages를 삭제 합니다.

사용자가 트랜잭션이 완료 될 때까지 사용자가 구매 단추를 탭 할 때 로드 표시기를 확인 하는 손목 발생 합니다. 이러한 상황을 해결 하기 위해 Apple에서는 로드 표시기를 표시 하는 대신 개발자가 사용자에 게 즉각적인 피드백을 제공 해야 한다고 제안 합니다. 

Apple의 제안 된 모델을 사용 하 여 동일한 빠른 상호 작용을 다시 살펴보겠습니다.

[![](quick-interaction-techniques-images/quick08.png "Apples suggested model diagram")](quick-interaction-techniques-images/quick08.png#lightbox)

1. 사용자가 시계에서 구매할 항목을 선택 합니다.
2. 구입 단추를 탭 합니다.
3. 앱은 네트워크 트랜잭션을 시작 하 고 구매가 성공적으로 시작 되었다는 메시지를 표시 합니다.
4. 사용자가 앱을 사용 하 여 손목 및 disengages를 삭제 합니다.
5. 나중에 트랜잭션이 성공적으로 완료 되 면 앱은 성공적인 구매를 사용자에 게 알리는 로컬 알림을 표시 합니다.

이번에는 사용자가 구입 단추를 탭 하면 구매가 시작 되었음을 알리는 메시지가 표시 됩니다. 그러면 손목를 안전 하 게 삭제 하 고이 시점에서 빠른 상호 작용을 종료할 수 있습니다. 나중에 사용자 알림에서 트랜잭션의 성공 또는 실패에 대 한 알림을 받습니다. 이러한 방식으로 사용자는 프로세스의 "활성" 단계 중에 앱과만 상호 작용 합니다.

네트워킹을 수행 하는 앱의 경우 백그라운드 `NSURLSession`를 사용 하 여 다운로드 작업을 통해 네트워크 통신을 처리할 수 있습니다. 그러면 앱이 백그라운드에서 해제 다운로드 된 정보를 처리할 수 있습니다. 백그라운드 처리가 필요한 앱의 경우 백그라운드 작업 어설션을 사용 하 여 필요한 처리를 처리 합니다.

## <a name="quick-interaction-design-tips"></a>빠른 상호 작용 디자인 팁

빠른 상호 작용의 원하는 길이는 2 초 이내 이므로 개발자는 디자인 프로세스의 시작 부분에서 앱의 상호 작용 디자인에 집중 해야 합니다. 위에서 설명한 기술을 사용 하 여 이러한 상호 작용을 간소화 하 고 watchOS 3의 새로운 기능을 사용 하 여 앱을 신속 하 고 응답성을 높일 수 있는 영역을 찾습니다.

Apple은 다음을 제안 합니다.

- 앱의 가장 많이 사용 되는 기능을 앞으로 가져와 빠르게 상호 작용에 집중 합니다.
- 복잡 한 사용자 알림을 사용 하 여 일반적인 기능 및 기능을 노출 합니다.
- SceneKit 및 SpriteKit를 사용 하 여 풍부한 glanceable 사용자 인터페이스를 만듭니다.
- 가능 하면 앱 내에서 탐색을 단순화 합니다.
- 사용자가 대기 하지 않도록 하 고 가능한 한 빨리 손목을 삭제 하 고 앱과 함께 분리 하도록 허용 합니다.

## <a name="summary"></a>요약

이 문서에서는 watchOS 3에서 Apple이 추가한 빠른 상호 작용 기법 및 Apple Watch에 대 한 Xamarin.ios에서이를 구현 하는 방법에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
