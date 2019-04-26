---
title: Xamarin.iOS에 대 한 응용 프로그램 수명 주기 데모
description: 이 문서에서는 이러한 이벤트가 처리 되는 시기와 방법을 보여 주는 iOS 응용 프로그램의 앱 대리자에서 처리 하는 다양 한 수명 주기 이벤트를 검사 합니다.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/17/2018
ms.openlocfilehash: 3beb511c03b328ecea824bf89355d056df003f3e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946195"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Xamarin.iOS에 대 한 응용 프로그램 수명 주기 데모

이 문서 및 [샘플 코드](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/) iOS의 네 가지 응용 프로그램 상태 및 역할을 보여 줍니다는 `AppDelegate` 가져오기 상태를 변경할 때의 응용 프로그램에 게 알리는 방법입니다. 응용 프로그램 앱의 상태가 변경 될 때마다 업데이트 콘솔에 인쇄 됩니다.

[![](application-lifecycle-demo-images/image3-sml.png "샘플 앱")](application-lifecycle-demo-images/image3.png#lightbox)

[![](application-lifecycle-demo-images/image4.png "앱의 상태가 변경 될 때마다 앱에서 콘솔에 업데이트를 인쇄 합니다.")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>연습

1. 엽니다는 **수명 주기** 프로젝트를 **LifecycleDemo** 솔루션입니다.
1. 엽니다는 `AppDelegate` 클래스입니다. 로깅 응용 프로그램에 상태를 변경 하는 경우를 나타내기 위해 수명 주기 메서드를 추가 되었습니다.

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

1. 장치 또는 시뮬레이터에서 응용 프로그램을 시작 합니다. `OnActivated` 앱이 시작 될 때 호출 됩니다. 응용 프로그램을 이제는 _활성_ 상태입니다.
1. 시뮬레이터 또는 백그라운드 응용 프로그램을 장치에서 홈 단추를 누릅니다. `OnResignActivation` 및 `DidEnterBackground` 에서 앱 전환으로 불리게 됩니다 `Active` 하 `Inactive` 으로 `Backgrounded` 상태입니다. 응용 프로그램이 백그라운드에서 실행할 수 없는 응용 프로그램 코드 집합 이므로 비율은 _일시 중단_ 메모리에 있습니다.
1. 다시 포그라운드 앱으로 다시 이동 합니다. `WillEnterForeground` 및 `OnActivated` 모두 호출 됩니다.

    ![](application-lifecycle-demo-images/image4.png "콘솔에 출력 하는 상태 변경")

    뷰 컨트롤러에서 코드의 다음 줄은 응용 프로그램이 백그라운드에서 포그라운드를 입력 하 고 화면에 표시 되는 텍스트를 변경 하는 경우 실행 됩니다.

    ```csharp
    UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
        label.Text = "Welcome back!";
    });
    ```

1. 키를 눌러 합니다 **홈** 백그라운드 응용 프로그램을 배치 하는 단추입니다. 그런 다음 두 번 눌러서 합니다 **홈** 단추 응용 프로그램 전환기를 합니다. Iphone X, 화면 맨 아래에서 위로 살짝 민:

    [![응용 프로그램 전환기](application-lifecycle-demo-images/app-switcher-sml.png "응용 프로그램 전환기")](application-lifecycle-demo-images/app-switcher.png#lightbox)
  
1. 응용 프로그램 전환기에서 응용 프로그램을 찾아서 (iOS 11, 길게 누름 모서리에 빨간색 아이콘이 표시 될 때까지)에서 제거 하려면 위쪽으로 살짝:

    [![실행 중인 앱을 제거 하는 최대 안쪽으로 살짝 밀어](application-lifecycle-demo-images/app-switcher-swipe-sml.png "제거를 실행 중인 응용 프로그램까지 살짝 밀기")](application-lifecycle-demo-images/app-switcher-swipe.png#lightbox)

iOS 응용 프로그램을 종료 합니다. 사실은 `WillTerminate` 응용 프로그램이 이미 있으므로 호출 되지 않습니다 _일시 중단_ 백그라운드에서.

## <a name="related-links"></a>관련 링크

- [LifecycleDemo (샘플)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
