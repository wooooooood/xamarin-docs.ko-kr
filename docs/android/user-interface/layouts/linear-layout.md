---
title: LinearLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: fc6bc9e1d4625f8f45887b0a144a31383046b296
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 이 [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) 자식을 표시 하는 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 선형 방향의 요소 세로 또는 가로로 합니다.

과도 하 게 사용에 대 한 주의 해야는 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)합니다.
여러 중첩을 시작 하면 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)를 만들려는 경우 사용 하는 것이 좋습니다는 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) 대신 합니다.

라는 새 프로젝트를 시작 **HelloLinearLayout**합니다.

열기 **Resources/Layout/Main.axml** 하 고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "fill_parent"
      android:layout_height=    "fill_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

이 XML을 신중 하 게 검사 합니다. 루트 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 세로 되도록 해당 방향을 정의 하는 &ndash; 모든 자식 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)(하는 중에 두 개의) s 세로로 누적 됩니다. 첫 번째 자식은 또 다른 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 가로 방향을 사용 하 고 두 번째 자식은 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 세로 방향을 사용 하 여 합니다. 이러한 각 중첩 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s 포함 여러 [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 요소는 해당 부모에 의해 정의 된 방식으로 서로 중심 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

이제 열 **HelloLinearLayout.cs** 로드 해야 하 고는 **Resources/Layout/Main.axml** 레이아웃에는 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 메서드:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) 메서드 로드에 대 한 레이아웃 파일은 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)리소스 ID에 지정 된 &ndash; `Resources.Layout.Main` 참조 하는 **리소스/레이아웃 / Main.axml** 레이아웃 파일.

응용 프로그램을 실행합니다. 다음을 참조 합니다.

[![스크린샷 첫 번째 LinearLayout 가로로 정렬 하는 앱의 두 번째 세로](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML 특성은 각 보기의 동작을 정의 하는 방법을 확인 합니다. 에 대해 다른 값을 사용 하 여 실험해 `android:layout_weight` 가중치의 각 요소에 따라 화면 자원을 분산 하는 방법을 볼 수 있습니다. 참조는 [공통 개체의 레이아웃](http://developer.android.com/guide/topics/ui/declaring-layout.html) 방법에 대 한 자세한 문서 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 핸들은 `android:layout_weight` 특성입니다.


## <a name="references"></a>참조

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/).

