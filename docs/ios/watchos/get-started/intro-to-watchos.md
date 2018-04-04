---
title: WatchOS 소개
description: WatchOS 솔루션 구조 및 제한 사항 개요
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 0a0adbab54fc134eaf2e69cc8088713e54b15d3b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-watchos"></a>WatchOS 소개

> [!NOTE]
> 체크 아웃 된 [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md) , 그러면 최신 기능에 대 한 개요입니다.

## <a name="about-watchos"></a>WatchOS에 대 한

WatchOS 앱 솔루션에 프로젝트 3에 있습니다.

- **확장을 시청** – watch 앱에 대 한 코드를 포함 하는 프로젝트입니다.
- **응용 프로그램을 시청** – 사용자 인터페이스 스토리 보드 및 리소스가 포함 되어 있습니다.
- **iOS 앱 부모** –이 앱은 일반 iPhone 앱. Watch 앱 및 확장은 사용자의 조사식에 배달할 수는 iPhone 응용 프로그램에 제공 됩니다.

WatchOS 1 응용 프로그램에서 확장에 코드가 iPhone에서 실행-Apple Watch 결과적으로 외부 디스플레이 합니다. watchOS 2와 3 단계 앱 Apple Watch 완전히 실행 합니다. 이러한 차이 아래 다이어그램에 표시 됩니다.

[ ![](intro-to-watchos-images/arch-sml.png "이 다이어그램에 나와 watchOS 1과 2 (및 큰) watchOS의 차이점")](intro-to-watchos-images/arch.png#lightbox)

WatchOS의 버전, 대상에 관계 없이 Mac의 솔루션 패드에 대 한 Visual Studio에서 완벽 한 솔루션은 다음과 같이 표시:

[![](intro-to-watchos-images/projectstructure-sml.png "솔루션 채움")](intro-to-watchos-images/projectstructure.png#lightbox)

*부모 앱* 는 watchOS 솔루션은 일반 iOS 앱. 표시 되는 솔루션의 프로젝트만 이것이 **통화**합니다. 이 앱에 대 한 사용 사례는 자습서, 관리 화면 및 중간 계층 필터링 cacheing 등 포함 됩니다. 그러나 사용자가 설치 하 고 실행 하지 않고 watch 앱/확장은 **적이** 부모 앱을 열고 난, 따라서 부모 응용 프로그램이 한 번만 초기화 또는 관리에 대 한 실행 되도록 해야 할 경우 해야 시계로 프로그래밍 사용자에 게 알리고 앱/확장입니다.

Watch 앱 및 확장을 제공 하는 부모 응용 프로그램, 하지만 다른 샌드박스에서 실행 합니다.

WatchOS 1에 공유 응용 프로그램 그룹 또는 정적 함수를 통해 데이터를 공유할 수 있습니다 `WKInterfaceController.OpenParentApplication`을 트리거하는 `UIApplicationDelegate.HandleWatchKitExtensionRequest` 부모 응용 프로그램에서 메서드 `AppDelegate` (참조 [부모 응용 프로그램 작업](~/ios/watchos/app-fundamentals/parent-app.md)).

조사식 연결 프레임 워크를 부모 응용 프로그램와 통신 사용 watchOS 2 이상에서 사용 하는 `WCSession` 클래스입니다.

## <a name="application-lifecycle"></a>응용 프로그램 수명 주기

조사식 확장에서의 서브 클래스는 `WKInterfaceController` 각 스토리 보드 장면에 대 한 클래스가 만들어집니다.

이러한 `WKInterfaceController` 클래스는 비슷합니다는 `UIViewController` iOS 프로그래밍에서 개체 있지만 동일한 수준의 보기에 대 한 액세스를 하지 않은 합니다.
예를 들어, 동적으로 컨트롤을 추가할 수 없거나 UI를 재구성 합니다.
그러나 숨기기 및 컨트롤을 표시 하 고, 일부 컨트롤에서 변경할 수의 크기, 투명도 및 모양 옵션입니다.

수명 주기는 `WKInterfaceController` 개체는 다음 호출 포함 됩니다.

- [절전 모드 해제](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) :이 방법에서는 대부분의 프로그램 초기화를 수행 해야 합니다.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : Watch 앱 사용자에 게 표시 되기 바로 전에 호출 됩니다. 이 메서드를 사용 하 여 마지막 순간 초기화를 수행 하 고 등 애니메이션을 시작 합니다.
- 이 시점에서 Watch 앱 나타나고 확장 입력 하 고 응용 프로그램 논리 당 Watch 앱의 표시를 업데이트 하는 사용자에 게 응답을 시작 합니다.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) Watch 앱 후 사용자가 해제 된,이 메서드를 호출 합니다. 이 메서드가 반환 된 후 다음 때까지 사용자 인터페이스 컨트롤을 수정할 수 없습니다 `WillActivate` 호출 됩니다. IPhone에 대 한 연결이 끊어진 경우에이 메서드를 호출 합니다.
- 확장 비활성화 된 후에 프로그램에 액세스할 수 없습니다. 보류 중인 비동기 기능이 **는 트리거하지** 호출할 수 있습니다. 조사식 키트 확장에는 백그라운드 처리 모드를 사용할 수 없습니다. 첫 번째 메서드 호출 될 사용자가 프로그램을 다시 활성화 하는 경우 운영 체제에서 응용 프로그램 종료 되지 않았습니다 `WillActivate`합니다.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "응용 프로그램 수명 주기 개요")

## <a name="types-of-user-interface"></a>사용자 인터페이스의 형식

