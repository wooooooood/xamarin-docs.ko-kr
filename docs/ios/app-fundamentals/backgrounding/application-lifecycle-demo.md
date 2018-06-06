---
title: Xamarin.iOS에 대 한 응용 프로그램 수명 주기 데모
description: 이 문서에서는 이러한 이벤트가 처리 되는 시기와 방법을 보여 주는 iOS 응용 프로그램의 응용 프로그램 대리자에서 처리 하는 다양 한 수명 주기 이벤트를 검사 합니다.
ms.prod: xamarin
ms.assetid: 5C8AACA6-49F8-4C6D-99C3-5F443C01B230
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 64c695065012e4bf796c219c260324d9b6278ca5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783586"
---
# <a name="application-lifecycle-demo-for-xamarinios"></a>Xamarin.iOS에 대 한 응용 프로그램 수명 주기 데모

이 섹션에서는 하겠습니다 4 개의 응용 프로그램 상태와의 역할을 보여 주는 응용 프로그램을 검사 하는 `AppDelegate` 상태 가져오기 변경 하는 경우의 응용 프로그램에 게 알리는 메서드. 응용 프로그램의 상태가 변경 될 때마다 응용 프로그램 콘솔에 대 한 업데이트를 인쇄 합니다.

 [![](application-lifecycle-demo-images/image3.png "샘플 응용 프로그램")](application-lifecycle-demo-images/image3.png#lightbox)

 [![](application-lifecycle-demo-images/image4.png "응용 프로그램의 상태가 변경 될 때마다 응용 프로그램 업데이트는 콘솔에 인쇄 됩니다.")](application-lifecycle-demo-images/image4.png#lightbox)

## <a name="walkthrough"></a>연습


  1. 열기는 _수명 주기_ 프로젝트에 _LifecycleDemo_ 솔루션입니다.
  1. 열립니다는 `AppDelegate` 클래스입니다. Note 응용 프로그램 상태가 변경 될 때 알려주시면 수명 주기 메서드 로깅을 추가 했습니다.

            ```chsarp
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

  1. 장치 또는 시뮬레이터에서 응용 프로그램을 시작 합니다. `OnActivated` 앱이 시작 되 면 호출 됩니다. 응용 프로그램을 이제는 _활성_ 상태입니다.
  1. 시뮬레이터 또는 장치를 백그라운드 응용 프로그램을 홈 단추를 누릅니다. `OnResignActivation` 및 `DidEnterBackground` 에서 응용 프로그램 전환으로 호출할 수 `Active` 를 `Inactive` 로 회수는 `Backgrounded` 상태입니다. 응용 프로그램에서는 제공 되지 않은 응용 프로그램이 백그라운드에서 실행할 코드를, 같으므로으로 간주 됩니다 _Suspended_ 메모리에 있습니다.
  1. 전경으로 다시 상태로 전환 하려면 앱으로 다시 이동 합니다. `WillEnterForeground` 및 `OnActivated` 모두 호출 됩니다.

        ![](application-lifecycle-demo-images/image4.png "콘솔에 인쇄 하는 상태 변경")

    Note 응용 프로그램이 백그라운드에서 포그라운드를 입력 했는지 알리는 보기 controller에 다음 코드 줄을 추가 했습니다.

        ```csharp
            UIApplication.Notifications.ObserveWillEnterForeground ((sender, args) => {
                    label.Text = "Welcome back!";
                });
        ```

1. 키를 눌러는 **홈** 버튼을 배경으로 응용 프로그램을 합니다. 그런 다음 두 번 누르면은 **홈** 단추 응용 프로그램 전환기를:
    
    ![](application-lifecycle-demo-images/app-switcher-.png "응용 프로그램 전환기")
  
1. 응용 프로그램 전환기에서 응용 프로그램을 찾아서 제거 하려면 위쪽으로 살짝:
    
    ![](application-lifecycle-demo-images/app-switcher-swipe-.png "실행 중인 응용 프로그램을 제거 하려면 위쪽으로 살짝") 
    
iOS 응용 프로그램을 종료 합니다. `WillTerminate` 응용 프로그램을 끝내 야 하기 때문에 호출 되지 않습니다 _Suspended_ 백그라운드에서 합니다.

IOS 응용 프로그램 상태와 전환, 점을 이해 했으므로 ios에서 backgrounding에 사용할 수 있는 다양 한 옵션을 참조를 이동 합니다.



## <a name="related-links"></a>관련 링크

- [LifecycleDemo(Part2) (샘플)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
