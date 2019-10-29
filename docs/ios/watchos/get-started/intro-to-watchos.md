---
title: watchOS 소개
description: 이 문서에서는 응용 프로그램 수명 주기, 사용자 인터페이스 유형, 화면 크기, 제한 사항 등에 대해 설명 하는 watchOS의 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: b3c2908d8ae9a68189fbff4d47afa49da21b88a5
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030187"
---
# <a name="introduction-to-watchos"></a>watchOS 소개

> [!NOTE]
> 최신 기능에 대 한 개요는 [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md) 를 확인 하세요.

## <a name="about-watchos"></a>WatchOS 정보

WatchOS app 솔루션에는 3 개의 프로젝트가 있습니다.

- **조사식 확장** – watch 앱에 대 한 코드를 포함 하는 프로젝트입니다.
- **앱 보기** – 사용자 인터페이스 storyboard 및 리소스를 포함 합니다.
- **IOS 부모 앱** -이 앱은 일반적인 iPhone 앱입니다. Watch 앱 및 확장은 사용자의 감시에 배달 하기 위해 iPhone 앱에 번들로 제공 됩니다.

WatchOS 1 앱에서 확장 프로그램의 코드는 iPhone에서 실행 되며 Apple Watch은 효과적으로 외부 디스플레이입니다. watchOS 2 및 3 앱은 전적으로 Apple Watch에서 실행 됩니다. 이러한 차이점은 아래 다이어그램에 나와 있습니다.

