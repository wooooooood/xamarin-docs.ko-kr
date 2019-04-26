---
title: Xamarin에서 watchOS 3에 대 한 빠른 상호 작용 기술
description: 이 문서에서는 빠른 상호 작용 기술 Apple이 Xamarin.iOS에서 Apple Watch 대 한 구현 하는 방법과 watchOS 3에 추가 합니다.
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 5086724b565fb95274c4988ca1b6e4bb11064575
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082318"
---
# <a name="quick-interaction-techniques-for-watchos-3-in-xamarin"></a>Xamarin에서 watchOS 3에 대 한 빠른 상호 작용 기술

_이 문서에서는 빠른 상호 작용 기술 Apple이 Xamarin.iOS에서 Apple Watch 대 한 구현 하는 방법과 watchOS 3에 추가 합니다._

빠른 사용자 상호 작용을 제공 하는 뛰어난 Apple Watch 앱 및 복잡성을 만드는 데 매우 중요 합니다. 새 watchOS 3 Apple에는 제스처 인식기를 액세스 하 고 디지털 Crown 사용자 알림 및 탐색에 대 한 새로운 기술에 대 한 지원이 추가 되었습니다. 따라서 SceneKit 및 SpriteKit를 둘 다에 대 한 지원이 추가 되었습니다 함께 사용 하면 개발자는 빠르고 응답성 있는 풍부 하 고 glanceable 인터페이스를 쉽게 만들 합니다.

## <a name="what-are-quick-interactions"></a>빠른 상호 작용은 무엇 인가요

IOS 또는 macOS (위치: 사용자는 앱과 상호 작용을 소비 하는 시간의 양을 분 또는 시간 단위로 측정 함)에 대 한 응용 프로그램을 만드는 데 사용 되는 개발자에 게 Apple Watch 대 한 성공적인 앱 디자인은 어려울 수 있습니다 하며 다른 접근 방식입니다.

WatchOS 사용자 일반적으로 해당 손목, 신속 하 게 (일반적으로 간단한 몇 시간 (초))에 대 한 앱과 상호 작용을 손목 삭제 시키고에 있던 수행 된 계속 하려고 합니다.

다음은 몇 가지 예가 Apple Watch 일반적인 빠른 상호 작용입니다.

- 타이머를 시작합니다.
- 날씨를 확인합니다.
- 할 일 목록 항목을 표시 합니다.

이러한 목표를 달성 하려면 Apple Watch 앱 여야 합니다.

- **Glanceable** -즉 빠른을 사용 하 여 사용자 보기는 필요한 정보를 가져올 수 있어야 합니다. 
- **실행 가능한** -즉, 사용자가 신속 하 고 합리적인 결정을 내릴 수 있어야 합니다.
- **반응 형** -즉, 사용자 기다리면 안 필요한 정보를 또는를 강화 하려는 작업을 수행 합니다.

### <a name="quick-interactions-length"></a>빠른 상호 작용 길이

Apple Watch 앱의 glanceable 특성상 Apple에서 제안 빠른 상호 작용 하는 이상적인 길이의 2 초 되도록 미만입니다. 이 두 개의 두 번째 제한으로 인해 개발자 모두 디자인 및 Apple Watch 앱을 구현 하는 시간에 많은 시간을 투자할 해야 합니다. 

## <a name="new-watchos-3-features-and-apis"></a>새 watchOS 3 기능 및 Api

Apple가 WatchKit 개발자는 데 도움이 되는 Apple Watch 앱에 빠른 상호 작용을 추가 하려면 몇 가지 새 기능과 Api를 추가 합니다.

- watchOS 3 사용자와 같은 입력의 새 종류에 대 한 액세스를 제공 합니다.
    - 제스처 인식기
    - 디지털 Crown 회전 
- watchOS 3에 표시 하 고 같은 정보를 업데이트 하는 새로운 방법을 제공 합니다.
    - 향상 된 테이블 탐색
    - 새 사용자 알림 프레임 워크 지원
    - SpriteKit 및 SceneKit 통합

