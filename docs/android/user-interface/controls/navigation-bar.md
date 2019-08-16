---
title: Xamarin Android 탐색 모음
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 3bb2b47623c03d335ae1edc4bf87881622823ea1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522931"
---
# <a name="xamarinandroid-navigation-bar"></a>Xamarin Android 탐색 모음

Android 4에는 **홈**, **뒤로**및 **메뉴**를 위한 하드웨어 단추가 포함 되지 않은 장치에 대 한 탐색 컨트롤을 제공 하는 *탐색 모음*이라는 새로운 시스템 사용자 인터페이스 기능이 도입 되었습니다.
다음 스크린샷은 Nexus 프라임 장치의 탐색 모음을 보여 줍니다.

 [![Android 탐색 모음의 예](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Android 3에 도입 된 시스템 표시줄의 표시 유형 뿐만 아니라 탐색 모음과 해당 컨트롤의 표시 여부를 제어 하는 몇 가지 새로운 플래그를 사용할 수 있습니다. 플래그는 `Android.View.View` 클래스에 정의 되며 아래에 나열 되어 있습니다.

- `SystemUiFlagVisible`&ndash; 탐색 모음이 표시 되도록 합니다. 
- `SystemUiFlagLowProfile`&ndash; 탐색 모음에서 컨트롤을 희미하게 표시 합니다. 
- `SystemUiFlagHideNavigation`&ndash; 탐색 모음을 숨깁니다. 


이러한 플래그는 속성을 `SystemUiVisibility` 설정 하 여 뷰 계층의 모든 뷰에 적용할 수 있습니다. 여러 뷰에이 속성이 설정 된 경우 시스템은 OR 작업과 결합 하 고 플래그가 설정 된 창이 포커스를 유지 하는 동안이를 적용 합니다. 뷰를 제거 하면 설정 된 플래그도 제거 됩니다.

다음 예제에서는 단추 중 하나를 클릭 하면가 `SystemUiVisibility`변경 되는 간단한 응용 프로그램을 보여 줍니다.

 [![표시, 낮은 프로필 및 숨겨진 SystemUiVisibility를 보여 주는 스크린샷](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

를 변경 `SystemUiVisibility` 하는 코드는 아래와 같이 각 `TextView` 단추의 click 이벤트 처리기에서 속성을 설정 합니다.

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

또한 변경으로 `SystemUiVisibility` `SystemUiVisibilityChange` 이벤트를 발생 시킵니다. `SystemUiVisibility` 속성을 설정 하는 것과 마찬가지로 계층의 `SystemUiVisibilityChange` 모든 뷰에 대해 이벤트 처리기를 등록할 수 있습니다. 예를 들어 아래 코드는 `TextView` 인스턴스를 사용 하 여 이벤트를 등록 합니다.

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>관련 링크

- [SystemUIVisibilityDemo (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/systemuivisibilitydemo)
- [아이스크림 및 사우스 샌드위치](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 플랫폼](https://developer.android.com/sdk/android-4.0.html)