[![](intro-to-watchos-images/arch-sml.png "The difference between watchOS 1 and watchOS 2 (and greater) is shown in this diagram")](intro-to-watchos-images/arch.png#lightbox)

WatchOS의 Mac용 Visual Studio 대상 버전에 관계 없이 전체 솔루션은 다음과 같이 표시 됩니다 Solution Pad.

[![](intro-to-watchos-images/projectstructure-sml.png "The Solution Pad")](intro-to-watchos-images/projectstructure.png#lightbox)

WatchOS 솔루션의 *부모 앱* 은 일반적인 iOS 앱입니다. **휴대폰에**표시 되는 솔루션의 유일한 프로젝트입니다. 이 앱에 대 한 사용 사례에는 자습서, 관리 화면, 중간 계층 필터링, cacheing 등이 포함 됩니다. 그러나 사용자는 부모 앱을 **열지 않고 시청** 앱/확장을 설치 및 실행할 수 있으므로, 일회성 초기화 또는 관리를 위해 부모 앱을 실행 해야 하는 경우에는 감시 앱/확장 프로그램을 프로그래밍 하 여 확인 해야 합니다. 사용자입니다.

부모 앱은 조사식 앱 및 확장을 제공 하지만 다른 샌드박스에서 실행 됩니다.

WatchOS 1에서 공유 앱 그룹 또는 정적 함수 `WKInterfaceController.OpenParentApplication`를 통해 데이터를 공유할 수 있습니다. 그러면 부모 앱의 `AppDelegate`에서 `UIApplicationDelegate.HandleWatchKitExtensionRequest` 메서드를 트리거합니다 ( [부모 앱 작업](~/ios/watchos/app-fundamentals/parent-app.md)참조).

WatchOS 2 이상에서, Watch 연결 프레임 워크는 `WCSession` 클래스를 사용 하 여 부모 앱과 통신 하는 데 사용 됩니다.

## <a name="application-lifecycle"></a>애플리케이션 수명 주기

조사식 확장에서 각 스토리 보드 장면에 대해 `WKInterfaceController` 클래스의 서브 클래스가 생성 됩니다.

이러한 `WKInterfaceController` 클래스는 iOS 프로그래밍의 `UIViewController` 개체와 유사 하지만 보기에 대 한 액세스 수준이 동일 하지 않습니다.
예를 들어, UI에 동적으로 컨트롤을 추가 하거나 구조를 재구성할 수 없습니다.
그러나 컨트롤을 숨기 거 나 표시 하 고, 일부 컨트롤에서 크기, 투명도 및 모양 옵션을 변경할 수 있습니다.

`WKInterfaceController` 개체의 수명 주기는 다음 호출을 포함 합니다.

- [활성](xref:WatchKit.WKInterfaceController.Awake*) :이 메서드에서 대부분의 초기화를 수행 해야 합니다.
- [WillActivate](xref:WatchKit.WKInterfaceController.WillActivate) : Watch 앱이 사용자에 게 표시 되기 직전에 호출 됩니다. 이 메서드를 사용 하 여 마지막 초기화, 시작 애니메이션 등을 수행할 수 있습니다.
- 이 시점에서 조사식 앱이 나타나고 확장이 사용자 입력에 응답 하기 시작 하 고 응용 프로그램 논리에 따라 Watch 앱의 표시를 업데이트 합니다.
- [DidDeactivate](xref:WatchKit.WKInterfaceController.DidDeactivate) 사용자가 감시 앱을 해제 한 후에는이 메서드가 호출 됩니다. 이 메서드가 반환 된 후에는 다음에 `WillActivate`를 호출할 때까지 사용자 인터페이스 컨트롤을 수정할 수 없습니다. IPhone에 대 한 연결이 끊어진 경우에도이 메서드가 호출 됩니다.
- 확장을 비활성화 한 후에는 프로그램에 액세스할 수 없습니다. 보류 중인 비동기 함수 **는 호출 되지** 않습니다. Watch Kit 확장은 백그라운드 처리 모드를 사용할 수 없습니다. 프로그램이 사용자에 의해 다시 활성화 되었지만 운영 체제에서 앱을 종료 하지 않은 경우에는 라는 첫 번째 메서드가 `WillActivate`됩니다.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "Application Lifecycle overview")

## <a name="types-of-user-interface"></a>사용자 인터페이스 형식

사용자가 시청 앱에 사용할 수 있는 상호 작용에는 세 가지 유형이 있습니다.
All은 `WKInterfaceController`의 사용자 지정 하위 클래스를 사용 하 여 프로그래밍 되므로 앞에서 설명한 수명 주기 시퀀스는 전체적으로 적용 됩니다. 알림은 `WKUserNotificationController`의 하위 클래스를 사용 하 여 프로그래밍 되므로 자체는 `WKInterfaceController`의 하위 클래스입니다.

### <a name="normal-interaction"></a>정상적인 상호 작용

대부분의 조사식 앱/확장 상호 작용은 조사식 응용 프로그램의 **인터페이스. storyboard**에서 장면에 해당 하는 사용자가 작성 하는 `WKInterfaceController`의 하위 클래스와 함께 사용 됩니다. 이 내용은 [설치](~/ios/watchos/get-started/installation.md) 및 [시작](~/ios/watchos/get-started/index.md) 문서에 자세히 설명 되어 있습니다.
다음 이미지는 [감시 키트 카탈로그](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 샘플의 스토리 보드 중 일부를 보여줍니다. 여기에 표시 된 각 장면에는 확장 프로젝트에 해당 하는 사용자 지정 `WKInterfaceController` (`LabelDetailController`, `ButtonDetailController`, `SwitchDetailController`등)가 있습니다.

![](intro-to-watchos-images/scenes.png "Normal Interaction examples")

### <a name="notifications"></a>알림

[알림은](~/ios/watchos/platform/notifications.md) Apple Watch의 주요 사용 사례입니다. 로컬 알림과 원격 알림이 모두 지원 됩니다. 알림과의 상호 작용은 단기 및 긴 모양 이라는 두 단계로 이루어집니다.

간략 한 모양은 잠깐 표시 되 고 보기 앱 아이콘, 해당 이름 및 제목을 표시 합니다 (`WKInterfaceController.SetTitle`지정).

긴 모양은 시스템에서 제공 하는 **섀시** 영역 및 해제 단추를 사용자 지정 스토리 보드 기반 콘텐츠와 결합 합니다.

`WKUserNotificationInterfaceController` `DidReceiveLocalNotification` 및 `DidReceiveRemoteNotification`메서드를 사용 하 여 `WKInterfaceController`를 확장 합니다.
알림 이벤트에 반응 하려면 이러한 메서드를 재정의 합니다.

알림 UI 디자인에 대 한 자세한 내용은 [Apple Watch 휴먼 인터페이스 지침](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1) 을 참조 하세요.

![](intro-to-watchos-images/notifications.png "Sample notifications")

## <a name="screen-sizes"></a>화면 크기

이 Apple Watch에는 38mm 및 42mm의 두 가지 크기 (5:4 표시 비율과 레 티 나 표시)가 있습니다. 사용할 수 있는 크기는 다음과 같습니다.

- 38mm: 136 x 170 논리 픽셀 (272 x 340 실제 픽셀)
- 42mm: 156 x 195 논리 픽셀 (312 x 390 실제 픽셀)

`WKInterfaceDevice.ScreenBounds`를 사용 하 여 시청 앱이 실행 되 고 있는 표시를 확인 합니다.

일반적으로 더 제한적인 38mm 표시를 사용 하 여 텍스트 및 레이아웃 디자인을 개발 하 고 확장 하는 것이 더 쉽습니다.
더 큰 환경에서 시작 하는 경우 축소를 축소 하면 작은 겹침 또는 텍스트 잘림이 발생할 수 있습니다.

[화면 크기 작업](~/ios/watchos/app-fundamentals/screen-sizes.md)에 대해 자세히 알아보세요.

## <a name="limitations-of-watchos"></a>WatchOS의 제한 사항

WatchOS apps를 개발 하는 경우 watchOS에 대 한 몇 가지 제한 사항이 있습니다.

- Apple Watch 장치는 저장소 공간이 제한 되어 있습니다. 예를 들어, 용량이 많은 파일을 다운로드 하기 전에 오디오 또는 동영상 파일).

- 수많은 watchOS [컨트롤](~/ios/watchos/user-interface/index.md) 은 uikit에 analogues 있지만 다른 클래스 (`UIButton`, `UISwitch``WKInterfaceSwitch`에 대 한이 아닌`WKInterfaceButton`)와 해당 uikit와 비교 하 여 제한 된 일련의 메서드를 포함 합니다. 또한 watchOS에는 UIKit에 포함 되지 않은 `WKInterfaceDate` (날짜 및 시간을 표시 하는) 등의 몇 가지 컨트롤이 있습니다.

  - 알림도 보기만 또는 iPhone 으로만 라우팅할 수 없습니다 (사용자가 라우팅을 통한 제어의 종류는 Apple에서 발표 하지 않음).

기타 알려진 몇 가지 제한 사항/질문과 대답:

- Apple에서는 타사 사용자 지정 조사식 얼굴을 허용 하지 않습니다.

- 시계를 연결 된 휴대폰에서 제어 하는 데 사용할 수 있는 Api는 비공개입니다.

## <a name="further-reading"></a>추가 정보

Apple에서 설명서를 확인 하세요.

- [Watch Kit 개발](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

- [시청 키트 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

- [Apple Watch 휴먼 인터페이스 지침](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)

## <a name="related-links"></a>관련 링크

- [watchOS 3 카탈로그 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [watchOS 1 카탈로그 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [설정 및 설치](~/ios/watchos/get-started/installation.md)
- [첫 번째 시청 앱 비디오](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple의 감시 키트 개발 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple의 WatchKit 팁](https://developer.apple.com/watchkit/tips/)
- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
