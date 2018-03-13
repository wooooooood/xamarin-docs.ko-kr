---
title: "WatchOS 3에 대 한 빠른 상호 작용 방법"
description: "이 문서에서는 빠른 상호 작용 하는 기술을 Apple Apple Watch 대 한 Xamarin.iOS에 구현 하는 방법과 watchOS 3에 추가 되었습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 26697F68-AF7E-4A36-988F-85E2674A4DD1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: bf93744914a0caf4f6599fc333ae200468d66e48
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="quick-interaction-techniques-for-watchos-3"></a>WatchOS 3에 대 한 빠른 상호 작용 방법

_이 문서에서는 빠른 상호 작용 하는 기술을 Apple Apple Watch 대 한 Xamarin.iOS에 구현 하는 방법과 watchOS 3에 추가 되었습니다._

빠른 사용자 상호 작용을 제공 하는 강력한 Apple Watch 응용 프로그램 및 복잡성을 만드는 데 매우 중요 합니다. 새로운 3 watchOS 하기 위해 Apple 제스처 인식기, 디지털 왕관 및 새 사용자 알림 및 탐색 기술에 대 한 액세스에 대 한 지원을 추가 했습니다. 이렇게, SceneKit와 SpriteKit에 대 한 지원 추가 함께 수 개발자가 다양 하 고 동적인 인터페이스를 신속 하 고 응답을 쉽게 만들 수 있습니다.

## <a name="what-are-quick-interactions"></a>빠른 상호 작용은 무엇입니까

개발자 iOS 또는 macOS (여기서는 사용자가 응용 프로그램 보낸 시간 단위로 측정 됩니다 분 단위 또는 시간)에 대 한 응용 프로그램을 만드는 데 사용 되는, Apple Watch 대 한 성공적인 앱 디자인은 어려울 수 있습니다 및 다른 필요 접근 방식입니다.

WatchOS, 일반적으로 만들려고 자신의 손목 발생, 신속 하 게와 상호 작용 하는 응용 프로그램 (일반적으로 대 한 간단한 몇 초)을 다음 자신의 손목 drop 무엇이 든가 수행 된 계속 합니다.

다음은 몇 가지 Apple Watch 빠른 일반적인 상호 작용입니다.

- 타이머를 시작합니다.
- 날씨 확인 중입니다.
- 할 일 목록 밖으로 항목을 표시 합니다.

이러한 목표를 달성 하기 위해에서 Apple Watch 앱 이어야 합니다.

- **동적인** -빠른와 사용자 보기을 의미 하는 필요한 정보를 가져올 수 있어야 합니다. 
- **실행 가능한** -사용자가 의미 하는 신속 하 고 올바른 의사 결정을 내릴 수 있어야 합니다.
- **응답성이 뛰어난** -사용자 기다리면 안 필요한 정보를 수신 하거나을 원하는 작업을 제공 하기.

### <a name="quick-interactions-length"></a>빠른 상호 작용 길이

Apple Watch 앱의 동적인 특성상 Apple 제안 빠른 상호 작용의 이상적인 길이 2 초 되도록 제한 합니다. 이 두 개의 두 번째 제한으로 인해 개발자 상당 둘 다 디자인 및 Apple Watch 앱을 구현 하는 시간을 소비 해야 합니다. 

## <a name="new-watchos-3-features-and-apis"></a>새 watchOS 3 가지 기능 및 Api

Apple이 추가 몇 가지 새로운 기능과 Api WatchKit 해결 하기 위한 Apple Watch 앱을 빠르게 상호 작용에 추가.

- watchOS 3 새로운 종류의 사용자와 같은 입력에 대 한 액세스를 제공 합니다.
    - 제스처 인식기
    - 디지털 왕관 회전 
- watchOS 3 표시 하 고 같은 정보를 업데이트 하는 새로운 방법을 제공 합니다.
    - 향상 된 테이블 탐색
    - 새 사용자 알림 프레임 워크 지원
    - SpriteKit 및 SceneKit 통합

이러한 새 기능을 구현 하 여 개발자는 watchOS 3 앱 Glanceable, 실행 및 Responsive 되는지 확인할 수 있습니다.

