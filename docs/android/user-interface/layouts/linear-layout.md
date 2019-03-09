---
title: LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
ms.openlocfilehash: f3d0394f6b2388918f728bd5a25e9e809a832ca6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670976"
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 가 [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
자식 표시 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
선형 방향으로 요소 가로 또는 세로로 합니다.

과도 하 게 사용 하 여 주의 해야 합니다 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)합니다.
여러 중첩을 시작 하면 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)사용을 고려 하려는 s는 [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
사용합니다.

명명 된 새 프로젝트를 시작 **HelloLinearLayout**합니다.

오픈 **Resources/Layout/Main.axml** 하 고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "match_parent"
      android:layout_height=    "match_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

이 XML을 신중 하 게 검사 합니다. 루트 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
정의 하는 해당 방향을 세로 &ndash; 모든 자식 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)(자신이의 두 개)가 세로로 누적 됩니다. 첫 번째 자식은 또 다른 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
가로 방향을 사용 하 고 두 번째 자식을 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
사용 하는 세로 방향. 이러한 각 중첩 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s 몇 가지를 포함 합니다. [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
요소는 해당 부모에서 정의 된 방식으로 서로 지향 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)합니다.

이제 열 **HelloLinearLayout.cs** 로드 해야 합니다 **Resources/Layout/Main.axml** 레이아웃에는 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
방법:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

합니다 [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) 에 대 한 레이아웃 파일을 로드 하는 메서드를 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)리소스 ID 기준으로 지정 된 &ndash; `Resources.Layout.Main` 가리킵니다는 **리소스/레이아웃 / Main.axml** 레이아웃 파일입니다.

애플리케이션을 실행합니다. 다음을 참조 해야 합니다.

[![스크린 샷 첫 번째 LinearLayout 가로로 정렬 하는 앱의 두 번째 세로 방향으로](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML 특성은 각 보기의 동작을 정의 하는 방법을 확인할 수 있습니다. 에 대해 다른 값을 사용 하 여 실험해 `android:layout_weight` 각 요소의 가중치에 따라 화면 부동산을 분산 하는 방법을 확인 합니다. 참조 된 [일반적인 레이아웃 개체](https://developer.android.com/guide/topics/ui/declaring-layout.html) 하는 방법에 대 한 자세한 문서 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
핸들을 `android:layout_weight` 특성입니다.


## <a name="references"></a>참조

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).

