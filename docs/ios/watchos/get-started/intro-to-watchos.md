---
title: WatchOS 소개
description: 이 문서는 watchOS 응용 프로그램 수명 주기, 사용자 인터페이스 형식, 화면 크기, 제한 사항 등을 설명 하는 개요를 제공 합니다.
ms.prod: xamarin
ms.assetid: 99c316d6-6707-40f6-bec9-801d05888759
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: ba5e7a24524f9371cbd810e18c11acc9e2e2a4cb
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055623"
---
# <a name="introduction-to-watchos"></a>WatchOS 소개

> [!NOTE]
> 체크 아웃 합니다 [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md) 최신 기능에 대 한 개요입니다.

## <a name="about-watchos"></a>WatchOS에 대 한

WatchOS 앱 솔루션에 3 프로젝트:

- **확장 보기** – watch 앱에 대 한 코드를 포함 하는 프로젝트입니다.
- **앱 보기** – 사용자 인터페이스 스토리 보드 및 리소스를 포함 합니다.
- **부모 앱 iOS** –이 앱은 일반 iPhone 앱. Watch 앱 및 확장은 사용자의 보기에는 배달에 대 한 iPhone 앱으로 제공 됩니다.

WatchOS 1 앱에서 iPhone에서 실행 되는 확장의 코드 – Apple Watch 효과적으로 외장 디스플레이입니다. watchOS 2 및 3 앱 Apple Watch 완전히 실행합니다. 이러한 차이 아래 다이어그램에 표시 됩니다.

