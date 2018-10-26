---
title: 설치 및 watchOS에서 Xamarin 사용
description: 이 문서에는 설치 및 Xamarin을 사용 하 여 watchOS를 사용 하는 방법을 설명 합니다. 설치, watchOS 프로젝트에 설명 구조체 Xcode 통합을 iOS 디자이너를 사용 하는 방법 및 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 12/05/2017
ms.openlocfilehash: daece8029bd6a97923d6469f9b42d69efbd3f905
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119887"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>설치 및 watchOS에서 Xamarin 사용

watchOS 4에는 macOS Sierra (10.12) Xcode 9가 함께 필요합니다.

watchOS 1에는 원래 Xcode 7을 사용 하 여 OS X Yosemite (10.10) 필요합니다.

> [!WARNING]
> [2018 년 4 월 1 일 후 watchOS 1 업데이트를 허용 하지 것입니다](https://developer.apple.com/news/?id=11162017a)합니다. 향후 업데이트에서 watchOS 2를 사용 해야 SDK 이상; 빌드는 watchos 4 SDK 것이 좋습니다.

## <a name="project-structure"></a>프로젝트 구조

Watch 앱의 세 프로젝트로 구성 됩니다.

- **Xamarin.iOS 앱 프로젝트를 iPhone** -일반 iPhone 프로젝트가이, Xamarin.iOS 템플릿 중 하나일 수 있습니다. Watch 앱 및 해당 확장이 기본 프로젝트 내에서 함께 제공 됩니다.

- **조사식 확장 프로젝트** -Watch 앱에 대 한 코드 (예: 컨트롤러 클래스)를 포함 합니다.

- **Watch 앱 프로젝트** -Watch 앱에 대 한 모든 UI 리소스를 사용 하 여 사용자 인터페이스 스토리 보드 파일을 포함 합니다.

합니다 [조사식 키트 Catalog 예제](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/) Xamarin.Studio에서 다음과 같은 솔루션:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](installation-images/catalog-solution.png "Visual Studio에서 솔루션")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/catalog-solution-vs.png "Visual Studio에서 솔루션")

-----

다운로드 및 실행 합니다 [WatchKitCatalog](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) 샘플으로 시작 합니다.
샘플에서 화면에서 찾을 수 있습니다 합니다 [컨트롤](~/ios/watchos/user-interface/index.md) 페이지입니다.


## <a name="creating-a-new-project"></a>새 프로젝트 만들기

수 없습니다. 솔루션을 만들 새 "Watch"... 대신 Watch 앱을 기존 iOS 응용 프로그램에 추가할 수 있습니다. Watch 앱을 만들려면 다음이 단계를 수행 합니다.

