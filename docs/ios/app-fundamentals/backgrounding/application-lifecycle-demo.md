---
title: Xamarin.ios 용 응용 프로그램 수명 주기 데모
description: 이 문서에서는 iOS 응용 프로그램의 앱 대리자가 처리 하는 다양 한 수명 주기 이벤트를 조사 하 여 이러한 이벤트가 처리 되는 시기와 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/17/2018
ms.openlocfilehash: 13f34f6287d68736ee509e6fb43e5fc47321b907
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73011196"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Xamarin.ios 용 응용 프로그램 수명 주기 데모

이 문서 및 [샘플 코드](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo) 는 iOS의 네 가지 응용 프로그램 상태 및 상태가 변경 될 때 응용 프로그램에 알리는 `AppDelegate` 메서드의 역할을 보여 줍니다. 응용 프로그램은 앱 상태를 변경할 때마다 콘솔에 대 한 업데이트를 인쇄 합니다.

[![](application-lifecycle-demo-images/image3-sml.png "The sample app")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "The app will print updates to the console whenever the app changes state")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>연습

1. **LifecycleDemo** 솔루션에서 **수명 주기** 프로젝트를 엽니다.
1. `AppDelegate` 클래스를 엽니다. 응용 프로그램이 상태를 변경 하는 시간을 나타내기 위해 수명 주기 메서드에 로깅이 추가 되었습니다.

    ```csharp
    public override void OnActivated(UIApplication application)
    {
        Console.WriteLine("OnActivated called, App is active.");
    }
    public override void WillEnterForeground(UIApplication application)
    {
        Console.WriteLine("App will enter foreground");
    }
    public override void OnResignActivation(UIApplication application)
    {
        Console.WriteLine("OnResignActivation called, App moving to inactive state.");
    }
    public override void DidEnterBackground(UIApplication application)
    {
        Console.WriteLine("App entering background state.");
    }
    // not guaranteed that this will run
    public override void WillTerminate(UIApplication application)
    {
        Console.WriteLine("App is terminating.");
    }
    ```

1. 시뮬레이터 또는 장치에서 응용 프로그램을 시작 합니다. 앱이 시작 되 면 `OnActivated`가 호출 됩니다. 응용 프로그램이 _현재 활성_ 상태입니다.
1. 응용 프로그램을 백그라운드로 가져오려면 시뮬레이터 또는 장치의 홈 단추를 누릅니다. `OnResignActivation` 및 `DidEnterBackground`는 앱이 `Active`에서 `Inactive`로 전환 되 고 `Backgrounded` 상태로 전환 될 때 호출 됩니다. 백그라운드에서 실행할 응용 프로그램 코드를 설정 하지 않았으므로 응용 프로그램이 메모리에서 _일시 중단_ 된 것으로 간주 됩니다.
1. 앱으로 다시 이동 하 여 다시 포그라운드로 가져옵니다. `WillEnterForeground` 및 `OnActivated` 모두 호출 됩니다.

    ![](application-lifecycle-demo-images/image4.png "State changes printed to the console")

    뷰 컨트롤러의 다음 코드 줄은 응용 프로그램에서 배경에 전경을 입력 한 경우 실행 되 고 화면에 표시 되는 텍스트를 변경 합니다.

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. **홈** 단추를 눌러 응용 프로그램을 배경에 넣습니다. 그런 다음 **홈** 단추를 두 번 탭 하 여 응용 프로그램 전환기를 표시 합니다. IPhone X에서 화면 아래쪽으로 살짝 밉니다.

    [![응용 프로그램 전환기](application-lifecycle-demo-images/app-switcher-sml.png "응용 프로그램 전환기")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. 앱 전환기에서 응용 프로그램을 찾고 위로 살짝 밀어 제거 합니다 (iOS 11에서 빨간색 아이콘이 모퉁이에 표시 될 때까지 길게 누르기).

    [![실행 중인 앱을 제거 하려면 위로 살짝 밉니다.](application-lifecycle-demo-images/app-switcher-swipe-sml.png "실행 중인 앱을 제거 하려면 위로 살짝 밉니다.")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS에서 응용 프로그램이 종료 됩니다. 응용 프로그램이 백그라운드에서 이미 _일시 중단_ 되었기 때문에 `WillTerminate`가 호출 되지 않습니다.

## <a name="related-links"></a>관련 링크

- [LifecycleDemo (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
