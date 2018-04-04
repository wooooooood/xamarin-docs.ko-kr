---
title: 탐색 모음
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/01/2017
ms.openlocfilehash: 476e73906e4fbe01847740f90a8da701768b29db
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="navigation-bar"></a>탐색 모음

Android 4 라는 새로운 시스템 사용자 인터페이스 기능을 도입는 *탐색 모음*, 탐색 컨트롤에 대 한 하드웨어 단추를 포함 하지 않는 장치에서 제공 하는 **홈**, **다시** , 및 **메뉴**합니다.
다음 스크린 샷에서 Nexus Prime 장치에서 탐색 모음을 보여 줍니다.

 [![Android 탐색 모음의 예](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

몇 가지 새로운 플래그를 사용할 수 있는 탐색 모음과 해당 컨트롤의 표시 여부 뿐만 아니라 Android 3에 도입 된 시스템 표시줄의 표시 여부를 제어 합니다. 에 정의 된 플래그는 `Android.View.View` 클래스와 다음과 같습니다:

-   `SystemUiFlagVisible` &ndash; 탐색 모음을 볼 수 있게 합니다. 
-   `SystemUiFlagLowProfile` &ndash; 탐색 모음에서 컨트롤 dims 합니다. 
-   `SystemUiFlagHideNavigation` &ndash; 탐색 모음을 숨깁니다. 


설정 하 여 계층 구조 보기의에서 보기 중 하나에 적용할 수 있습니다 이러한 플래그는 `SystemUiVisibility` 속성입니다. 여러 뷰가이 속성을 설정 하면 시스템 OR 연산으로 결합 하 여 하 고 포커스를 유지 하는 플래그를 설정 하는 창으로 적용 합니다. 보기를 제거 하는 경우에 설정 된 플래그 제거 됩니다.

다음 예제에서는 간단한 응용 프로그램 단추 중 하나를 클릭 하면 변경 하는 위치는 `SystemUiVisibility`:

 [![Visible, 낮은 프로필과 숨겨진 SystemUiVisibility 보여 주는 스크린샷](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

변경 하는 코드는 `SystemUiVisibility` 에서 속성 설정는 `TextView` 아래 표시 된 대로 클릭 이벤트 처리기에서 각 단추:

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

또한 한 `SystemUiVisibility` 발생 시키는 변경는 `SystemUiVisibilityChange` 이벤트입니다. 마찬가지로 다음 설정의 `SystemUiVisibility` 속성에 대 한 처리기는 `SystemUiVisibilityChange` 계층의 모든 보기에 대 한 이벤트를 등록할 수 있습니다. 예를 들어, 사용 하 여 아래 코드는 `TextView` 등록할 이벤트에 대 한 인스턴스:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>관련 링크

- [SystemUIVisibilityDemo (샘플)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [아이스크림 샌드위치 소개](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](http://developer.android.com/sdk/android-4.0.html)
