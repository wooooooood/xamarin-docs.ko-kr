---
title: watchOS 문제 해결
description: 알려진된 문제 및 watchOS 개발 문제에 대 한 대안입니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 6e7a7dd09d65b88831136662d8718886aaf483c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="watchos-troubleshooting"></a>watchOS 문제 해결

이 페이지에는 추가 정보 및 해결 방법 아직 개발 중인 기능에 대 한 포함 되어 있습니다. 이러한 해결 방법 중 일부 우리의 미리 보기 버전에만 적용 됩니다.

- [알려진 문제](#knownissues)

- [알파 채널 아이콘 이미지에서 제거](#noalpha)

- [인터페이스 컨트롤러 파일을 수동으로 추가](#add) Xcode 인터페이스 작성기에 대 한 합니다.

- [명령줄에서 WatchApp 시작](#command_line)합니다.

<a name="knownissues" />

## <a name="known-issues"></a>알려진 문제

### <a name="general"></a>일반

<a name="deploy" />

- Mac 용 Visual Studio의 이전 릴리스에서 잘못 표시 하 고 중 하나는 **AppleCompanionSettings** 아이콘 88 x 88 픽셀; 결과적으로 **누락 아이콘 오류** 앱을 제출 하려는 경우 저장소입니다.
    이 아이콘 87 x 87 픽셀 이어야 합니다. (29 단위에 대 한 **@3x** 레 티 나 화면). 이 문제는 해결 Visual Studio에서 Mac-중 하나가 편집 Xcode에서 이미지 자산 수 없거나 수동으로 편집 하는 **Contents.json** 파일 (일치 하도록 [이 샘플](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- 경우 조사식 확장 프로젝트 **Info.plist > WKApp 번들 ID** 않습니다 [올바르게 설정](~/ios/watchos/get-started/project-references.md) Watch 앱에 맞게 **번들 ID**, 디버거를 연결 하지 못합니다 및 시각적 Mac 용 studio 메시지와 대기 *"디버거를 연결할 때까지 기다리는"*합니다.

- 디버깅은 지원 **알림** 모드 이지만 안정적 수 있습니다. 다시 시도 하는 경우에 따라 작동 합니다. 있는지 확인 Watch 앱 **Info.plist** `WKCompanionAppBundleIdentifier` iOS 부모/컨테이너 응용 프로그램의 번들 식별자와 일치 하도록 설정 됩니다 (ie. iPhone에서 실행 되는 것).

- iOS 디자이너 보기 또는 알림 인터페이스 컨트롤러에 대 한 진입점 화살표를 표시 하지 않습니다.

- 두 개를 추가할 수 없습니다 `WKNotificationControllers` 스토리 보드에 있습니다.
    해결 방법:는 `notificationCategory` XML 스토리 보드에서 요소를 동일한 삽입 항상 `id`합니다. 두 개 이상의 알림 컨트롤러를 추가할을 텍스트 편집기에서 스토리 보드 파일을 열고 한 다음 변경 수동으로이 문제를 해결 하려면는 `id` 요소를 고유 하 게 합니다.

    [![](troubleshooting-images/duplicate-id-sml.png "텍스트 편집기에서 파일을 고유 하 게 하는 id 요소를 수동으로 변경 스토리 보드 열기")](troubleshooting-images/duplicate-id.png#lightbox)

- "응용 프로그램을 작성 되지 않았습니다." 오류가 표시 될 수 응용 프로그램을 실행 하려고 할 때입니다. 다음에 발생 한 **Clean** 조사식 확장 프로젝트를 시작 프로젝트로 설정 된 경우.
    선택 하는 수정 프로그램 **빌드 > 모두 다시 빌드** 하 고 응용 프로그램을 다시 시작 합니다.

### <a name="visual-studio"></a>Visual Studio

IOS 디자이너 조사식 키트에 대 한 지원 *필요* 솔루션을 올바르게 구성할 수 있습니다. 프로젝트 참조를 설정 하지 않은 경우 (참조 [참조를 설정 하는 방법을](~/ios/watchos/get-started/project-references.md)) 디자인 화면 제대로 작동 하지 것입니다.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>알파 채널 아이콘 이미지에서 제거

아이콘 포함 될 수 없습니다 (알파 채널 이미지의 투명 영역을 정의 하는 데 사용)에 알파 채널이, 앱 스토어 제출 다음과 비슷한 오류가 발생 하는 동안 앱을 거부 하는 그렇지 않은 경우:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

쉽게 사용 하 여 Mac OS X에서 알파 채널을 제거 하는 **미리 보기** 앱:

1. 아이콘 이미지에서 열고 **미리 보기** 선택한 후 **파일 > 내보내기**합니다.

2. 나타나는 대화 상자에 포함 됩니다는 **알파** 알파 채널이 있는 경우 확인란을 선택 합니다.

    ![](troubleshooting-images/remove-alpha-sml.png "알파 채널이 있는 경우 대화 상자가 표시 되는 알파 확인란 포함 됩니다.")

3. *Untick* 는 **알파** 확인란을 선택 하 고 **저장** 올바른 위치에 파일입니다.

4. 이제 아이콘 이미지 Apple의 유효성 검사를 통과 해야 합니다.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>인터페이스 컨트롤러 파일을 수동으로 추가

> [!IMPORTANT]
> Xamarin의 WatchKit 지원 조사식 스토리 보드 아래 설명 된 단계를 요구 하지 않는 (모두 Mac 용 Visual Studio 및 Visual Studio에서), iOS 디자이너에서 디자인 포함 됩니다. 단순히 인터페이스 컨트롤러를 Mac 속성 패드에 대 한 Visual Studio 및 C# 코드 파일을 자동으로 만들에서 클래스 이름을 지정 합니다.


*경우* Xcode 인터페이스 작성기를 사용 하는 watch 앱에 대 한 새 인터페이스 컨트롤러를 만들고 콘센트 및 동작을 C#에서 사용할 수 있도록 Xcode와의 동기화를 사용 하도록 설정 하려면 다음이 단계를 수행 하십시오.

1. Watch 앱을 열고 **Interface.storyboard** 에 **Xcode 인터페이스 작성기**합니다.
    
    ![](troubleshooting-images/add-6.png "Xcode 인터페이스 작성기에서 스토리 보드 열기")

2. 새 끌어 `InterfaceController` 스토리 보드에:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. 이제 인터페이스 컨트롤러에 컨트롤을 않음 (예: 끌어 수 있습니다. 레이블 및 단추) 만들 수는 없습니다 콘센트 또는 동작 아직 있기 때문에 없는 **.h** 헤더 파일입니다. 다음 절차를 수행 하면 필요한 **.h** 헤더 파일을 만들어야 합니다.

    ![](troubleshooting-images/add-2.png "레이아웃의 단추")

4. 스토리 보드를 닫고 Mac.에 대 한 Visual Studio로 돌아가기 새 C# 파일을 만들 **MyInterfaceController.cs** (또는 원하는 원하는 이름)에 **앱 확장을 시청** 프로젝트 (하지 여기서 스토리 보드는 자체는 watch 앱). 다음 코드 (네임 스페이스, classname, 및 생성자 이름 업데이트)를 추가 합니다.

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

5. 다른 새 C# 파일을 만들 **MyInterfaceController.designer.cs** 에 **앱 확장을 시청** 프로젝트 및 아래 코드를 추가 합니다. 네임 스페이스, 클래스 업데이트 해야 및 `Register` 특성:

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
    
    팁: 만들 수 있습니다 (선택 사항)이 Mac 솔루션 패드에 대 한 다른 C# 파일 Visual Studio에서 위로 끌어 첫 번째 파일의 자식 노드를 파일. 그런 다음 다음과 같이 표시 됩니다.
    
    ![](troubleshooting-images/add-5.png "솔루션 채움")

6. 선택 **빌드 > 모두 빌드** Xcode 동기화는 새 클래스를 인식할 수 있도록 (통해는 `Register` 특성) 사용 하는 것입니다.

7. 조사식 앱 스토리 보드 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 스토리 보드를 열고 다시 **프로그램 > Xcode 인터페이스 작성기**:

    ![](troubleshooting-images/add-6.png "스토리 보드 인터페이스 작성기에서 열기")

8. 새 인터페이스 컨트롤러를 선택 하 고 예: 위의 정의 classname 합니다. `MyInterfaceController`.
모든 항목에 올바르게 작동 하는 경우에 자동으로 표시 됩니다는 **클래스:** 드롭다운 목록 및 여기에서 선택할 수 있습니다.

    ![](troubleshooting-images/add-4.png "사용자 지정 클래스를 설정합니다.")

9. 선택 된 **도우미 편집기** 스토리 보드 및는 코드-함께 볼 수 있도록 Xcode (두 개의 겹치는 원 있는 아이콘)에서 보기:

    ![](troubleshooting-images/add-7.png "도우미 편집기 도구 모음 항목")

    코드 창에 포커스가 있으면 확인에서 보면는 **.h** 헤더 파일과 if 하지 이동 경로 탐색 막대에서 마우스 오른쪽 단추로 클릭 하 고 올바른 파일 선택 (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "MyInterfaceController 선택")

10. 이제 콘센트 및 작업으로 만들 수 **Ctrl + 드래그** 에 스토리 보드에서는 **.h** 헤더 파일.

    ![](troubleshooting-images/add-9.png "콘센트 및 작업 만들기")

    끌어서 놓으면 콘센트 또는 같은 작업을 만들 것인지 선택 하 라는 메시지가 표시 될 수 하 고 해당 이름을 선택:

    ![](troubleshooting-images/add-a.png "소켓과 동작 대화 상자")

11. 스토리 보드 변경 내용이 저장 되 고 Xcode 닫혀을 Mac.에 대 한 Visual Studio로 돌아가서 헤더 파일 변경 내용을 감지 하 고 코드를 자동으로 추가 하는 **. designer.cs** 파일:


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


이제 컨트롤 참조 (하거나 된 동작을 구현) C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>명령줄에서 Watch 앱을 시작합니다.

> [!IMPORTANT]
> 기본적으로 및에서 일반적인 앱 모드로 Watch 앱을 시작할 수 **보기** 또는 **알림** 모드를 사용 하 여 [사용자 지정 실행 매개 변수](~/ios/watchos/get-started/installation.md#custommodes) Mac 용 Visual Studio에서 및 Visual Studio 합니다.


IOS 시뮬레이터를 제어 하려면 명령줄을 사용할 수도 있습니다. Watch 앱을 시작 하는 데 사용 하는 명령줄 도구는 **mtouch**합니다.

(터미널에서을 한 줄으로 실행) 하는 전체 예제는 다음과 같습니다.

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

응용 프로그램에 맞게 업데이트 해야 할 매개 변수는 `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

기본 응용 프로그램 번들의 전체 경로 *watch 앱 및 확장을 포함 하는 iOS 앱에 대 한*합니다.

> [!NOTE]
> 에 대 한을 제공 해야 하는 경로는 *iPhone 응용 프로그램.app 파일*, 하는 iOS 시뮬레이터로 배포 되 고 조사식 확장와 watch 앱 모두를 포함 하는 것, 즉 합니다.

예제:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>알림 모드

응용 프로그램의 테스트 하려면 [ **알림** 모드](~/ios/watchos/platform/notifications.md)로 설정 된 `watchlaunchmode` 매개 변수를 `Notification` 테스트 알림 페이로드를 포함 하는 JSON 파일에 대 한 경로 제공 합니다.

페이로드 매개 변수는 *필요한* 알림 모드에 대 한 합니다.

예를 들어 mtouch 명령에 이러한 인수를 추가 합니다.

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>다른 인수

나머지 인수는 아래 설명 되어 있습니다.

### <a name="--sdkroot"></a>--sdkroot

필수. Xcode (6.2 이상)에 대 한 경로 지정합니다.

예제:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--device

시뮬레이터 장치 실행입니다. 이 런타임 및 장치 유형의 조합을 사용 하 여 하거나 특정 장치 udid를 사용 하 여 두 가지 방법으로 지정할 수 있습니다.

정확한 값 컴퓨터 간에 다르며 Apple의를 사용 하 여 쿼리할 수 **simctl** 도구:

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

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