이러한 새 기능을 구현 하 여 개발자가 3 watchOS 앱 Glanceable, 실행 및 반응 형 인지 확인할 수 있습니다.

### <a name="gesture-recognizer-support"></a>제스처 인식기 지원

개발자가 iOS에서 제스처 인식기를 구현, 제스처 인식기 watchOS 3에서에서 작동 하는 방법에 대해 잘 알고 해야 합니다. 를 새로 고치려면 제스처 인식기는 인식할 수 있는, 미리 정의 된 제스처를 낮은 수준의 터치 이벤트를 구문 분석 하는 개체입니다.

watchOS 3 4 다음 제스처 인식기를 지원 합니다.

- 개별 제스처 유형은 다음과 같습니다.
    - 살짝 밀기 제스처 (`WKSwipeGestureRecognizer`).
    - 탭 제스처 (`WKTapGestureRecognizer`).
- 연속 제스처 유형은 다음과 같습니다.
    - 팬 제스처 (`WKPanGestureRecognizer`).
    - 장기 눌러 제스처 (`WKLongPressGestureRecognizer`).

새 제스처 인식기 중 하나를 구현 하려면 Mac 용 Visual Studio의 디자이너 iOS의 디자인 화면으로 끌어 하 고 해당 속성을 구성 합니다.

코드에서 사용자에 의해 트리거되지 제스처를 처리 하는 인식기의 동작에 응답 합니다. 다시, iOS에서 처리 되 고 처럼 동일한 방식으로 수행 됩니다.

#### <a name="discrete-gesture-states"></a>개별 제스처 상태

개별 제스처를 동작 이라고 제스처 인식 되 고 상태 (`WKGestureRecognizerState`)으로 할당 됩니다.