### <a name="gesture-recognizer-support"></a>제스처 인식기 지원

개발자가 iOS에서 제스처 인식기 구현 제스처 인식기 watchOS 3에서에서 작동 하는 방법에 대해 잘 알고 수 있어야 합니다. 를 새로 고치려면 제스처 인식기는 인식할 수 있는, 미리 정의 된 제스처를 하위 수준 터치 이벤트를 구문 분석 하는 개체입니다.

4 명의 다음 제스처 인식기 watchOS 3을 지원 합니다.

- 개별 제스처 유형:
    - 살짝 밀기 (`WKSwipeGestureRecognizer`).
    - 탭 제스처 (`WKTapGestureRecognizer`).
- 연속 제스처 유형:
    - 팬 제스처 (`WKPanGestureRecognizer`).
    - 장기 키를 눌러 제스처 (`WKLongPressGestureRecognizer`).

새 제스처 인식기 중 하나를 구현 하려면 Mac 용 iOS Visual Studio의 디자이너에서에서 디자인 화면으로 끌어 하 고 속성을 구성 합니다.

코드에서 처리는 사용자에 의해 트리거된 제스처 인식기의 동작에 응답 합니다. 다시, 이렇게 같은 방법으로 iOS에서 처리 될 것으로 합니다.

#### <a name="discrete-gesture-states"></a>개별 제스처 상태

개별 제스처에 대 한 제스처 인식 되 면 및 상태는 작업이 호출 된 (`WKGestureRecognizerState`)으로 할당 됩니다.