1. 기존 프로젝트에 없는 경우 먼저 선택 **파일 > 새 솔루션** iOS 앱을 만듭니다 (예를 들어를 **단일 뷰 앱**):

    [![](installation-images/cycle8-2-sml.png "파일 선택 > 새 솔루션 및 iOS 앱 만들기")](installation-images/cycle8-2.png#lightbox)

2. IOS 앱이 만들어지면 (또는 기존 iOS 앱을 사용 하려는) 되 면 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...** . 에 **새 프로젝트** 창 선택 **watchOS > 앱 > WatchKit 앱**:

    [![](installation-images/cycle8-6-sml.png "WatchOS 선택 > 앱 > WatchKit 앱")](installation-images/cycle8-6.png#lightbox)

3. 다음 화면에는 iOS 앱 프로젝트는 watch 앱을 포함할지 선택할 수 있습니다.

    [![](installation-images/cycle8-7-sml.png "IOS 앱 프로젝트는 watch 앱을 포함 해야 선택")](installation-images/cycle8-7.png#lightbox)

4. 마지막으로 프로젝트를 저장할 위치를 선택 (및 필요에 따라 소스 제어를 사용 하도록 설정):

    [![](installation-images/cycle8-8-sml.png "프로젝트를 저장할 위치를 선택 합니다.")](installation-images/cycle8-8.png#lightbox)

5. Mac 용 visual Studio에서 자동으로 구성 [프로젝트 참조 및 **Info.plist** 설정](~/ios/watchos/get-started/project-references.md) 있습니다.

## <a name="creating-the-watch-user-interface"></a>보기 사용자 인터페이스 만들기

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Xamarin iOS 디자이너 사용

Watch 앱을 두 번 클릭 **Interface.storyboard** iOS 디자이너를 사용 하 여 편집 합니다. 인터페이스 컨트롤러 및 UI 컨트롤에서 스토리 보드로 끌면 합니다 **도구 상자** 구성 하 고 사용 하 여를 **속성** 패드:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](installation-images/iosdesigner-sml.png "디자이너에서 스토리 보드")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](installation-images/iosdesigner-sml-vs.png "디자이너에서 스토리 보드")](installation-images/iosdesigner-vs.png#lightbox)

-----

각 새 인터페이스 컨트롤러를 지정 해야는 **클래스** 선택 하 고 이름을 입력 한 다음는 **속성** 패드 (이렇게 하면 필요한 만들어집니다 C# 코드 숨김 파일을 자동으로):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](installation-images/iosdesigner-classname.png "각 새 인터페이스 컨트롤러 클래스를 제공")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/iosdesigner-classname-vs.png "각 새 인터페이스 컨트롤러 클래스를 제공")

-----

만들기에서 segue **Ctrl + 드래그** 다른 인터페이스 컨트롤러에 단추, 테이블 또는 인터페이스 컨트롤러에서.


### <a name="using-xcode-on-the-mac"></a>Mac에서 Xcode를 사용 하 여

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Xcode를 사용 하 여 Interface.storyboard 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 사용자 인터페이스를 만들려고 할 수 있습니다 **연결 > Xcode Interface Builder**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio 사용자에 게 전환 Mac 빌드 호스트를 직접 사용 하 여 해당 사용자 인터페이스를 만드는 Xcode를 이용할 수 있습니다.
Mac 용 Visual Studio에서 솔루션을 열고 Interface.storyboard 파일을 마우스 오른쪽 단추로 클릭을 선택한 다음 **연결 > Xcode Interface Builder**:

-----

![](installation-images/openwith-xcode.png "Interface.storyboard Xcode Interface Builder에서 열기")

Xcode를 사용 하는 경우 보통의 경우 watch 앱에 대 한 동일한 단계를 수행 해야 [iOS 앱 스토리 보드](~/ios/user-interface/storyboards/index.md) (출 선 및 작업을 만드는 등 **Ctrl + 드래그** 에 **.h**헤더 파일).

저장 하면 스토리 보드를 자동으로 추가 하는 Xcode Interface Builder에서 출 선 및 작업을 만들기는 C# **. designer.cs** 조사식 확장 프로젝트에서 파일.


### <a name="adding-additional-screens-in-xcode"></a>Xcode에서 추가 화면을 추가합니다.

Xcode Interface Builder를 사용 하 여 스토리 보드를 기본적으로 서식 파일에 포함 된 항목) (이외 추가 화면을 추가 하면 **수동으로 추가 해야 합니다 C# 코드 파일** 각각의 새 인터페이스 컨트롤러에 대 한 합니다.

참조 된 [스토리 보드에 새 인터페이스 컨트롤러를 추가 하는 방법에 지침을 고급](~/ios/watchos/troubleshooting.md#add)합니다.

*Xamarin iOS 디자이너에서는이 자동으로, 수동 단계가 필요 하지 않습니다.*


## <a name="building"></a>빌드

다른 iOS 프로젝트와 같은 watch 앱을 포함 하는 프로젝트를 빌드합니다. 빌드 프로세스에서 코드를 사용 하지 않는 watch 응용 프로그램 (.app)를 포함 하는 watch 확장 (.appex)를 포함 하는 iPhone 응용 프로그램 (.app) 발생 합니다.


## <a name="launching"></a>시작

Studio (Mac 빌드 호스트에서 시작) Visual 또는 Mac 용 Visual Studio를 사용 하 여 시뮬레이터에서 watch 앱을 시작할 수 있습니다.

WatchKit 앱을 시작 하는 것에 대 한 두 가지 모드를 가지 있습니다.

 - 일반 앱 모드 (기본값) 및
- [알림](~/ios/watchos/platform/notifications.md) (JSON 형식으로 테스트 알림 페이로드가 필요).

### <a name="xcode-8-support"></a>Xcode 8 지원

Apple Watch 시뮬레이터는 iOS 시뮬레이터에서에서 별도 Xcode 8 (이상)가 설치 되 면 (달리 [Xcode 6](#xcode6)로 나타난 여기서는 *외부 디스플레이*).
시뮬레이터 목록에 표시 됩니다 Watch 앱 프로젝트를 선택 하 고 시작 프로젝트를 만들 때 *iOS 시뮬레이터* 를 선택할 수 (아래 참조).

[![](installation-images/xs-xcode8-watchos3-sml.png "시뮬레이터 유형 선택")](installation-images/xs-xcode8-watchos3.png#lightbox)

디버깅을 시작할 때 *두* 시뮬레이터 시작 해야-iOS 시뮬레이터 *고* Apple Watch 시뮬레이터입니다. 사용 하 여 **명령 + Shift + H** watch 메뉴 및 시계 화면의;로 이동 하 고 사용 하는 **하드웨어** 설정 하려면 메뉴를 **Force Touch 압력**합니다. 트랙 패드 또는 마우스에 스크롤 디지털 Crown를 사용 하 여 시뮬레이션 됩니다.

#### <a name="troubleshooting"></a>문제 해결

에 다음 오류가 표시 됩니다는 **응용 프로그램 출력** 쌍을 이루는 조사식 없는 시뮬레이터를 시작 하려는 경우:

```csharp
error MT0000: Unexpected error - Please file a bug report at http://bugzilla.xamarin.com
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

가리킵니다 [Apple 포럼](https://forums.developer.apple.com/thread/7783) 기본값 작동 하지 않을 경우 시뮬레이터를 구성 하기 위한 지침은 합니다.


<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 및 watchOS 1

확인 해야 합니다 *조사식 확장 프로젝트* 는 **시작 프로젝트** 실행 또는 앱을 디버깅 하기 전에 합니다. 시작할 수 없습니다"" 자체 watch 앱 및 iOS 앱을 선택 하면 다음 시작 iOS 시뮬레이터에서에서 정상적으로 합니다.

기본적으로 표준에서 watch 앱을 시작 **앱** Mac 용 Visual Studio에서 모드 (없습니다 보기 또는 알림 모드) **실행** 하거나 **디버그** 명령입니다.

Xcode 6을 사용 하는 경우 iPhone 5, iPhone 5, 6, iPhone 및 iPhone 6 Plus에 대 한 외부 디스플레이 활성화할 수 있습니다 **Apple Watch-(38mm** 하거나 **Apple Watch-42 mm** watch 응용 프로그램 될 위치 표시 됩니다.

**참고:** 기억 Xcode 6을 사용 하는 경우 watch 화면 iOS 시뮬레이터에에서 자동으로 표시 되지 않습니다.
사용 된 **하드웨어 > 외부 표시** watch 화면에 표시할 메뉴.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>알림 모드를 시작합니다.

참조 된 [알림 페이지](~/ios/watchos/platform/notifications.md) 정보에 대 한 코드에서 알림을 처리 하는 방법.


Mac 용 visual Studio 알림이 watch 앱을 시작할 수 _시작 모드_ 알림에 대 한 합니다.



Watch 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **사용 하 여 실행 > 사용자 지정 구성...** :


[![](installation-images/runwith-customparams-sml.png "사용자 지정 구성을 실행합니다.")](installation-images/runwith-customparams.png#lightbox)


열립니다는 **사용자 지정 매개 변수** 선택할 수 있는 창 **알림** (및 JSON 페이로드를 제공) 키를 누릅니다 **실행** 시뮬레이터에서 watch 앱을 시작 하려면:


[![](installation-images/runwith-execargs-sml.png "알림 및 페이로드를 설정합니다.")](installation-images/runwith-execargs.png#lightbox)



## <a name="debugging"></a>디버깅

Mac 용 Visual Studio 및 Visual Studio에서 디버깅이 지원 됩니다.
알림 모드에서 디버깅 하는 경우 알림 JSON 파일을 제공 해야 합니다. 이 스크린샷은 watch 앱에 도달 하는 디버그 중단점을 보여 줍니다.

![](installation-images/debug-sml.png "Watch 앱에 도달 하는 디버그 중단점을 보여 주는이 스크린샷")

시작 지침을 따른 후 결국 watch 앱을 실행 합니다 **iOS 시뮬레이터 (감시)** 합니다.
알림 모드를 선택할 수 있습니다 **디버그 > 시스템 로그 열기** (**CMD + /**)를 사용 하 고 `Console.WriteLine` 코드에서.

### <a name="debugging-lifecycle-event-handlers"></a>수명 주기 이벤트 처리기를 디버깅합니다.

<!--
To test the functionality in your  and 
    methods, use the **Hardware > Lock** command in the iOS Simulator.
    Locking will trigger the `DidDeactivate` method and the watch simulator
    will indicate that it has been locked. Swipe the iOS Simulator to unlock,
    which triggers the `WillActivate` method of the watch app.
-->

WatchOS 템플릿 파일 (같은 `InterfaceController`, `ExtensionDelegate`를 `NotificationController`, 및 `ComplicationController`) 이미 구현 하는 필수 수명 주기 메서드를 사용 하 여 제공 합니다. 추가 `Console.WriteLine` 호출과 읽기 합니다 **응용 프로그램 출력** 이벤트 수명 주기를 더 잘 이해 하려면.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [첫 번째 Watch 앱 비디오](http://blog.xamarin.com/your-first-watch-kit-app/)
- [Apple의 WatchKit 팁](https://developer.apple.com/watchkit/tips/)
