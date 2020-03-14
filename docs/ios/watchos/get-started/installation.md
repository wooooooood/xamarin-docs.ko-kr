---
title: 설치 및 watchOS에서 Xamarin 사용
description: 이 문서에는 설치 및 Xamarin을 사용 하 여 watchOS를 사용 하는 방법을 설명 합니다. 설치, watchOS 프로젝트에 설명 구조체 Xcode 통합을 iOS 디자이너를 사용 하는 방법 및 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/05/2017
ms.openlocfilehash: f986099011dbccb0eb43c62d253ee497d46ca08e
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306202"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>설치 및 watchOS에서 Xamarin 사용

watchOS 4에는 macOS Sierra (10.12) Xcode 9가 함께 필요합니다.

watchOS 1에는 원래 Xcode 7을 사용 하 여 OS X Yosemite (10.10) 필요합니다.

> [!WARNING]
> [2018 년 4 월 1 일 이후에는 watchOS 1 업데이트가 허용 되지](https://developer.apple.com/news/?id=11162017a)않습니다. 향후 업데이트에서 watchOS 2를 사용 해야 SDK 이상; 빌드는 watchos 4 SDK 것이 좋습니다.

## <a name="project-structure"></a>프로젝트 구조

Watch 앱의 세 프로젝트로 구성 됩니다.

- **Xamarin.ios iphone 앱 프로젝트** -이 프로젝트는 일반적인 iPhone 프로젝트 이며, xamarin.ios 템플릿 중 하나일 수 있습니다. Watch 앱 및 해당 확장이 기본 프로젝트 내에서 함께 제공 됩니다.

- 감시 앱에 대 한 코드 (예: 컨트롤러 클래스)가 포함 된 **조사식 확장 프로젝트** 입니다.

- **Watch 앱 프로젝트** -여기에는 watch 앱에 대 한 모든 UI 리소스를 포함 하는 사용자 인터페이스 스토리 보드 파일이 포함 됩니다.

[시청 키트 카탈로그 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 솔루션은 xamarin.ios에서 다음과 같이 표시 됩니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![](installation-images/catalog-solution.png "The solution in Visual Studio")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![](installation-images/catalog-solution-vs.png "The solution in Visual Studio")

-----

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 샘플을 다운로드 하 고 실행 하 여 시작 하세요.
샘플의 화면은 [컨트롤](~/ios/watchos/user-interface/index.md) 페이지에서 찾을 수 있습니다.

## <a name="creating-a-new-project"></a>새 프로젝트 만들기

수 없습니다. 솔루션을 만들 새 "Watch"... 대신 Watch 앱을 기존 iOS 응용 프로그램에 추가할 수 있습니다. Watch 앱을 만들려면 다음이 단계를 수행 합니다.

1. 기존 프로젝트가 없는 경우 먼저 **파일 > 새 솔루션** 을 선택 하 고 iOS 앱을 만듭니다 (예: **단일 뷰 앱**).

    [![](installation-images/cycle8-2-sml.png "Choose File > New Solution and create an iOS app")](installation-images/cycle8-2.png#lightbox)

2. IOS 앱이 생성 되 면 (또는 기존 iOS 앱을 사용 하려는 경우) 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 프로젝트 추가**...를 선택 합니다. **새 프로젝트** 창에서 **watchOS > App > WatchKit app**을 선택 합니다.

    [![](installation-images/cycle8-6-sml.png "Select watchOS > App > WatchKit App")](installation-images/cycle8-6.png#lightbox)

3. 다음 화면에는 iOS 앱 프로젝트는 watch 앱을 포함할지 선택할 수 있습니다.

    [![](installation-images/cycle8-7-sml.png "Choose which iOS app project should include the watch app")](installation-images/cycle8-7.png#lightbox)

4. 마지막으로 프로젝트를 저장할 위치를 선택 (및 필요에 따라 소스 제어를 사용 하도록 설정):

    [![](installation-images/cycle8-8-sml.png "Choose the location to save the project")](installation-images/cycle8-8.png#lightbox)

5. Mac용 Visual Studio에서 자동으로 [프로젝트 참조 및 **info.plist** 설정을](~/ios/watchos/get-started/project-references.md) 구성 합니다.

## <a name="creating-the-watch-user-interface"></a>보기 사용자 인터페이스 만들기

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS 디자이너 사용

IOS Designer를 사용 하 여 편집 하려면 조사식 앱의 **인터페이스. 스토리 보드** 를 두 번 클릭 합니다. **도구 상자** 에서 인터페이스 컨트롤러 및 UI 컨트롤을 storyboard로 끌어와 **속성** 패드를 사용 하 여 구성할 수 있습니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

[![](installation-images/iosdesigner-sml.png "The storyboard in the Designer")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![](installation-images/iosdesigner-sml-vs.png "The storyboard in the Designer")](installation-images/iosdesigner-vs.png#lightbox)

-----

새 인터페이스 컨트롤러를 선택한 다음 **속성** 패드에 이름을 입력 하 여 각 인터페이스 컨트롤러에 C# **클래스** 를 제공 해야 합니다. 이렇게 하면 필요한 코드 숨김 파일이 자동으로 생성 됩니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

![](installation-images/iosdesigner-classname.png "Give each new interface controller a Class")

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![](installation-images/iosdesigner-classname-vs.png "Give each new interface controller a Class")

-----

단추, 테이블 또는 인터페이스 컨트롤러에서 다른 인터페이스 컨트롤러로 segue을 **Ctrl + 끌어서** 만듭니다.

### <a name="using-xcode-on-the-mac"></a>Mac에서 Xcode를 사용 하 여

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

Xcode를 사용 하 여 인터페이스를 마우스 오른쪽 단추로 클릭 하 고 **> Xcode Interface Builder**를 사용 하 여 열기를 선택 하 여 사용자 인터페이스를 빌드할 수 있습니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio 사용자에 게 전환 Mac 빌드 호스트를 직접 사용 하 여 해당 사용자 인터페이스를 만드는 Xcode를 이용할 수 있습니다.
Mac용 Visual Studio에서 솔루션을 열고 Xcode를 마우스 오른쪽 단추로 클릭 한 다음 **> Interface Builder를 사용 하 여 열기**를 선택 합니다.

-----

![](installation-images/openwith-xcode.png "Open the Interface.storyboard in Xcode Interface Builder")

Xcode를 사용 하는 경우 일반 [iOS 앱 storyboard](~/ios/user-interface/storyboards/index.md) 와 동일한 단계를 수행 해야 합니다 (예: **Ctrl + Ctrl + 마우스** 를 사용 하 여 **.h** 헤더 파일에 끌어서 작업 만들기).

Xcode에 스토리 보드를 저장 하면 사용자가 만든 C# Interface Builder 작업은 조사식 확장 프로젝트의 **designer.cs** 파일에 자동으로 추가 됩니다.

### <a name="adding-additional-screens-in-xcode"></a>Xcode에서 추가 화면을 추가합니다.

Xcode Interface Builder를 사용 하 여 스토리 보드에 추가 화면 (기본적으로 템플릿에 포함 된 것 외)을 스토리 보드에 추가 하 **는 C#**  경우 새 각 인터페이스 컨트롤러에 대 한 코드 파일을 수동으로 추가 해야 합니다.

[스토리 보드에 새 인터페이스 컨트롤러를 추가 하는 방법에 대 한 고급 지침](~/ios/watchos/troubleshooting.md#add)을 참조 하세요.

*Xamarin iOS 디자이너는이 작업을 자동으로 수행 하므로 수동 단계가 필요 하지 않습니다.*

## <a name="building"></a>빌딩

다른 iOS 프로젝트와 같은 watch 앱을 포함 하는 프로젝트를 빌드합니다. 빌드 프로세스에서 코드를 사용 하지 않는 watch 응용 프로그램 (.app)를 포함 하는 watch 확장 (.appex)를 포함 하는 iPhone 응용 프로그램 (.app) 발생 합니다.

## <a name="launching"></a>시작

Studio (Mac 빌드 호스트에서 시작) Visual 또는 Mac 용 Visual Studio를 사용 하 여 시뮬레이터에서 watch 앱을 시작할 수 있습니다.

WatchKit 앱을 시작 하는 것에 대 한 두 가지 모드를 가지 있습니다.

- 일반 앱 모드 (기본값) 및
- [알림](~/ios/watchos/platform/notifications.md) (JSON 형식의 테스트 알림 페이로드가 필요 함).

### <a name="xcode-8-support"></a>Xcode 8 지원

Xcode 8 (이상)이 설치 되 고 나면 Apple Watch 시뮬레이터은 iOS 시뮬레이터와는 별개입니다. 여기서 [Xcode 6](#xcode6)은 *외부 디스플레이로*나타났습니다.
Watch 앱 프로젝트를 선택 하 고 시작 프로젝트로 만들면 시뮬레이터 목록에 *IOS 시뮬레이터* (아래 참조)가 표시 됩니다.

[![](installation-images/xs-xcode8-watchos3-sml.png "Selecting the Simulator type")](installation-images/xs-xcode8-watchos3.png#lightbox)

디버깅을 시작 하면 시뮬레이터 *두 개의* 이 시작 됩니다 (iOS 시뮬레이터 *및* Apple Watch 시뮬레이터). **명령 + Shift + H** 를 사용 하 여 조사식 메뉴와 클록 얼굴로 이동 합니다. **하드웨어** 메뉴를 사용 하 여 **Force Touch 압력**을 설정 합니다. 트랙 패드 또는 마우스에 스크롤 디지털 Crown를 사용 하 여 시뮬레이션 됩니다.

#### <a name="troubleshooting"></a>문제 해결

다음 오류는 페어링된 조사식이 없는 시뮬레이터를 시작 하려고 하면 **응용 프로그램 출력** 에 표시 됩니다.

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

시뮬레이터 구성에 대 한 지침은 [Apple 포럼](https://forums.developer.apple.com/thread/7783) 을 참조 하세요. 기본값은 작동 하지 않습니다.

<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 및 watchOS 1

응용 프로그램을 실행 하거나 디버깅 하기 전에 *조사식 확장 프로젝트* 를 **시작 프로젝트로** 만들어야 합니다. 시작할 수 없습니다"" 자체 watch 앱 및 iOS 앱을 선택 하면 다음 시작 iOS 시뮬레이터에서에서 정상적으로 합니다.

기본적으로 조사식 응용 프로그램은 Mac용 Visual Studio의 **실행** 또는 **디버그** 명령에서 일반 **앱** 모드 (개요 아님 또는 알림 모드)로 시작 됩니다.

Xcode 6을 사용 하는 경우 iPhone 5, iPhone 5 초, iPhone 6 및 iPhone 6 Plus만이 38mm **Apple Watch** 또는 조사식 응용 프로그램이 표시 되는 **Apple Watch** 에 대 한 외부 디스플레이를 활성화할 수 있습니다.

> [!NOTE]
> Xcode 6을 사용 하는 경우에는 조사식 화면이 iOS 시뮬레이터에 자동으로 표시 되지 않습니다.
> **하드웨어 > 외부 디스플레이** 메뉴를 사용 하 여 조사식 화면을 표시 합니다.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>알림 모드를 시작합니다.

코드에서 알림을 처리 하는 방법에 대 한 자세한 내용은 [알림 페이지](~/ios/watchos/platform/notifications.md) 를 참조 하세요.

알림을 위한 알림 _시작 모드_ 를 사용 하 여 watch 앱을 시작할 수 Mac용 Visual Studio:

Watch 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **> 사용자 지정 구성으로 실행**을 선택 합니다.

[![](installation-images/runwith-customparams-sml.png "Running a Custom Configuration")](installation-images/runwith-customparams.png#lightbox)

그러면 **알림** ()을 선택 하 고 JSON 페이로드를 제공할 수 있는 **사용자 지정 매개 변수** 창이 열리고 **실행** 을 눌러 시뮬레이터에서 시청 앱을 시작할 수 있습니다.

[![](installation-images/runwith-execargs-sml.png "Setting the Notification and Payload")](installation-images/runwith-execargs.png#lightbox)

## <a name="debugging"></a>디버깅

Mac 용 Visual Studio 및 Visual Studio에서 디버깅이 지원 됩니다.
알림 모드에서 디버깅 하는 경우 알림 JSON 파일을 제공 해야 합니다. 이 스크린샷은 watch 앱에 도달 하는 디버그 중단점을 보여 줍니다.

![](installation-images/debug-sml.png "This screenshot shows a debug breakpoint being hit in a watch app")

시작 지침을 수행 하면 **IOS 시뮬레이터 (watch)** 에서 실행 되는 시청 앱이 종료 됩니다.
알림 모드의 경우 디버그 > 선택 하 여 **시스템 로그** (**CMD +/** )를 열고 코드에서 `Console.WriteLine`를 사용할 수 있습니다.

### <a name="debugging-lifecycle-event-handlers"></a>수명 주기 이벤트 처리기를 디버깅합니다.

<!--
To test the functionality in your  and 
  methods, use the **Hardware > Lock** command in the iOS Simulator.
  Locking will trigger the `DidDeactivate` method and the watch simulator
  will indicate that it has been locked. Swipe the iOS Simulator to unlock,
  which triggers the `WillActivate` method of the watch app.
-->

WatchOS 템플릿 파일 (예: `InterfaceController`, `ExtensionDelegate`, `NotificationController`및 `ComplicationController`)에는 이미 구현 된 필수 수명 주기 방법이 제공 됩니다. `Console.WriteLine` 호출을 추가 하 고, 이벤트 수명 주기를 보다 잘 이해할 수 있도록 **응용 프로그램 출력** 을 읽습니다.

## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [첫 번째 시청 앱 비디오](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple의 WatchKit 팁](https://developer.apple.com/watchkit/tips/)
