---
title: 탐색 모음
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 9455cac81a0f9ea81e08cf63397e45c1698e1c1b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670014"
---
# <a name="navigation-bar"></a>탐색 모음

Android 4 라는 새 시스템 사용자 인터페이스 기능을 소개를 *탐색 모음*, 탐색 컨트롤에 대 한 하드웨어 단추를 포함 하지 않는 장치에서 제공 하는 **홈**, **다시** , 및 **메뉴**합니다.
다음 스크린 샷에서 Nexus 소수 장치에서 탐색 모음을 보여 줍니다.

 [![Android 탐색 표시줄의 예](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

몇 가지 새 플래그를 사용할 수 있는 탐색 모음 및 해당 컨트롤의 표시 여부 뿐만 아니라 Android 3에 도입 하는 시스템 막대의 표시 유형을 제어 합니다. 플래그에 정의 된는 `Android.View.View` 클래스 및 다음과 같습니다.

-   `SystemUiFlagVisible` &ndash; 탐색 모음을 볼 수 있게 합니다. 
-   `SystemUiFlagLowProfile` &ndash; 탐색 모음에서 컨트롤 dims 합니다. 
-   `SystemUiFlagHideNavigation` &ndash; 탐색 모음을 숨깁니다. 


설정 하 여 계층 구조 보기의에서 보기 중 하나에 적용할 수 있습니다 이러한 플래그는 `SystemUiVisibility` 속성입니다. 여러 뷰가이 속성을 설정 하는 경우 시스템 또는 작업과 결합 하 여 및으로 포커스를 유지 하는 플래그 설정이 창에 적용 합니다. 뷰를 제거 하면 모든 플래그 설정도 제거 됩니다.

다음 예제에서는 간단한 응용 프로그램 단추를 클릭 하면 변경 되는 `SystemUiVisibility`:

 [![표시, 낮은 프로필 및 숨겨진 SystemUiVisibility를 보여 주는 스크린샷](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

변경 하는 코드를 `SystemUiVisibility` 속성을 설정 합니다는 `TextView` 각 단추의 클릭 이벤트 처리기 아래와 같이:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

또한를 `SystemUiVisibility` 변경 발생을 `SystemUiVisibilityChange` 이벤트입니다. 마찬가지로 다음 설정 합니다 `SystemUiVisibility` 속성에 대 한 처리기를 `SystemUiVisibilityChange` 계층의 모든 뷰에 대 한 이벤트를 등록할 수 있습니다. 예를 들어, 사용 하 여 아래 코드는 `TextView` 이벤트에 등록 하는 인스턴스:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>관련 링크

- [SystemUIVisibilityDemo (샘플)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Ice Cream Sandwich 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](https://developer.android.com/sdk/android-4.0.html)
