---
title: watchOS 문제 해결
description: 이 문서에서는 Xamarin을 사용한 watchOS 개발에 대 한 알려진 문제 및 해결 방법을 설명 합니다. 또한 문제가 있는 이미지를 설명 하 고, 인터페이스 컨트롤러 파일을 수동으로 추가 하 고, 명령줄에서 감시 앱을 시작 하는 등의 방법을 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 17ccc67b2976b93fbb290a1d2425168cab50228e
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84568791"
---
# <a name="watchos-troubleshooting"></a>watchOS 문제 해결

이 페이지에는 발생할 수 있는 문제에 대 한 추가 정보 및 해결 방법이 포함 되어 있습니다.

- [알려진 문제](#knownissues)

- [아이콘 이미지에서 알파 채널 제거](#noalpha)

- Xcode Interface Builder에 대 한 [인터페이스 컨트롤러 파일을 수동으로 추가](#add) 합니다.

- [명령줄에서 WatchApp를 시작](#command_line)합니다.

<a name="knownissues"></a>

## <a name="known-issues"></a>알려진 문제

### <a name="general"></a>일반

<a name="deploy"></a>

- 이전 버전의 Mac용 Visual Studio 잘못 된 **설정** 아이콘 중 하나를 88x88 픽셀로 잘못 표시 합니다. 그러면 앱 스토어에 제출 하려고 할 때 **아이콘 오류가** 표시 되지 않습니다.
    이 아이콘은 87x87 픽셀 (레 티 나 화면의 경우 29 단위) 이어야 합니다 **@3x** . Mac용 Visual Studio에서이 문제를 해결할 수 없습니다. Xcode에서 이미지 자산을 편집 하거나 **콘텐츠. json** 파일을 수동으로 편집 합니다.

- Watch 확장 프로젝트의 **info.plist > WKApp 번들** Id가 watch 앱의 **번들 id**와 일치 하도록 [올바르게 설정](~/ios/watchos/get-started/project-references.md) 되지 않은 경우 디버거는 연결에 실패 하 Mac용 Visual Studio 고 *"디버거가 연결 되기를 기다리는 중입니다." 라는*메시지와 함께 대기 합니다.

- 디버깅은 **알림** 모드에서 지원 되지만 안정적이 지 않을 수 있습니다. 작업을 다시 시도 하는 경우도 있습니다. Watch 앱의 **info.plist** `WKCompanionAppBundleIdentifier` 가 iOS 부모/컨테이너 앱의 번들 식별자 (즉, iPhone에서 실행 되는 앱)와 일치 하도록 설정 되었는지 확인 합니다.

- iOS 디자이너에는 개요 또는 알림 인터페이스 컨트롤러에 대 한 진입점 화살표가 표시 되지 않습니다.

- Storyboard에는 두 개를 추가할 수 없습니다 `WKNotificationControllers` .
    해결 방법: `notificationCategory` 스토리 보드 XML의 요소는 항상 같은로 삽입 됩니다 `id` . 이 문제를 해결 하려면 둘 이상의 알림 컨트롤러를 추가 하 고 텍스트 편집기에서 스토리 보드 파일을 연 다음 `id` 요소를 고유 하 게 변경할 수 있습니다.

    [![](troubleshooting-images/duplicate-id-sml.png "Opening the storyboard file in a text editor and manually change the id element to be unique")](troubleshooting-images/duplicate-id.png#lightbox)

- 앱을 시작 하려고 하면 "응용 프로그램이 빌드되지 않았습니다" 라는 오류가 표시 될 수 있습니다. 이는 시작 프로젝트가 조사식 확장 프로젝트로 설정 된 경우 **정리** 된 후에 발생 합니다.
    해결 방법은 **빌드 > 모두 다시** 빌드를 선택 하 고 앱을 다시 시작 하는 것입니다.

### <a name="visual-studio"></a>Visual Studio

Watch Kit에 대 한 iOS Designer 지원에서는 솔루션을 올바르게 구성 *해야* 합니다. 프로젝트 참조를 설정 하지 않은 경우 ( [참조를 설정 하는 방법](~/ios/watchos/get-started/project-references.md)참조) 디자인 화면이 제대로 작동 하지 않습니다.

<a name="noalpha"></a>

## <a name="removing-the-alpha-channel-from-icon-images"></a>아이콘 이미지에서 알파 채널 제거

아이콘에 알파 채널이 포함 되어서는 안 됩니다 (알파 채널에서 이미지의 투명 영역을 정의 함). 그렇지 않으면 앱 스토어를 전송 하는 동안 앱이 거부 되 고 다음과 같은 오류가 발생 합니다.

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

**미리 보기** 앱을 사용 하 여 Mac OS X에서 알파 채널을 쉽게 제거할 수 있습니다.

1. **미리 보기** 에서 아이콘 이미지를 열고 **파일 > 내보내기**를 선택 합니다.

2. 알파 채널이 있는 경우 표시 되는 대화 상자에는 **알파** 확인란이 포함 됩니다.

    ![](troubleshooting-images/remove-alpha-sml.png "The dialog that appears will include an Alpha checkbox if an alpha channel is present")

3. **알파** *확인란의 선택을* 취소 하 고 파일을 올바른 위치에 **저장** 합니다.

4. 이제 아이콘 이미지가 Apple의 유효성 검사를 통과 해야 합니다.

<a name="add"></a>

## <a name="manually-adding-interface-controller-files"></a>수동으로 인터페이스 컨트롤러 파일 추가

> [!IMPORTANT]
> Xamarin의 WatchKit 지원에는 iOS designer에서 조사식 storyboard 디자인 (Mac용 Visual Studio 및 Visual Studio 둘 다)이 포함 되며, 아래에 설명 된 단계는 필요 하지 않습니다. Mac용 Visual Studio 속성 패드에서 인터페이스 컨트롤러에 클래스 이름을 지정 하기만 하면 c # 코드 파일이 자동으로 생성 됩니다.

Xcode Interface Builder를 사용 하는 *경우* 다음 단계에 따라 watch 앱에 대 한 새 인터페이스 컨트롤러를 만들고 Xcode와 동기화를 사용 하도록 설정 하 여 c #에서 콘센트 및 작업을 사용할 수 있도록 합니다.

1. **Xcode Interface Builder**에서 watch 앱의 **인터페이스. storyboard** 를 엽니다.

    ![](troubleshooting-images/add-6.png "Opening the storyboard in Xcode Interface Builder")

2. 새를 `InterfaceController` 스토리 보드로 끌어 옵니다.

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. 이제 컨트롤을 인터페이스 컨트롤러 (예: 레이블 및 단추)를 사용할 수 있지만,이 경우에는 **.h** 헤더 파일이 없기 때문에 콘센트가 나 작업을 만들 수 없습니다. 다음 단계를 수행 하면 필요한 **.h** 헤더 파일이 생성 됩니다.

    ![](troubleshooting-images/add-2.png "A button in the layout")

4. 스토리 보드를 닫고 Mac용 Visual Studio로 돌아갑니다. **Watch 앱 확장** 프로젝트에서 새 c # 파일 **MyInterfaceController.cs** (또는 원하는 이름을 원하는 대로)를 만듭니다 (storyboard가 있는 WATCH 앱 자체는 아님). 네임 스페이스, classname 및 생성자 이름을 업데이트 하는 다음 코드를 추가 합니다.

    ```csharp
    using System;
    using WatchKit;
    using Foundation;

    namespace WatchAppExtension  // remember to update this
    {
        public partial class MyInterfaceController // remember to update this
        : WKInterfaceController
        {
            public MyInterfaceController // remember to update this
            (IntPtr handle) : base (handle)
            {
            }
            public override void Awake (NSObject context)
            {
                base.Awake (context);
                // Configure interface objects here.
                Console.WriteLine ("{0} awake with context", this);
            }
            public override void WillActivate ()
            {
                // This method is called when the watch view controller is about to be visible to the user.
                Console.WriteLine ("{0} will activate", this);
            }
            public override void DidDeactivate ()
            {
                // This method is called when the watch view controller is no longer visible to the user.
                Console.WriteLine ("{0} did deactivate", this);
            }
        }
    }
    ```

5. **Watch 앱 확장** 프로젝트에서 다른 새 c # 파일 **MyInterfaceController.designer.cs** 을 만들고 아래 코드를 추가 합니다. 네임 스페이스, classname 및 특성을 업데이트 해야 합니다 `Register` .

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;

    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```

    > [!TIP]
    > (선택 사항)이 파일을 Mac용 Visual Studio Solution Pad의 다른 c # 파일로 끌어서이 파일을 첫 번째 파일의 자식 노드로 만들 수 있습니다. 그러면 다음과 같이 표시 됩니다.

    ![](troubleshooting-images/add-5.png "The Solution pad")

6. Xcode 동기화가 사용 된 새 클래스 (특성을 통해)를 인식할 수 있도록 **빌드 > 모두 빌드** 를 선택 `Register` 합니다.

7. Watch 앱 스토리 보드 파일을 마우스 오른쪽 단추로 클릭 하 고 **> Xcode Interface Builder를 사용**하 여 열기를 선택 하 여 스토리 보드를 다시 엽니다.

    ![](troubleshooting-images/add-6.png "Opening the storyboard in Interface Builder")

8. 새 인터페이스 컨트롤러를 선택 하 고 위에서 정의한 클래스 이름 (예:)을 제공 합니다. `MyInterfaceController`.
    모든 항목이 제대로 작동 하는 경우 **클래스:** 드롭다운 목록에 자동으로 표시 되며 여기에서 선택할 수 있습니다.

    ![](troubleshooting-images/add-4.png "Setting a custom class")

9. 스토리 보드와 코드를 나란히 볼 수 있도록 Xcode에서 **도우미 편집기** 보기 (두 개의 겹치는 원이 있는 아이콘)를 선택 합니다.

    ![](troubleshooting-images/add-7.png "The Assistant Editor toolbar item")

    코드 창에 포커스가 있는 경우 .h 헤더 파일을 확인 하 고 이동 경로 탐색 막대를 마우스 오른쪽 단추로 클릭 하 고 올바른 파일 (**MyInterfaceController**)을 선택 합니다 **.**

    ![](troubleshooting-images/add-8.png "Select MyInterfaceController")

10. 이제 **Ctrl +** storyboard에서 **.h** 헤더 파일로 끌어서 작업을 만들 수 있습니다.

    ![](troubleshooting-images/add-9.png "Creating outlets and actions")

    끌기를 해제할 때 콘센트가 나 작업을 만들지 여부를 선택 하 라는 메시지가 표시 되 면 해당 이름을 선택 합니다.

    ![](troubleshooting-images/add-a.png "The outlet and an action dialog")

11. 스토리 보드 변경을 저장 하 고 Xcode를 닫으면 Mac용 Visual Studio로 돌아갑니다. 헤더 파일 변경 내용을 검색 하 고 **designer.cs** 파일에 코드를 자동으로 추가 합니다.

    ```csharp
    [Register ("MyInterfaceController")]
    partial class MyInterfaceController
    {
        [Outlet]
        WatchKit.WKInterfaceButton myButton { get; set; }

        void ReleaseDesignerOutlets ()
        {
            if (myButton != null) {
                myButton.Dispose ();
                myButton = null;
            }
        }
    }
    ```

이제 c #에서 컨트롤을 참조 하거나 작업을 구현할 수 있습니다.

<a name="command_line"></a>

## <a name="launching-the-watch-app-from-the-command-line"></a>명령줄에서 Watch 앱 시작

> [!IMPORTANT]
> 기본적으로 일반 앱 모드에서 시청 앱을 시작할 수 있으며, Mac용 Visual Studio 및 Visual Studio에서 [사용자 지정 실행 매개 변수](~/ios/watchos/get-started/installation.md#custommodes) 를 사용 하 여 **한눈** 에 보기 또는 **알림** 모드를 시작할 수 있습니다.

명령줄을 사용 하 여 iOS 시뮬레이터를 제어할 수도 있습니다. Watch 앱을 시작 하는 데 사용 되는 명령줄 도구는 **mtouch**입니다.

다음은 전체 예제입니다 (터미널에서 한 줄로 실행 됨).

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

앱을 반영 하기 위해 업데이트 해야 하는 매개 변수는 `launchsimwatch` 다음과 같습니다.

### <a name="--launchsimwatch"></a>--launcha watch

*Watch 앱 및 확장을 포함 하는 iOS 앱에 대 한*기본 앱 번들의 전체 경로입니다.

> [!NOTE]
> 제공 해야 하는 경로는 *iPhone 응용 프로그램 파일*에 대 한 것입니다. 즉, iOS 시뮬레이터에 배포 되 고 조사식 확장과 조사식 앱을 모두 포함 하는 앱입니다.

예제:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

## <a name="notification-mode"></a>알림 모드

앱의 [ **알림** 모드](~/ios/watchos/platform/notifications.md)를 테스트 하려면 `watchlaunchmode` 매개 변수를로 설정 하 `Notification` 고 테스트 알림 페이로드가 포함 된 JSON 파일에 대 한 경로를 제공 합니다.

알림 모드에는 페이로드 매개 변수가 *필요* 합니다.

예를 들어 다음 인수를 mtouch 명령에 추가 합니다.

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```

## <a name="other-arguments"></a>기타 인수

나머지 인수는 아래에 설명 되어 있습니다.

### <a name="--sdkroot"></a>--sdkroot

필수 요소. Xcode에 대 한 경로를 지정 합니다 (6.2 이상).

예제:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--장치

실행할 시뮬레이터 장치입니다. 이는 특정 장치의 udid를 사용 하거나 런타임 및 장치 유형의 조합을 사용 하 여 두 가지 방법으로 지정할 수 있습니다.

정확한 값은 컴퓨터 마다 다르며 Apple의 해당 **인증서** 를 사용 하 여 쿼리할 수 있습니다.

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

예제:

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**런타임 및 장치 유형**

예제:

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```

## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WatchTables (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables)