세 가지 방법으로 상호 작용 사용자 조사식 응용 프로그램과 함께 있을 수 있습니다.
사용자 지정 하위 클래스를 사용 하 여 프로그래밍 하는 데는 모든 `WKInterfaceController`, 위에 설명 된 수명 주기 시퀀스 전체적으로 적용 됩니다 (의 하위 클래스와 함께 알림 되며 프로그래밍 `WKUserNotificationController`, 논리는의 하위 클래스 `WKInterfaceController`):

### <a name="normal-interaction"></a>일반적인 상호 작용

하위 클래스와 대부분의 앱/확장 상호 작용 조사식 됩니다 `WKInterfaceController` 장면 조사식 응용 프로그램에 맞게 작성 하는 **Interface.storyboard**합니다. 이 내용은에서 자세히 설명 된 [설치](~/ios/watchos/get-started/installation.md) 및 [시작](~/ios/watchos/get-started/index.md) 문서입니다.
다음 그림의 일부를 보여 줍니다.는 [조사식 키트 카탈로그](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) 샘플의 스토리 보드 합니다. 여기 표시는 각 장면에는 해당 사용자 지정 `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`등) 확장 프로젝트에서.

![](intro-to-watchos-images/scenes.png "기본 상호 작용 예제")

### <a name="notifications"></a>알림

[알림](~/ios/watchos/platform/notifications.md) Apple Watch 대 한 주요 사용 사례 됩니다. 로컬 및 원격 알림은 사용할 수 있습니다. 알림와의 상호 작용을 단기 및 장기 모양을 호출 하는 두 단계로 발생 합니다.

짧은 찾습니다 간단 하 게 표시 되 고 조사식 앱 아이콘, 이름 및 제목 표시 (으로 지정 된 대로 `WKInterfaceController.SetTitle`).

시스템 제공 결합 하 여 긴 조회 **섀시** 영역 및 사용자 지정 스토리 보드 기반 콘텐츠로 Dismiss 단추입니다.

`WKUserNotificationInterfaceController` 확장 `WKInterfaceController` 방법을 사용 하 여 `DidReceiveLocalNotification` 및 `DidReceiveRemoteNotification`합니다.
알림 이벤트에 반응 하 이러한 메서드를 재정의 합니다.

알림 UI 디자인에 대 한 자세한 내용은 참조는 [Apple Watch Human Interface Guidelines](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "샘플 알림")

## <a name="screen-sizes"></a>한 화면 크기

Apple Watch 두 개의 글꼴 크기: 38 mm 및 42 mm, 모두는 5:4 표시 비율과 레 티 나 디스플레이 사용 합니다. 사용 가능한 크기는:

- 38 mm: 136 x 170 논리 픽셀 (272 x 340 물리적 픽셀)
- 42 mm: 156 x 195 논리 픽셀 (312 x 390 물리적 픽셀)입니다.

사용 하 여 `WKInterfaceDevice.ScreenBounds` 어느 디스플레이에 Watch 앱이 실행 중인지 확인 하려면.

일반적으로 더 제한적인된 38 mm 표시와 함께 텍스트 및 레이아웃 디자인을 개발 하 고 다음 확장을 쉽습니다.
더 큰 환경을 시작 하는 경우 까다로운 겹쳐진 또는 텍스트 잘림이 발생할 수 있습니다 축소.

에 대해 자세히 알아보세요 [화면 크기와 작업](~/ios/watchos/app-fundamentals/screen-sizes.md)합니다.


## <a name="limitations-of-watchos"></a>WatchOS의 제한 사항

watchOS watchOS 앱을 개발 하는 경우 고려해 야 할 몇 가지 제한 사항이 있습니다.

- Apple Watch 장치 수가 저장소가 제한-않음 (예: 큰 파일을 다운로드 하기 전에 사용 가능한 공간에 주의 오디오 또는 동영상 파일).

- 많은 watchOS [컨트롤](~/ios/watchos/user-interface/index.md) UIKit의 유사한 점은 있지만 다양 한 클래스 (`WKInterfaceButton` 대신 `UIButton`, `WKInterfaceSwitch` 에 대 한 `UISwitch`등) 있고 해당 UIKit에 비해 메서드의 제한 된 집합 해당 하는 합니다. 또한 watchOS에 일부 컨트롤 같은 `WKInterfaceDate` (날짜 및 시간 표시)에 해당 UIKit가 없습니다.

  - 만 시계에 알림 또는 (어떤 유형의 라우팅을 통해 사용자가 제어 하지 발표 Apple에 의해)만 iPhone을 라우팅할 수 없습니다.

몇 가지 다른 알려진된 제한 사항이 자주 묻는 질문 /:

- Apple에서는 타사 사용자 지정 조사식을 만들 수 없습니다.

- 연결 된 휴대폰에서 iTunes를 제어 하려면 시계를 허용 하는 Api는 private입니다.


## <a name="further-reading"></a>추가 정보

Apple에서 제공한 설명서를 확인해 보십시오.

* [조사식 키트에 대 한 개발](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [감시 키트 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Apple Watch 휴먼 인터페이스 지침](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>관련 링크

- [3 watchOS 카탈로그 (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [1 watchOS 카탈로그 (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [설치 및 설정](~/ios/watchos/get-started/installation.md)
- [첫 번째 Watch 앱 비디오](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple는 Watch 키트 가이드에 대 한 개발의](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple의 WatchKit 팁](https://developer.apple.com/watchkit/tips/)
- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