[ ![](intro-to-watchos-images/arch-sml.png "WatchOS 1 및 watchOS 2 (이상) 간의 차이이 다이어그램에 표시 됩니다.")](intro-to-watchos-images/arch.png#lightbox)

WatchOS 버전을 대상으로 하는 것에 관계 없이 Solution Pad Mac 용 Visual Studio에서 완벽 한 솔루션을는 다음과 비슷합니다.

[![](intro-to-watchos-images/projectstructure-sml.png "Solution Pad")](intro-to-watchos-images/projectstructure.png#lightbox)

합니다 *부모 앱* 는 watchOS 솔루션은 일반 iOS 앱입니다. 표시 되는 솔루션의 프로젝트만 이것이 **휴대폰**합니다. 자습서, 관리 화면 및 중간 계층 필터링 cacheing 등이 앱에 대 한 사용 사례가 포함 됩니다. 하지만 설치 하지 않고 watch 앱/확장을 실행 하는 사용자에 대 한 가능 **적이** 부모 앱 난, 따라서 부모 앱이 일회 초기화 또는 관리에 대 한 실행 되도록 해야 하는 경우 해야 watch 프로그램 앱/사용자에 게 알리고 확장입니다.

부모 앱 watch 앱 및 확장을 제공, 하지만 다른 샌드박스에서 실행 됩니다.

WatchOS 1에서 정적 함수 또는 공유 앱 그룹을 통해 데이터를 공유할 수 있습니다 `WKInterfaceController.OpenParentApplication`을 트리거하는 합니다 `UIApplicationDelegate.HandleWatchKitExtensionRequest` 부모와 앱에서 메서드 `AppDelegate` (참조 [부모 앱을 사용 하 여 작업](~/ios/watchos/app-fundamentals/parent-app.md)).

조사식 Connectivity framework 부모 앱과 통신 하는 watchOS 2 이상에서 사용 하는 `WCSession` 클래스입니다.

## <a name="application-lifecycle"></a>응용 프로그램 수명 주기

조사식 확장의 서브 클래스에는 `WKInterfaceController` 각 스토리 보드 장면에 대 한 클래스를 만듭니다.

이러한 `WKInterfaceController` 클래스는 비슷합니다는 `UIViewController` iOS 프로그래밍에서 개체 있지만 동일한 수준의 보기에 액세스할 수 없습니다.
예를 들어 동적으로 컨트롤을 추가 하거나 UI를 재구성할 수 없습니다.
그러나 숨기기 및 컨트롤 표시를, 일부 컨트롤에서 해당 크기, 투명성 및 모양 옵션을 변경 합니다.

수명 주기를 `WKInterfaceController` 개체는 다음 호출을 포함 합니다.

- [절전 모드 해제](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.Awake/) :이 방법에서는 대부분의 초기화를 수행 해야 합니다.
- [WillActivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.WillActivate/) : Watch 앱 사용자에 게 표시 되기 바로 전에 호출 됩니다. 이 메서드를 사용 하 여 마지막 순간 초기화를 수행 하려면 애니메이션 등을 시작 합니다.
- 이 시점에서 Watch 앱 나타나고 확장을 입력 하 고 응용 프로그램 논리 당 Watch 앱의 표시를 업데이트 하는 사용자에 게 응답을 시작 합니다.
- [DidDeactivate](https://developer.xamarin.com/api/member/WatchKit.WKInterfaceController.DidDeactivate/) Watch 앱 후 사용자가 해제 된,이 메서드를 호출 합니다. 다음 시간까지 사용자 인터페이스 컨트롤을 수정할 수 없습니다이 메서드에서 반환 된 후 `WillActivate` 라고 합니다. IPhone에 대 한 연결이 끊어진 경우에이 메서드를 호출 합니다.
- 확장이 비활성화 된 후 프로그램에 액세스할 수 없는 합니다. 보류 중인 비동기 기능이 **것입니다** 호출할 수 있습니다. 조사식 키트 확장에는 백그라운드 처리 모드를 사용할 수 없습니다. 첫 번째 메서드 호출 수는 프로그램은 사용자가 다시 활성화 하는 경우 운영 체제에서 앱으로 끝나지 않았습니다 `WillActivate`합니다.

![](intro-to-watchos-images/wkinterfacecontrollerlifecycle.png "응용 프로그램 수명 주기 개요")

## <a name="types-of-user-interface"></a>사용자 인터페이스의 형식

Watch 앱을 사용 하 여 사용자가 상호 작용 하는 방법은 세 종류가 있습니다.
모든 사용자 지정 하위 클래스를 사용 하 여 프로그래밍할 `WKInterfaceController`이므로 수명 주기 앞에서 설명한 순서 보편적으로 적용 됩니다 (하위 클래스를 사용 하 여 알림을 프로그래밍할 `WKUserNotificationController`의 하위 클래스는 직접 `WKInterfaceController`):

### <a name="normal-interaction"></a>일반적인 상호 작용

Watch 앱/확장 상호 작용의 대부분의 하위 클래스를 사용 하 여 될 `WKInterfaceController` watch 앱의 백그라운드에서 일치 하도록 작성 하는 **Interface.storyboard**합니다. 자세히 설명 합니다 [설치](~/ios/watchos/get-started/installation.md) 하 고 [Getting Started](~/ios/watchos/get-started/index.md) 문서입니다.
다음 이미지의 일부를 보여 줍니다.는 [조사식 키트 카탈로그](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) 샘플의 스토리 보드입니다. 여기에 대해 설명 했습니다 각 장면에는 해당 사용자 지정 `WKInterfaceController` (`LabelDetailController`를 `ButtonDetailController`, `SwitchDetailController`등) 확장 프로젝트에서.

![](intro-to-watchos-images/scenes.png "기본 상호 작용 예제")

### <a name="notifications"></a>알림

[알림](~/ios/watchos/platform/notifications.md) Apple Watch 대 한 주요 사용 사례는 합니다. 로컬 및 원격 알림이 지원 됩니다. 알림을 통해 상호 작용 단기 및 장기 확인 이라는 두 단계로 발생 합니다.

짧은 같습니다 간단 하 게 표시 되 고 watch 앱 아이콘, 이름 및 제목 표시 (지정 된 대로 `WKInterfaceController.SetTitle`).

시스템 제공 결합 긴 찾습니다 **섀시** 영역 및 사용자 지정 스토리 보드 기반 콘텐츠를 사용 하 여 해제 단추입니다.

`WKUserNotificationInterfaceController` 확장 `WKInterfaceController` 메서드를 사용 하 여 `DidReceiveLocalNotification` 고 `DidReceiveRemoteNotification`입니다.
알림 이벤트에 반응 하는 이러한 메서드를 재정의 합니다.

알림 UI 디자인에 대 한 자세한 내용은 참조는 [Apple Watch 휴먼 인터페이스 지침](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/Notifications.html#//apple_ref/doc/uid/TP40014992-CH20-SW1)

![](intro-to-watchos-images/notifications.png "샘플 알림")

## <a name="screen-sizes"></a>화면 크기

Apple Watch 두 개의 얼굴 크기: (38mm 및 42 mm 모두를 5:4 표시 비율과 레 티 나 디스플레이 사용 하 여 합니다. 사용 가능한 크기는 다음과 같습니다.

- 38 mm: 136 x 170 논리 픽셀 (272 x 340 물리적 픽셀)
- 42 mm: 156 x 195 논리 픽셀 (312 x 390 물리적 픽셀)입니다.

사용 하 여 `WKInterfaceDevice.ScreenBounds` 어느 디스플레이의 Watch 앱이 실행 중인지 확인 하려면.

일반적으로 하기가 더 제한적인된 (38mm 표시를 사용 하 여 텍스트 및 레이아웃 디자인을 개발 하 고 다음를 확장 합니다.
대규모 환경의 시작 하기 까다로운 중복 또는 텍스트 잘림 축소 발생할 수 있습니다.

에 대해 자세히 알아보세요 [화면 크기를 사용 하 여 작업](~/ios/watchos/app-fundamentals/screen-sizes.md)합니다.


## <a name="limitations-of-watchos"></a>WatchOS의 제한 사항

WatchOS watchOS 앱을 개발할 때 알아야 할 몇 가지 제한이 있습니다.

- Apple Watch 장치 저장소가 제한 된-(예: 큰 파일을 다운로드 하기 전에 사용 가능한 공간에 주의 오디오 또는 동영상 파일).

- 여러 watchOS [컨트롤](~/ios/watchos/user-interface/index.md) UIKit에서 유사한 기능을 갖지만 다른 클래스 (`WKInterfaceButton` 대신 `UIButton`에 `WKInterfaceSwitch` 에 대 한 `UISwitch`등) 있고 해당 UIKit 비교할 메서드의 제한 된 집합 해당 하는 합니다. 또한 watchOS에 일부 컨트롤 같은 `WKInterfaceDate` (날짜 및 시간을 표시)는 UIKit 없는 합니다.

  - 시계만 알림을 또는 (라우팅을 통해 사용자가 컨트롤의 종류에 공지 되지 Apple에서)만 iPhone을 라우팅할 수 없습니다.

일부 다른 알려진된 제한 사항 / 질문과 대답:

- Apple은 타사 사용자 지정 조사 얼굴을 허용 하지 않습니다.

- 연결 된 휴대폰에서 iTunes를 제어 하려면 시계를 허용 하는 Api는 private입니다.


## <a name="further-reading"></a>추가 정보

Apple의 설명서를 확인해 보세요.

* [조사식 키트에 대 한 개발](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html#//apple_ref/doc/uid/TP40014969-CH8-SW1)

* [키트 프로그래밍 가이드를 시청 하세요.](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/DesigningaWatchKitApp.html)

* [Apple Watch 휴먼 인터페이스 지침](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/index.html#//apple_ref/doc/uid/TP40014992-CH3-SW1)


## <a name="related-links"></a>관련 링크

- [watchOS 3 카탈로그 (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [watchOS 1 카탈로그 (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [설정 및 설치](~/ios/watchos/get-started/installation.md)
- [첫 번째 Watch 앱 비디오](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple는 Watch 키트 설명서에 대 한 개발의](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/index.html)
- [Apple의 WatchKit 팁](https://developer.apple.com/watchkit/tips/)
- [watchOS 3 소개](~/ios/watchos/platform/introduction-to-watchos3/index.md)