[![](quick-interaction-techniques-images/quick01.png "개별 제스처 상태")](quick-interaction-techniques-images/quick01.png#lightbox)

모든 개별 제스처에서 시작 합니다 `Possible` 상태 및 전환에는 `Failed` 또는 `Recognized` 상태입니다. 경우 일반적으로 개발자 불연속 제스처를 사용 하 여 상태를 직접 처리 하지 않습니다. 대신, 제스처만 인식 되 면 호출 되는 작업에 의존 합니다.

#### <a name="continuous-gesture-states"></a>연속 제스처 상태

연속 제스처는 작업을 여러 번 호출 제스처를 인식 하는 개별 제스처와 약간 다릅니다.

[![](quick-interaction-techniques-images/quick02.png "연속 제스처 상태")](quick-interaction-techniques-images/quick02.png#lightbox)

마찬가지로 연속 제스처에서 시작 된 `Possible` 여러 업데이트를 통해 진행 상태에 있지만. 개발자는 인식기의 상태를 고려 하는 동안 앱의 UI를 업데이트 해야 하는 여기를 `Changed` 제스처 마지막 될 때까지 단계 `Recognized` 또는 `Canceled`합니다.

#### <a name="gesture-recognizer-usage-tips"></a>제스처 인식기 사용 팁

Apple는 watchOS 3에서에서 제스처 인식기를 사용 하 여 작업할 때 다음을 제안 합니다.

- 요소를 그룹화 하려면 개별 컨트롤 대신 제스처 인식기를 추가 합니다. 그룹 요소를 더 큰 경향이 Apple Watch 작은 실제 화면 크기에 있으므로 및 적중를 사용자에 대해 쉽게 대상으로 합니다. 또한 제스처 인식기는 네이티브 UI 컨트롤에서 이미 제스처에서 빌드한와 충돌할 수 있습니다.
- Watch 앱의 스토리 보드에서 종속성 관계를 설정 합니다.
- 일부 제스처 보다 우선적으로 적용 다른 제스처 형식 예:
    - 스크롤
    - Force Touch
 
### <a name="digital-crown-rotation"></a>디지털 Crown 회전

WatchOS 3 앱에서 디지털 Crown 지원을 구현 하 여 사용자에 대 한 속도 전체 자릿수 상호 작용 개발자 향상 된 탐색 제공할 수 있습니다.

WatchOS 2 이후로 Apple Watch 앱의 사용할 수는 `WKInterfacePicker` 개체에 디지털 Crown의 목록을 제공 하 여 액세스 `WKPickerItems` 및 선택기 스타일 (목록, 누적 또는 이미지 순서). watchOS에는 다음 사용자 디지털 Crown 목록에서 항목을 선택 하는 데 사용할 수 있습니다.

사용 하는 경우는 `WKInterfacePicker`, WatchKit 대부분의 작업을 처리 하는:

- 목록 및 개별 인터페이스 요소를 그리기입니다.
- 디지털 Crown 이벤트를 처리 합니다.
- 항목이 선택 될 때 작업을 호출 합니다.

새 watchOS 3에 개발자 이제 직접 액세스할 수 있으며 디지털 Crown 회전 이벤트 회전 값에 응답 하는 고유한 UI 요소를 만들 수 있습니다.

디지털 Crown 액세스는 다음 요소에 의해 제공 됩니다.

- `WKCrownSequencer` -초당 회전에 대 한 액세스를 제공합니다.
- `WKCrownDelegate` -회전 델타 이벤트에 대 한 액세스를 제공합니다.

#### <a name="rotations-per-second"></a>초당 회전

에 액세스 하는 회전 초당 디지털 Crown에서 물리학을 사용 하 여 작업 기반 애니메이션 하는 경우 유용 합니다. 회전 초당에 액세스 하려면 사용 하 여는 `CrownSequencer` 의 속성은 `WKInterfaceController` 조사식 확장 합니다. 예를 들어:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>회전 델타

회전 수를 계산 하려면 디지털 Crown에서 회전 델타를 사용 합니다. 사용 하 여는 `CrownDidRotate` 의 메서드를 재정의 `WKCrownDelegate` 회전 델타를 액세스할 수 있습니다. 예를 들어:

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

앱에서 누적 된 값을 유지 하는 여기 (`AccumulatedRotations`) 회전의 수를 결정 합니다. 디지털 Crown의 전체 회전 값과의 누적 된 델타를 같음 `1.0` 절반 회전을 고 `0.5`입니다.

Apple이이 회전 수를 업데이트 중인 UI 요소에 대 한 변경의 민감도에 어떻게 대응 하는지 확인 하려면 개발자에 게 달려 나갔습니다.

로그인 (`+/-`) 회전 델타의 사용자의 디지털 Crown 회전 하는 방향을 나타냅니다.

[![](quick-interaction-techniques-images/quick03.png "회전 델타의 부호는 사용자의 디지털 Crown 회전 하는 방향을 나타내는합니다")](quick-interaction-techniques-images/quick03.png#lightbox)


사용자를 스크롤 하는 양의 델타 반환 하는 경우 아래로 스크롤하여, 다음 음수 델타는 어떤 방향 사용자에서 조사식 입고 되 든 WatchKit 반환 됩니다.

#### <a name="digital-crown-focus"></a>디지털 Crown 포커스

다른 인터페이스 요소와 마찬가지로 디지털 Crown에 포커스 개념이 있습니다. 이 이는 사용자는 시계와 상호 작용 하는 방법에 따라 다른 인터페이스 요소에 디지털 Crown에서 변할 수 있습니다. 

예를 들어 디지털 Crown의 포커스를 도용 수 다음 컨트롤 중 하나:

- 선택기
- 슬라이더
- 스크롤 컨트롤러

해당 사용자 지정 인터페이스 요소 디지털 Crown의 포커스를 해야 하는 시기를 결정 하는 개발자가 합니다. Apple에서 제안 된 새 제스처 인식기를 사용 하 여 사용자 지정 UI 요소에 포커스를 얻게 하 게 합니다.

### <a name="vertical-paging"></a>세로 페이징

표준 사용자가 watchOS 앱에서 테이블 뷰를 탐색 하는 방법은 원하는 데이터 부분을 스크롤 하 고, 자세한 뷰를 표시, 세부 정보를 보고 하는 경우 뒤로 단추를 눌러 특정 행을 탭 하 고, 기타 정보에 대 한 프로세스를 반복 하는 y가 관심의 테이블 내에서:

[![](quick-interaction-techniques-images/quick04.png "테이블 및 세부 정보 보기 간 이동")](quick-interaction-techniques-images/quick04.png#lightbox)

새 watchOS 3에 개발자 설정할 수 있습니다. 세로 페이징에서 해당 테이블 뷰 컨트롤입니다. 이 기능을 사용 하면 사용자 테이블 뷰 행을 찾을 탭 하기 전에 해당 세부 정보를 보려면 행을 스크롤할 수입니다. 그러나 이러한 수 이제 위쪽으로 살짝까지 이전 행을 선택 또는 테이블의 다음 행을 선택 (또는 디지털 Crown 사용)를 테이블 보기로 돌아가려면 필요 없이 먼저:

[![](quick-interaction-techniques-images/quick05.png "테이블 및 세부 정보 보기 간 이동 및 넘기기가 위아래로 다른 행 사이 이동 하려면")](quick-interaction-techniques-images/quick05.png#lightbox)

이 모드를 사용 하려면 Xcode에서 편집을 위해 watchOS 앱의 스토리 보드를 엽니다, 테이블 보기를 선택 하 고 확인 합니다 **세로 세부 페이징** 확인란:

[![](quick-interaction-techniques-images/quick06.png "세로 세부 페이징 확인란")](quick-interaction-techniques-images/quick06.png#lightbox)

테이블 자세한 뷰를 표시 하 고 스토리 보드에 변경 내용을 저장 및 동기화 하는 Mac 용 Visual Studio로 돌아가서 고 Segues를 사용 하는지 확인 합니다.

개발자 참여할 수 있습니다 프로그래밍 방식으로 세로 페이징 표 보기에 대해 다음 코드를 사용 하 여 특정 행으로:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

세로 페이징을 사용 하는 경우 개발자는 WatchKit는 컨트롤러의 미리 로드를 자동으로 처리 하며 결과적으로 일부 컨트롤러 수명 주기 메서드를 호출할 수 전에 UI가 실제로 표시 해야 합니다.

### <a name="notification-enhancements"></a>알림 향상

알림은 사용자는 일반적으로 watchOS에서 발생 하는 빠른 상호 작용의 기본 형식 및 첫 번째 Apple Watch 및 watchOS 1 이후 제공 되었습니다.

일반적인 알림 빠른 상호 작용은 다음과 같습니다.

1. 사용자 자신을 새 알림이 수신 되 면 알림 햅 틱입니다.
2. 알림에 대 한 짧은 표시 인터페이스를 보려면 해당 손목을 발생 시킵니다.
3. 계속 발생 하는 손목, watchOS 긴 표시 알림 인터페이스에 자동으로 전환 됩니다.

사용자 알림을 응답할 수 있는 방법은 여러 가지가 있습니다.

- 제대로 정의 및 알림 표시에 대 한 사용자 아무 작업도 수행 되며 알림을 해제 하면 됩니다.
- WatchOS 앱을 시작 하는 알림의 탭 수 할 합니다.
- 사용자는 알림을 지 원하는 사용자 지정 작업에 대 한 사용자 지정 작업 중 하나를 선택할 수 있습니다. 일 수 있습니다.
    - **포그라운드 작업** -이러한 작업을 수행 하려면 앱을 시작 합니다.
    - **작업을 백그라운드** -watchOS 2만 방향의 iPhone에 항상 라우팅된 하지만 watchOS 3에에서 watchApp로 라우팅할 수 있습니다.

WatchOS 3에 대 한 새:

* 모든 플랫폼 (iOS, watchOS, tvOS 및 macOS)에서 비슷한 API를 사용 하는 알림입니다.
* Apple Watch 로컬 알림을 예약할 수 있습니다.
* Apple Watch 예약 된 경우 백그라운드 알림 응용 프로그램의 확장으로 라우팅됩니다.

#### <a name="notification-scheduling-and-delivery"></a>알림 일정 예약 및 배달

사용자의 iPhone에서 알림이 됩니다 앞으로 Apple Watch 다음과 같은 경우:

* IPhone의 화면 해제 되어 있습니다.
* Apple Watch 착용 및 잠금 해제 되었습니다.

WatchOS 3에서에서 로컬 알림 Apple Watch 예약할 수 있습니다 및 시계에만 제공 됩니다. 앱에서 필요로 하는 경우는 해당 iPhone 알림을 예약 하는 개발자는 것입니다.

Apple Watch 및 iPhone 버전 알림 모두에서 동일한 알림 식별자를 포함 하 여 중복 된 알림을 시계에 표시 되지 않도록 합니다. Apple Watch 버전 알림의 iPhone 버전에 비해 우선을 적용 됩니다.

WatchOS 3을 사용 하 여 동일 하므로 `UINotification` iOS 10으로 API 프레임 워크를 참조 하세요. iOS 10 [사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md) 자세한 세부 정보에 대 한 설명서입니다.

### <a name="using-spritekit-and-scenekit"></a>SpriteKit 및 SceneKit을 사용 하 여

새 watchOS 3에 개발자 이제 개체를 사용할 수 SpritKit와 SceneKit 해당 앱의 사용자 인터페이스 디자인에서 2D 및 3D 그래픽을 제공 합니다.

이 기능을 지원 하도록 새로운 인터페이스 클래스 두 개 추가 되었습니다.

- `WKInterfaceSKScene` -에 대 한 2D 그래픽 SpriteKit 사용 합니다.
- `WKInterfaceSCNScene` -작업용 SceneKit 3D 그래픽을 사용 합니다.

이러한 개체를 사용 하려면 단순히 watch 앱의 Xcode의 Interface Builder 스토리 보드 내에서 디자인 화면으로 끌어와 사용 된 **특성 검사기** 구성 합니다.

이 시점부터 SpriteKit 또는 SceneKit 백그라운드에서 작업 작동 동일한 iOS 앱 내에서 마찬가지로 합니다. Watch 앱 제공을 `WKInterfaceSKScene` 중 하나를 호출 하 여는 `Present` 메서드. SceneKit을 설정 하기만 하면 됩니다 합니다 `Scene` 의 속성을 `WKInterfaceSCNScene` 개체입니다.

## <a name="actionable-complications"></a>실행 가능한 복잡성

WatchOS 2만, Apple는 타사 앱에 대 한 복잡성을 도입 했습니다. WatchOS 3에에서 Apple 개발자 WatchKit Complication에 포함할 수 있는 기능을 확장 했습니다. 

또한 더 많은 기본 제공 조사식에 얼굴 이제 복잡성 등이 있습니다 기존 조사식 얼굴 이미 지원 되는 복잡성 수 이제 포함 더욱 복잡 합니다.

또한 새 기능은 사용자에 대 한 신속 하 게 안쪽으로 살짝 밀어 왼쪽 또는 오른쪽으로 해당 Apple Watch 설치한 조사식 얼굴의 모든 전환에입니다. 새 갤러리를 사용 하 여 Apple Watch 도우미 iPhone 앱에서 사용자를 추가 및 새 조사식 얼굴 및 포함할 수 있으므로 문제 중 하나를 사용자 지정.

이러한 새 기능으로 인해 Apple에서 제안 하는 모든 응용 프로그램에 Apple Watch Complication 하나 이상 포함 해야 있고 따라서 기본 Apple Watch 앱의 모든 이제 복잡 합니다.

앱에 다음 기능을 제공 하는 복잡성:

- 이들은 항상 glanceable watch 화면에 항상 이기 때문입니다.
- 복잡성은 watchOS에서 자주 업데이트 됩니다. 복잡 한 사용자의 현재 표시 된 watch 화면에 포함 된 모든 앱에는 1 시간 이상 두 번 업데이트 됩니다.
- 모든 앱 사용자의 현재 표시 된 watch 화면에는 복잡 한를 사용 하 여 신속 하 게 시작 하는 앱을 만들고 앱의 응답 속도 개선 하는 메모리에 보관 됩니다.
- 복잡성이를 사용 하면 쉽게 사용자 watchOS 앱에서 특정 기능을 시작할 수 있습니다.

## <a name="glanceable-notification"></a>Glanceable 알림

Apple Watch 알림이 사용자에 게 신속 하 게 알려 이벤트 또는 들어오는 메시지 또는 운동 앱에서 목표를 달성 하기와 같은 새 정보는 유용한, 사용자 지정 가능한 방법을 제공 합니다.

알림을 사용 하면 중요 한 정보를 사용자에 게 신속 하 게 표시할 수 있습니다. 대부분의 경우에서 잘 디자인 된 알림 사용자가 실제로 앱을 시작 하는 필요성을 제거할 수 있습니다.

WatchOS 3, 모든 알림 이제 지원 접하는:

- SpriteKit
- SceneKit
- 인라인 비디오

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>SpriteKit 및 SceneKit을 사용 하 여 향상 된 UI

일반적으로 개발자 SpriteKit 및 SceneKit 멘 션 하는 경우 게임 UI의 생각 합니다. 그러나 SpriteKit와 SceneKit 게임이 아닌 사용자 지정된 레이아웃, 콘텐츠를 포함 하는 Ui 및 WatchKit 만으로 가능 하지 않은 애니메이션 만들기에 대 한 유용할 수 있습니다.

예를 들어, 사진 공유 앱에서에서 사용자 알림을 SpriteKit를 사용 하 여 실제 이미지 및 사용자 환경을 강화 하는 사용자 지정 된 다른 정보와 함께 그림을 게시 하는 사용자를 포함 하 여 풍부한 사용자 환경을 제공 하 수 있습니다.

또한 앱의 사용자 인터페이스 디자인에서 표준 WatchKit UI 요소를 사용 하 여 SpriteKit와 SceneKit을 혼합할 수 있습니다.

## <a name="simple-navigation"></a>간단한 탐색

watchOS 3에서는 여러 가지 방법으로 개발자가 같은 새 watchOS 앱 내 탐색을 간소화할 수 있습니다 [세로 페이징](#vertical-paging)하십시오 [제스처 인식기 지원](#gesture-recognizer-support) 및 [디지털 Crown 회전](#digital-crown-rotation) 기능 위에 표시 합니다.

디지털 Crown Apple Watch 고유 하며 탐색을 간소화 하기 위해 다양 한 방법으로 사용할 수 있습니다. 예를 들어 타이머 응용 프로그램을 통해 사용할 수 있는 타이머 길이 밖에도 디지털 Crown를 사용할 수입니다.

사용자 지정 제스처 watch 앱을 사용 하 여 상호 작용할 수에 대 한 새롭고 고유한 방식으로 제공할 수 및 앱 탐색을 간소화 하기 위해 사용할 수도 있습니다.

Apple 모든 watchOS 3 쉽고 빠르게 watchOS 앱 인터페이스를 사용 하는 다양 한 표시에 추가 된 새로운 빠른 상호 작용 기능을 결합 하는 방법에 대 한 확인 하는 것이 좋습니다.

## <a name="finishing-the-quick-interaction"></a>빠른 상호 작용이 완료

잘 설계 된 빠른 상호 작용 경험을 나옴 사용자 안심 하 고 해당 손목 삭제 (및 해제 하는 앱을 사용 하 여) 이러한 테스트가 완료 되 면 현재 상호 작용 합니다.

특히 이것이 하는 경우 watch 앱은 모든 종류의 네트워크 연결을 수행 또는 해당 도우미 iPhone 앱을 사용 하 여 정보를 공유 하는 경우는 문제가입니다. 이 발생할 수 있습니다 자주 대기 표시기가 트랜잭션이 진행 되는 동안, 바람직하지 않을 빠른 상호 작용 하는 중입니다. 다음 예제를 참조하세요.

[![](quick-interaction-techniques-images/quick07.png "네트워크 연결을 수행 하 고 해당 도우미 iPhone 앱을 사용 하 여 정보 공유 watch 앱 다이어그램")](quick-interaction-techniques-images/quick07.png#lightbox)

1. 사용자는 시계에 구매할 항목을 선택 합니다.
2. 이러한 구입 단추를 누릅니다.
3. 앱은 네트워크 트랜잭션을 시작 하 고 로드 표시기를 표시 합니다.
4. 얼마 후에 트랜잭션이 완료 하 고 앱 구매 conformation를 표시 합니다.
5. 사용자는 자신의 손목을 삭제 하 고 앱과 분리 합니다.

트랜잭션이 완료 될 때까지 사용자가 구매 단추를 누를 경우에서 로드 지표를 살펴보면 발생 하는 손목을 갖습니다. 이 문제를 해결 하려면 Apple 개발자 로드 표시기를 표시 하는 대신 사용자에 게 즉각적인 피드백을 제공 해야는 제안 합니다. 

Apple의 제안 된 모델을 사용 하는 같은 빠른 상호 작용 다시 살펴보겠습니다를 수행 합니다.

[![](quick-interaction-techniques-images/quick08.png "사과 제안 된 모델 다이어그램")](quick-interaction-techniques-images/quick08.png#lightbox)

1. 사용자는 시계에 구매할 항목을 선택 합니다.
2. 이러한 구입 단추를 누릅니다.
3. 앱은 네트워크 트랜잭션을 시작 하 고 구매 성공적으로 시작 하 라는 메시지가 표시 됩니다.
4. 사용자는 자신의 손목을 삭제 하 고 앱과 분리 합니다.
5. 트랜잭션이 잠시를 성공적으로 완료 되 면 앱 성공적인 구매의 사용자에 게 알려 로컬 알림을 표시 합니다.

이때 사용자가 안심 하 고 해당 손목을 삭제 하 고 빠른 상호 작용이 시점에서 종료 수 있도록 구매에 시작 하 라는 메시지가 표시 됩니다 구입 단추 탭으로 합니다. 나중에 사용자 알림을 트랜잭션의 성공 여부의 내용 알림이 표시 됩니다. 이러한 방식으로 사용자는만 앱과 상호 작용은 프로세스의 "활성" 단계 중.

네트워킹을 수행 하는 앱에 대 한 배경 낼 수 있습니다 `NSURLSession` 다운로드 작업을 사용 하 여 네트워크 통신을 처리 하도록 합니다. 이렇게 하면 앱을 다운로드 한 정보를 처리 하는 데 백그라운드에서 해제 합니다. 백그라운드 처리를 필요로 하는 앱에 대 한 백그라운드 작업 어설션을 사용 하 여 필요한 처리를 처리 합니다.

## <a name="quick-interaction-design-tips"></a>빠른 상호 작용 디자인 팁

빠른 상호 작용의 원하는 길이 2 초 이하의 개발자는 디자인 프로세스의 처음부터 앱의 상호 작용의 디자인에 집중 해야 합니다. 이러한 상호 작용 (위에서 설명한 기법을 사용 하 여) 단순화할 수 있습니다. 및 watchOS 3의 새로운 기능을 사용 하 여 신속 하 고 응답성이 뛰어난 앱을 확인 하는 영역을 찾습니다.

다음 Apple에서 제안:

- 앱의 자주 사용 하는 기능을 앞으로 전환 하 여 빠른 상호 작용에 집중 합니다.
- 일반적인 특징과 기능을 노출 하려면 복잡 한 사용자 알림을 사용 합니다.
- SceneKit 및 SpriteKit를 사용 하 여 풍부 하 고 glanceable 사용자 인터페이스를 만듭니다.
- 가능 하면 앱 내에서 탐색을 간소화 합니다.
- 대기, 해당 손목을 삭제 하 고 앱을 사용 하 여 가능한 한 빨리 해제 수 있도록 사용자를 만들면 안 됩니다.

## <a name="summary"></a>요약

이 문서에서는 빠른 상호 작용 기술에서는 이러한 부분이 Apple이 Xamarin.iOS에서 Apple Watch 대 한 구현 하는 방법과 watchOS 3에 추가 합니다.

## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://developer.xamarin.com/samples/watchos/all/)
