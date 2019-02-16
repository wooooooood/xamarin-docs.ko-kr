---
title: watchOS 문제 해결
description: 이 문서는 알려진된 문제 및 Xamarin 사용한 watchOS 개발에 대 한 대안을 설명합니다. 인터페이스 컨트롤러 파일, 명령줄에서 watch 앱을 시작 하거나 수동으로 추가 하는 문제 등을 사용 하 여 이미지를 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 70ef341c066c77e214761d75c173faef00266e4c
ms.sourcegitcommit: 2713f2c1d74e3582704c3d0ca65b6651119ed489
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56321157"
---
# <a name="watchos-troubleshooting"></a>watchOS 문제 해결

이 페이지에는 추가 정보 및 문제를 발생할 수 있습니다.

- [알려진 문제](#knownissues)

- [아이콘 이미지에서 알파 채널 제거](#noalpha)

- [인터페이스 컨트롤러 파일을 수동으로 추가](#add) Xcode Interface Builder에 대 한 합니다.

- [From the Command Line WatchApp 시작](#command_line)합니다.

<a name="knownissues" />

## <a name="known-issues"></a>알려진 문제

### <a name="general"></a>일반

<a name="deploy" />

- 이전 릴리스의 Mac 용 Visual Studio로 잘못 표시 중 하나는 **AppleCompanionSettings** 는 88 x 88 픽셀;으로 아이콘을 **누락 된 아이콘 오류** 앱 스토어에 제출 하려는 경우.
    이 아이콘 87 x 87 픽셀 이어야 합니다. (29 단위 **@3x** 레 티 나 화면). Xcode의 이미지 자산을 편집 하거나-Mac 용 Visual Studio에서이 해결 하거나 수동으로 편집할 수 없습니다는 **Contents.json** 파일 (맞게 [이 샘플](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- 경우 조사식 확장 프로젝트 **Info.plist > WKApp 번들 ID** 아닙니다 [올바르게 설정](~/ios/watchos/get-started/project-references.md) Watch 앱에 맞게 **번들 ID**, 디버거를 연결 하지 못합니다 및 시각적 메시지를 사용 하 여 Mac 용 studio 대기할 *"디버거를 연결할 때까지 기다리는"* 합니다.

- 디버깅은 지원 됩니다 **알림을** 모드 하지만 신뢰할 수 없습니다. 다시 시도 경우에 따라 작동 합니다. 확인 Watch 앱의 **Info.plist** `WKCompanionAppBundleIdentifier` iOS 부모/컨테이너 앱의 번들 식별자를 일치 하도록 설정 된 (ie. iPhone에서 실행 되는 것).

- iOS 디자이너 보기 또는 알림 인터페이스 컨트롤러에 대 한 entrypoint 화살표를 표시 하지 않습니다.

- 두 개를 추가할 수 없습니다 `WKNotificationControllers` 스토리 보드입니다.
    해결 방법: 합니다 `notificationCategory` storyboard XML에에서 요소를 동일한 삽입 항상 `id`합니다. 알림 컨트롤러 두 개 (이상), 텍스트 편집기에서 스토리 보드 파일을 엽니다을 추가한 다음 수동으로 변경이 문제를 해결 하려면는 `id` 요소 고유 해야 합니다.

    [![](troubleshooting-images/duplicate-id-sml.png "텍스트 편집기에서 파일을 고유 하 게 id 요소를 수동으로 변경 스토리 보드 열기")](troubleshooting-images/duplicate-id.png#lightbox)

- "응용 프로그램을 작성 되지 않았습니다." 오류가 표시 될 수 있습니다 응용 프로그램을 실행 하려고 할 때입니다. 다음에 발생 한 **정리** 조사식 확장 프로젝트를 시작 프로젝트로 설정 된 경우.
    수정 사항을 선택 하는 것 **빌드 > 모두 다시 빌드** 앱 다시 시작 합니다.

### <a name="visual-studio"></a>Visual Studio

IOS 디자이너 조사식 키트에 대 한 지원 *필요* 제대로 구성 된 솔루션입니다. 프로젝트 참조를 설정 하지 않은 경우 (참조 [참조를 설정 하는 방법을](~/ios/watchos/get-started/project-references.md)) 디자인 화면을 올바르게 작동 하지 것입니다.

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>아이콘 이미지에서 알파 채널 제거

아이콘에 알파 채널이 (알파 채널 이미지의 투명 영역을 정의 하는 데 사용)를 포함 하지 않아야, 그렇지 않으면 다음과 유사한 오류가 발생 하 여 앱 스토어 제출 하는 동안 앱 거부 됩니다.

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

다음을 사용 하 여 Mac OS X에서 알파 채널을 제거 하는 것이 쉽습니다 합니다 **미리 보기** 앱:

1. 아이콘 이미지를 엽니다 **미리 보기** 를 선택한 후 **파일 > 내보내기**합니다.

2. 대화 상자가 나타나면 포함 됩니다는 **알파** 알파 채널이 있는 경우 확인란을 선택 합니다.

    ![](troubleshooting-images/remove-alpha-sml.png "알파 채널이 있는 경우 나타나는 대화 상자는 알파 확인란이 포함 됩니다.")

3. *Untick* 는 **알파** 확인란을 선택 하 고 **저장** 올바른 위치로 파일.

4. 아이콘 이미지 이제 Apple의 유효성 검사 통과 합니다.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>인터페이스 컨트롤러 파일을 수동으로 추가

> [!IMPORTANT]
> Xamarin의 WatchKit 지원 아래에 설명 된 단계를 요구 하지 않는 (Mac 용 Visual Studio 및 Visual Studio에서), iOS 디자이너에서 조사식 스토리 보드 디자인을 포함 합니다. 단순히 이름을 인터페이스 컨트롤러 클래스는 Visual Studio에서 Mac 속성 패드에 대 한는 C# 코드 파일을 자동으로 생성 됩니다.


*하는 경우* Xcode Interface Builder를 사용 하는 존재 하지 않는 watch 앱에 대 한 새 인터페이스 컨트롤러를 만들고 출 선 및 작업에서 사용할 수 있도록 Xcode 사용 하 여 동기화를 사용 하도록 설정 하려면 다음이 단계를 수행 합니다 C#:

1. Watch 앱을 엽니다 **Interface.storyboard** 에 **Xcode Interface Builder**합니다.
    
    ![](troubleshooting-images/add-6.png "Xcode Interface Builder에서 스토리 보드 열기")

2. 새 끌어 `InterfaceController` 스토리 보드에:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. 이제 끌 수 컨트롤 인터페이스 컨트롤러 (예: 레이블과 단추) 만들 수는 없습니다 출 선 또는 작업 아직 있기 때문에 없습니다 **.h** 헤더 파일입니다. 다음 단계는 필요 하면 **.h** 헤더 파일을 만들어야 합니다.

    ![](troubleshooting-images/add-2.png "레이아웃에서 단추")

4. 스토리 보드를 닫고 mac 용 Visual Studio로 돌아가서 새 C# 파일 **MyInterfaceController.cs** (또는 원하는 무엇이 든 이름)에 **앱 확장을 시청 하세요** 프로젝트 (없습니다 watch 앱 스토리 보드가 있는 자체). (네임 스페이스, classname, 및 생성자 이름을 업데이트) 다음 코드를 추가 합니다.

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

5. 다른 새를 만들 C# 파일 **MyInterfaceController.designer.cs** 에 **앱 확장을 시청 하세요** 프로젝트 및 아래 코드를 추가 합니다. 네임 스페이스, 클래스를 업데이트 해야 하며 `Register` 특성:

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
    
    팁: (선택 사항)을 유지할 수 있습니다 다른으로 끌어서 첫 번째 파일의 자식 노드를 파일 C# Mac 솔루션 패드에 대 한 Visual Studio에서 파일입니다. 다음 다음과 같이 표시 됩니다.
    
    ![](troubleshooting-images/add-5.png "Solution pad")

6. 선택 **빌드 > 모두 빌드** Xcode 동기화는 새 클래스를 인식 (통해는 `Register` 특성) 사용 하는 것입니다.

7. Watch 앱 스토리 보드 파일을 마우스 오른쪽 단추로 클릭 하 고 선택 하 여 스토리 보드를 다시 엽니다 **연결 > Xcode Interface Builder**:

    ![](troubleshooting-images/add-6.png "Interface Builder에서 스토리 보드 열기")

8. 새 인터페이스 컨트롤러를 선택 하 고 예를 들어 위에 정의 클래스를 제공 합니다. `MyInterfaceController`.
모든 것이 제대로 작동 하는 경우에 자동으로 표시 되어야 합니다 **클래스:** 드롭다운 목록 및 여기에서 선택할 수 있습니다.

    ![](troubleshooting-images/add-4.png "사용자 지정 클래스를 설정합니다.")

9. 선택 합니다 **도우미 편집기** Xcode (두 개의 겹치는 원을 사용 하 여 아이콘)에서 보고 있는 스토리 보드 및는 코드에서 나란히 볼 수 있습니다.

    ![](troubleshooting-images/add-7.png "단말기 편집기 도구 모음 항목")

    코드 창에 포커스가 있을 때 보면 하 고 있는지 확인 합니다 **.h** 헤더 파일인 고 하지 이동 경로 탐색 막대에서 마우스 오른쪽 단추로 클릭 하 고 올바른 파일을 선택 하는 경우 (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "MyInterfaceController 선택")

10. 출 선 및 작업을 만들 수 있습니다 **Ctrl + 끌기** 에 스토리 보드에서 합니다 **.h** 헤더 파일입니다.

    ![](troubleshooting-images/add-9.png "출 선 및 작업 만들기")

    끌어서 놓으면 출 선 또는 작업을 만들지 여부를 선택 하 라는 메시지가 표시 됩니다 하 고 해당 이름을 선택 합니다.

    ![](troubleshooting-images/add-a.png "출 선 및 작업 대화 상자를")

11. Xcode가 닫혀 스토리 보드 변경 내용이 저장 되 고 mac 용 Visual Studio로 돌아가서 헤더 파일 변경 내용을 감지 하 고 자동으로 코드를 추가 합니다 **. designer.cs** 파일:


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


이제 컨트롤 참조 (하거나 작업을 구현)에서 C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>From the Command Line Watch 앱 시작

> [!IMPORTANT]
> 기본적으로 및에서 일반 앱 모드로 Watch 앱을 시작할 수 있습니다 **한 눈에 보기** 하거나 **알림** 모드를 사용 하 여 [매개 변수 사용자 지정 실행](~/ios/watchos/get-started/installation.md#custommodes) Mac 용 Visual Studio에서 및 Visual Studio입니다.


또한 iOS 시뮬레이터를 제어 하는 명령줄을 사용할 수 있습니다. Watch 앱을 시작 하는 데 사용 하는 명령줄 도구는 **mtouch**합니다.

(터미널에서 한 줄으로 실행) 하는 전체 예제는 다음과 같습니다.

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

앱을 반영 하도록 업데이트 해야 할 매개 변수가 `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

기본 앱 번들의 전체 경로 *watch 앱 및 확장을 포함 하는 iOS 앱에 대 한*합니다.

> [!NOTE]
> 제공 해야 하는 경로입니다 합니다 *iPhone 응용 프로그램.app 파일*, iOS 시뮬레이터에 배포 되는 고 watch 앱 및 watch 확장을 포함 하는 것, 즉 합니다.

예제:

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>알림 모드

앱을 테스트할 [ **알림** 모드](~/ios/watchos/platform/notifications.md)설정 합니다 `watchlaunchmode` 매개 변수를 `Notification` 테스트 알림 페이로드가 포함 된 JSON 파일의 경로를 제공 합니다.

페이로드 매개 변수가 *필요한* 알림 모드에 대 한 합니다.

예를 들어, mtouch 명령에 이러한 인수를 추가 합니다.

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>다른 인수

나머지 인수는 아래 설명 되어 있습니다.

### <a name="--sdkroot"></a>--sdkroot

필수 요소. Xcode (6.2 이상)에 대 한 경로 지정합니다.

예제:

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--device

시뮬레이터 장치 실행입니다. 이 두 가지 방법으로, 특정 장치의 udid를 사용 하 여 또는 런타임 형식과 장치를 조합 하 여 지정할 수 있습니다.

정확한 값을 컴퓨터 간에 다르며 Apple를 사용 하 여 쿼리할 수 있습니다 **simctl** 도구:

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