[![](quick-interaction-techniques-images/quick01.png "개별 제스처 상태")](quick-interaction-techniques-images/quick01.png#lightbox)

모든 개별 제스처 시작는 `Possible` 상태 및 전환 중 하나로 `Failed` 또는 `Recognized` 상태입니다. 때 일반적으로 개발자 불연속 제스처를 사용 하 여 상태를 직접 처리 하지 않습니다. 대신 제스처만 인식 되 면 호출 되는 작업을 사용 합니다.

#### <a name="continuous-gesture-states"></a>연속 제스처 상태

연속 제스처는 여기서 작업을 여러 번 호출 제스처를 인식 하는 개별 제스처를 약간 다릅니다.

[![](quick-interaction-techniques-images/quick02.png "연속 제스처 상태")](quick-interaction-techniques-images/quick02.png#lightbox)

다시, 연속 제스처에서 시작는 `Possible` 상태에 있지만 여러 업데이트 동안 진행 합니다. 개발자 인식기의 상태는 고려 하 하는 동안 응용 프로그램의 UI를 업데이트 해야 합니다는 여기에서 `Changed` 제스처 마지막 될 때까지 단계 `Recognized` 또는 `Canceled`합니다.

#### <a name="gesture-recognizer-usage-tips"></a>제스처 인식기 사용 팁

Apple는 제스처 인식기 watchOS 3에서에서 작업할 때는 다음을 제안 합니다.

- 요소를 그룹화 하는 대신 개별 컨트롤 제스처 인식기를 추가 합니다. 그룹 요소를 더 큰 경향이 Apple Watch 더 작은 실제 화면 크기에 있으므로 및 적중를 사용자에 대 한 보다 쉽게 대상으로 합니다. 또한 제스처 인식기 내장 네이티브 UI 컨트롤에 이미 있는 제스처와 충돌할 수 있습니다.
- Watch 앱 스토리 보드의 종속 관계를 설정 합니다.
- 일부 제스처 우선 다른 제스처 종류와 같은:
    - 스크롤
    - 터치를 적용 합니다.
 
### <a name="digital-crown-rotation"></a>디지털 왕관 회전

자신의 watchOS 3 앱에서 디지털 왕관 지원을 구현 하 여 해당 사용자에 대 한 속도 전체 자릿수 상호 작용 개발자 증가 탐색 제공할 수 있습니다.

Apple Watch 앱 watchOS 2 이후로 사용할 수는 `WKInterfacePicker` 목록을 제공 하 여 디지털 왕관에 액세스 하는 개체 `WKPickerItems` 및 선택기 스타일 (목록, 세로형 또는 이미지 시퀀스). 다음 watchOS 디지털 왕관 목록에서 항목을 선택 하는 데 사용자를 허용 합니다.

사용 하는 경우는 `WKInterfacePicker`, WatchKit 하 여 작업의 대부분을 처리 하는:

- 목록 및 각 인터페이스 요소를 그리기입니다.
- 디지털 왕관 이벤트를 처리 합니다.
- 항목을 선택 하는 경우 작업을 호출 합니다.

새로운 3 watchOS 개발자 이제에 직접 액세스 디지털 왕관 회전 이벤트를 회전 값에 응답 하는 고유한 UI 요소를 만들 수 있습니다.

디지털 왕관 액세스는 다음과 같은 요소에 의해 제공 됩니다.

- `WKCrownSequencer` -초당 회전에 대 한 액세스를 제공합니다.
- `WKCrownDelegate` -회전 델타 이벤트에 대 한 액세스를 제공합니다.

#### <a name="rotations-per-second"></a>초당 회전

에 액세스 하는 회전 Per Second 디지털 왕관에서 물리 작업할 애니메이션을 기반으로 하는 경우 유용 합니다. 회전 Per Second에 액세스 하려면 사용 하 여는 `CrownSequencer` 의 속성은 `WKInterfaceController` 조사식 확장의 합니다. 예:

```csharp
var rotationsPerSecond = CrownSequencer.RotationsPerSecond;
```

#### <a name="rotational-deltas"></a>회전 델타

디지털 왕관에서 회전 델타를 사용 하 여 회전 수를 계산 합니다. 사용 하 여는 `CrownDidRotate` 의 메서드를 재정의 하는 `WKCrownDelegate` 회전 델타를 액세스할 수 있습니다. 예:

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

누적 응용 프로그램 유지 관리 하는 여기 (`AccumulatedRotations`) 회전 수를 결정 합니다. 누적 된 델타에 디지털 왕관의 한 전체 표시 크거나 `1.0` 절반 회전 될 수 `0.5`합니다.

Apple 상태로 남아 서 회전 개수 변경 내용 업데이트 하 고 UI 요소에 대해 민감도에 해당 하는 방식을 결정 하는 개발자의 책임입니다.

기호 (`+/-`) 회전 델타의 사용자의 디지털 왕관 회전 하는 방향을 나타냅니다.

[![](quick-interaction-techniques-images/quick03.png "회전 델타의 부호는 사용자의 디지털 왕관 회전 하는 방향을 나타내는합니다")](quick-interaction-techniques-images/quick03.png#lightbox)


사용자를 스크롤 하는 경우 양수 델타 고 하는 경우 아래로 스크롤하여 다음 음수 델타 반환 됩니다, 방향 사용자에서 조사식 착용는 관계 없이 WatchKit 반환 합니다.

#### <a name="digital-crown-focus"></a>디지털 왕관 포커스

다른 인터페이스 요소와 마찬가지로 디지털 왕관 포커스의 개념을 있습니다. 이 기준의 사용자가 시계와 상호 작용 하는 방법에 따라 다른 인터페이스 요소가 디지털 왕관 이동 될 수 합니다. 

예를 들어 디지털 왕관의 포커스를 도용 하 고 수 다음 컨트롤 중 하나:

- 선택
- 슬라이더
- 스크롤 컨트롤러

개발자가 사용자 지정 인터페이스 요소 디지털 왕관의 주제를 만들어야 하는 시기를 결정 하는 합니다. Apple 새 제스처 인식기를 사용 하 여 사용자 지정 UI 요소에 포커스를 제안 합니다.

### <a name="vertical-paging"></a>세로 페이징

데이터의 원하는 부분으로 스크롤하고 자세히 보기에는 표시 하려면 세부 정보 확인을 마쳤으면 뒤로 단추를 눌러 특정 행에 탭 또는 기타 정보에 대 한 프로세스를 반복 하는 사용자가 watchOS 응용 프로그램에서 테이블 뷰를 탐색 하는 표준 방법 하 고 y에 관심이에서 테이블 내에서:

[![](quick-interaction-techniques-images/quick04.png "세부 정보 보기 및 테이블 간 이동")](quick-interaction-techniques-images/quick04.png#lightbox)

새로운 3 watchOS 개발자 활성화 세로 페이징 해당 테이블 뷰 컨트롤에 있습니다. 이 기능을 사용 하기 전에 해당 세부 정보를 보려면 행을 누르고 테이블 뷰 행을 찾을 사용자 스크롤할 수 있습니다. 그러나 있습니다 수 이제 위쪽으로 살짝 아래로 선택 이전 행 또는 테이블에서 다음 행 선택 (또는 사용 디지털 왕관), 테이블 보기로 돌아가려면 필요 없이 먼저:

[![](quick-interaction-techniques-images/quick05.png "세부 정보 보기 및 테이블 간 이동 및 넘기기가 아래로 다른 행에는 사이 이동 하려면")](quick-interaction-techniques-images/quick05.png#lightbox)

이 모드를 사용 하려면 watchOS 응용 프로그램의 스토리 보드 Xcode에서 편집을 위해 테이블 뷰 찾아서 확인는 **세로 세부 페이징** 확인란을 선택 합니다.

[![](quick-interaction-techniques-images/quick06.png "세로 세부 페이징 확인란")](quick-interaction-techniques-images/quick06.png#lightbox)

테이블 자세히 보기에 표시 하 고 스토리 보드의 변경 내용을 저장, 동기화 하는 Mac에 대 한 Visual Studio로 되돌아가려면을 Segues 사용 중인지 확인 합니다.

개발자 수 프로그래밍 방식으로 참여 세로 페이징 표 보기에 대해 다음 코드를 사용 하 여 특정 행에:

```csharp
// Segue into Vertical Paging and select the first row
MenuTable.PerformSegue (0);
```

세로 페이징을 사용할 경우 개발자는 WatchKit는 자동으로 컨트롤러를 미리 로드를 처리 하며 결과적으로, 일부 컨트롤러 수명 주기 메서드를 호출할 수 UI 실제로 표시 되기 전에 해야 합니다.

### <a name="notification-enhancements"></a>알림 향상 된 기능

알림을 기본 형태의 사용자 watchOS에 일반적으로 발생 하는 빠른 상호 작용 하 고 첫 번째 Apple Watch 및 watchOS 1 이후 사용 되었기 합니다.

일반적인 알림 빠른 상호 작용은 다음과 같습니다.

1. 새 알림이 수신 되 면 사용자 알림 Haptic 느낍니다.
2. 자신의 손목을 짧은 조회 인터페이스 알림에 대 한 참조를 발생 시킵니다.
3. 확장 저장 프로시저가 계속 발생 하는 해당 손목 유지, watchOS 긴 조회 알림 인터페이스에 자동으로 전환 합니다.

사용자가 알림에 응답 하는 몇 가지 있습니다.

- 제대로 정의 하 고 알림 표시에 대 한 사용자는 아무 작업도 수행 되 고 단순히 알림을 해제할 됩니다.
- 알림 watchOS 응용 프로그램을 실행 하는 눌러 수도 수 있습니다.
- 사용자는 알림을 지 원하는 사용자 지정 작업에 대 한 사용자 지정 작업 중 하나를 선택할 수 있습니다. 일 수 있습니다.
    - **전경 작업** -이 작업을 수행 하려면 응용 프로그램을 실행 합니다.
    - **평가 해야** -2 watchOS 방향의 iPhone에 항상 라우팅된 하지만 watchOS 3에서에서 watchApp 라우팅할 수 있습니다.

WatchOS 3의 새로운 기능:

* 알림 (iOS, watchOS, tvOS 및 macOS)는 모든 플랫폼에서 비슷한 API를 사용 합니다.
* Apple Watch 로컬 알림을 예약할 수 있습니다.
* Apple Watch 예약 된 경우 배경 알림이 응용 프로그램의 확장으로 라우팅됩니다.

#### <a name="notification-scheduling-and-delivery"></a>알림 일정 예약 및 배달

사용자의 iPhone에서 알림이 됩니다 앞으로 Apple Watch 다음과 같은 경우.

* IPhone의 화면 꺼져 있습니다.
* Apple Watch 착용 되 고 잠금 해제 되었습니다.

3 watchOS에서 로컬 알림을 Apple Watch 예약할 수 있습니다 하 고 시계에만 전달 됩니다. 사용자가 개발자가 응용 프로그램에서 필요로 하는 경우 해당 iPhone 알림을 예약할 수 있습니다.

Apple Watch는 알림을 iPhone 버전 모두에서 같은 알림 식별자를 포함 하 여 중복 된 알림을 시계에 표시 되지 않도록 방지 합니다. Apple Watch 버전 알림 iPhone 버전 보다 우선 합니다.

WatchOS 3을 사용 하는 동일 하므로 `UINotification` 10, iOS 프레임 워크 API를 참조 하십시오. iOS 10 [사용자 알림 프레임 워크](~/ios/platform/user-notifications/index.md) 자세한 세부 정보에 대 한 설명서입니다.

### <a name="using-spritekit-and-scenekit"></a>SpriteKit 및 SceneKit 사용 하 여

새로운 watchOS 3에 개발자는 이제 사용할 SpritKit 및 SceneKit 개체를 모두 응용 프로그램의 사용자 인터페이스 디자인 모두 2D 및 3D 그래픽을 표시 하 합니다.

이 기능을 지 원하는 새로운 인터페이스 클래스 두 개 추가 되었습니다.

- `WKInterfaceSKScene` -에 대 한 SpriteKit 2 차원 그래픽을 사용 합니다.
- `WKInterfaceSCNScene` -에 대 한 SceneKit 3D 그래픽 작업 합니다.

이러한 개체를 사용 하려면 단순히 Xcode의 인터페이스 작성기에서 스토리 보드 watch 앱 내에서 디자인 화면으로 끌어 및 사용 하 여는 **특성 검사기** 을 구성할 수 있습니다.

이 시점부터 작업 SpriteKit 또는 SceneKit 장면에 동일 하 게 작동 iOS 앱 내에서 마찬가지로 합니다. Watch 앱 제시 합니다는 `WKInterfaceSKScene` 중 하나를 호출 하 여는 `Present` 메서드. SceneKit를 설정 하기만 하면는 `Scene` 의 속성은 `WKInterfaceSCNScene` 개체입니다.

## <a name="actionable-complications"></a>실행 가능한 복잡성

2 watchOS Apple 타사 앱에 대 한 복잡성을 도입 했습니다. 3 watchOS에서 Apple 개발자 WatchKit Complication에 포함할 수 있는 능력을 확장 했습니다. 

또한, 더 많은 기본 제공 면 조사식에서 복잡성을 포함할 이제 수 및 기존 조사식 연결을 이미 지원 되는 복잡성 수 이제 포함 되어 더 많은 복잡 합니다.

또한 새는 사용자에 대 한 왼쪽 또는 오른쪽의 Apple Watch 설치한 조사식 면 모든 전환으로 살짝 신속 하 게에 있습니다. 새 갤러리를 사용 하 여 Apple Watch 도우미 iPhone 앱에서 사용자를 추가 및 새 조사식 면 및 포함할 수 있는 복잡성의 사용자 지정.

이러한 새 기능으로 인해 Apple 모든 응용 프로그램에 Apple Watch Complication 하나 이상 포함 해야 하 고 따라서 네이티브 Apple Watch 응용 프로그램의 모든 이제 관련 된 복잡 한를 제안 합니다.

복잡성 앱에 다음과 같은 기능을 제공합니다.

- 이들은 매우 동적인 watch 화면에 항상 있기 때문입니다.
- 복잡성은 watchOS로 자주 업데이트 됩니다. 사용자의 현재 표시 된 watch 화면에는 복잡 함 중 하나를 포함 하는 모든 앱에는 한 시간에 두 번 이상 업데이트 됩니다.
- 복잡 한 사용자의 현재 표시 된 watch 화면에 있는 모든 앱은 신속 하 게 시작 하면 응용 프로그램 및 응용 프로그램의 응답 속도도 메모리에 보관 됩니다.
- 복잡성 사용자 watchOS 응용 프로그램에서 특정 기능을 시작 하도록 쉽게 있습니다.

## <a name="glanceable-notification"></a>동적인 알림

Apple Watch 대 한 알림 이벤트 또는 들어오는 메시지 또는 워크 아웃 응용 프로그램에서 목표를 달성 같은 새 정보는 사용자에 게 알리는 신속 하 게 하는 뛰어난 사용자 지정 가능한 방법을 제공 합니다.

알림을 사용 하 여 중요 한 정보가 사용자에 게 신속 하 게 표시할 수 있습니다. 대부분의 경우 잘 설계 된 알림 사용자가 실제로 응용 프로그램을 실행 하는 필요성을 제거할 수 있습니다.

모든 알림 지금은 지원 watchOS 3, 새:

- SpriteKit
- SceneKit
- 인라인 비디오

## <a name="enhanced-ui-with-spritekit-and-scenekit"></a>SpriteKit 및 SceneKit 향상된 UI

일반적으로 개발자 SpriteKit과 SceneKit를 언급 하는 경우 게임 UI 생각할 수 있습니다. 그러나 SpriteKit와 SceneKit 비 게임 사용자 지정된 레이아웃, 콘텐츠를 포함 하는 Ui 및 WatchKit만 가능 하지 않을 애니메이션 만들기 위한 유용할 수 있습니다.

예를 들어 사진 공유 앱에서에서 사용자 알림 실제 이미지 및 사용자 환경을 강화 하는 사용자 지정 된 다른 정보와 함께 그림을 게시 하는 사용자를 포함 하 여 풍부한 사용자 환경을 제공 하도록 SpriteKit를 사용할 수 있습니다.

또한 응용 프로그램의 사용자 인터페이스 디자인 표준 WatchKit UI 요소와 SpriteKit와 SceneKit을 혼합할 수 있습니다.

## <a name="simple-navigation"></a>간단한 탐색

watchOS 3 표시 합니다. 개발자가 새 같은 watchOS 앱 내에서 탐색을 간소화할 수 있는 여러 가지 방법으로 [세로 페이징](#Vertical-Paging), [제스처 인식기 지원](#Gesture-Recognizer-Support) 및 [디지털 왕관 회전](#Digital-Crown-Rotation) 기능 위에 제시 합니다.

디지털 왕관 Apple Watch 고유 하며 탐색을 간소화 하기 위해 다양 한 방법으로 사용할 수 있습니다. 예를 들어 타이머 응용 프로그램 타이머를 사용할 수 있는 길이 통해 스크러빙 디지털 왕관을 사용할 수 있습니다.

사용자 지정 제스처 watch 앱와 상호 작용 하는 사용자에 대 한 새롭고 고유한 방식으로 제공할 수 있습니다 및 응용 프로그램 탐색을 간소화 하기 위해 사용할 수도 있습니다.

Apple의 모든 watchOS 쉽고 빠르게 watchOS 응용 프로그램 인터페이스를 사용 하는 풍부한 표시 하는 3에에서 추가 된 새 빠른 상호 작용 기능을 결합 하는 방법을 찾고 하는 것이 좋습니다.

## <a name="finishing-the-quick-interaction"></a>빠른 상호 작용을 마무리

잘 디자인 된 빠른 상호 작용 경험은 사용자에 게 부여 자신의 손목 삭제 (및 앱 분리)에 대 한 신뢰성은 제거가 완료 되 면 현재 상호 작용 합니다.

이 특히, 여기서 watch 앱 되었거나 모든 종류의 네트워크 연결을 수행 하는 도우미 iPhone 앱과 정보를 공유 하는 경우 문제는. 트랜잭션을 조치가 바람직하지 않습니다 빠른 상호 작용 하는 동안 발생 하는 동안 대기 표시기를 발생할 종종 있습니다. 다음 예제를 참조하세요.

[![](quick-interaction-techniques-images/quick07.png "네트워크 연결을 수행 하 고 해당 도우미 iPhone 앱과 정보를 공유 watch 앱의 다이어그램")](quick-interaction-techniques-images/quick07.png#lightbox)

1. 사용자가 시계에 구매 항목을 선택 합니다.
2. 구입 단추를 누릅니다.
3. 응용 프로그램 네트워크 트랜잭션을 시작 하 고 로드 표시기를 표시 합니다.
4. 얼마 후 트랜잭션이 완료 되 고 ऍ प 구매 conformation 합니다.
5. 사용자는 자신의 손목을 삭제 하 고 앱과 분리 합니다.

트랜잭션이 완료 될 때까지 사용자가 구매 단추를 누를 때부터 로드 표시기를 보면 해당 손목 발생 갖게 됩니다. 이 문제를 해결 하기 위해 Apple 개발자가 로드 표시기를 표시 하는 대신 사용자에 대 한 즉각적인 피드백을 표시 하도록 제안 합니다. 

Apple의 제안 된 모델을 사용 하는 같은 빠른 상호 작용 다시 살펴보겠습니다를 수행 합니다.

[![](quick-interaction-techniques-images/quick08.png "사과 제안 된 모델 다이어그램")](quick-interaction-techniques-images/quick08.png#lightbox)

1. 사용자가 시계에 구매 항목을 선택 합니다.
2. 구입 단추를 누릅니다.
3. 응용 프로그램 네트워크 트랜잭션을 시작 하 고 구매가 성공적으로 시작 한다는 메시지가 표시 됩니다.
4. 사용자는 자신의 손목을 삭제 하 고 앱과 분리 합니다.
5. 트랜잭션이는 얼마 후 성공적으로 완료 되 면 응용 프로그램 성공 구매 사용자에 게 알리는 게 로컬 알림을 표시 합니다.

이 시간으로 사용자가 구매 단추 확신을 자신의 손목을 삭제 하 고 빠른 상호 작용을이 시점에서 종료 수 있도록 구매가 시작 되었음을 알리는 메시지가 표시 됩니다. 나중의 성공 또는 실패에 사용자 알림을 트랜잭션의 내용 알림이 표시 됩니다. 이러한 방식으로 사용자가만와 상호 작용 응용 프로그램 프로세스는 "활성" 단계 중입니다.

네트워킹을 수행 하는 응용 프로그램에 대 한 배경을 사용할 수 있습니다 `NSURLSession` 다운로드 작업으로 네트워크 통신을 처리 하도록 합니다. 이렇게 하면 응용 프로그램에서 다운로드 한 정보를 처리 하는 백그라운드에서 해제 합니다. 백그라운드 처리를 필요로 하는 응용 프로그램에 대 한 백그라운드 작업 어설션을 사용 하 여 필요한 처리를 처리 합니다.

## <a name="quick-interaction-design-tips"></a>빠른 상호 작용 디자인 팁

빠른 상호 작용의 원하는 길이 2 초 때문 개발자의 디자인 프로세스의 시작 부분에서 응용 프로그램의 상호 작용 디자인 초점을 합니다. 여기서 이러한 상호 작용 (위에서 설명한 기술을 사용 하 여) 단순화 하 고 watchOS 3의 새로운 기능을 사용 하 여 응용 프로그램을 신속 하 고 응답 하도록 찾습니다.

Apple 다음을 제안 합니다.

- 응용 프로그램의 가장 사용 되는 기능을 앞으로 전환 하 여 빠른 상호 작용에 집중 합니다.
- 복잡성 및 사용자 알림을 사용 하 여 일반적인 기능 및 함수를 노출 합니다.
- SceneKit 및 SpriteKit 다양 하 고 동적인 사용자 인터페이스를 만듭니다.
- 가능 하면 항상 앱 내에서 탐색을 단순화 합니다.
- 대기, 자신의 손목을 삭제 하 고 응용 프로그램을 가능한 한 빨리 해제 하도록 허용 하는 사용자를 만들면 안 됩니다.

## <a name="summary"></a>요약

이 문서는 빠른 상호 작용 기술을 검사가 수행 Apple Apple Watch 대 한 Xamarin.iOS에 구현 하는 방법과 watchOS 3에 추가 되었습니다.

## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://developer.xamarin.com/samples/watchos/all/)
