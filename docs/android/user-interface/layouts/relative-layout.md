---
title: RelativeLayout Xamarin.Android에서 사용 하 여
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: cd2d7537036978e30c97b5776155e429178b6dac
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436000"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) 이 [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) 자식을 표시 하는 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 상대적 위치에 있는 요소입니다. 위치는 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 형제 요소 (예:의 왼쪽에 대 한 또는 지정된 된 요소 아래)에 상대적으로 지정 하거나 배치에 대 한 상대는 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) 영역 (예 아래쪽 맞춤, 센터의 왼쪽).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) 중첩 제거할 수 있으므로 사용자 인터페이스 디자인에 대해는 매우 강력한 유틸리티 [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s입니다. 생각 되 면 몇 가지를 사용 하 여 중첩 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 그룹 수 있습니다는 단일으로 바꾸려면 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)합니다.

라는 새 프로젝트를 시작 **HelloRelativeLayout**합니다.

열기는 **Resources/Layout/Main.axml** 파일을 다음에 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

각각의 표시는 `android:layout_*` 같은 특성 `layout_below`, `layout_alignParentRight`, 및 `layout_toLeftOf`합니다.
사용 하는 경우는 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), 이러한 특성을 사용 하 여 각 위치를 지정 하려는 방법에 대해 설명 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)합니다. 이러한 특성 중 하나를 각 다른 종류의 상대 위치를 정의 합니다. 형제의 리소스 ID를 사용 하 여 일부 특성 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 상대 위치를 정의할 수 있습니다. 예를 들어 마지막 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 왼쪽의 및 정렬 된-the-위쪽-의 배치 되도록 정의 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) ID로 식별 된 `ok` (이전 는[`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

에 정의 된 모든 사용 가능한 레이아웃 특성 [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)합니다.

이 레이아웃에 로드 해야는 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 메서드:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) 메서드 로드에 대 한 레이아웃 파일은 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)리소스 ID에 지정 된 &mdash; `Resource.Layout.Main` 참조 하는 **리소스/레이아웃 / Main.axml** 레이아웃 파일.

응용 프로그램을 실행합니다. 다음과 같은 레이아웃을 표시 되어야 합니다.

[![TextView, EditText, 두 개의 단추와 상대적 레이아웃의 스크린 샷](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>자료

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/).
