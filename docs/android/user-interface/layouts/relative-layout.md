---
title: Xamarin.Android에는 RelativeLayout를 사용 하 여
description: RelativeLayout Xamarin.Android 응용 프로그램을 사용 하는 방법
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/29/2018
ms.openlocfilehash: af8d37775a798fc6019106a66df75843a951c108
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403418"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) 표시 하는 자식 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 상대 위치에 요소입니다. 위치를 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 등의 왼쪽에 대 한 또는 지정된 된 요소의 다음 형제 요소를 기준으로 지정 하거나의 기준으로 배치 합니다 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) 영역 (예: 아래쪽 맞춤, center의 왼쪽).

A [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) 중첩 제거할 수 있으므로 사용자 인터페이스 디자인에 매우 강력한 유틸리티 [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s입니다. 한다면 몇 가지를 사용 하 여 중첩 [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 그룹을 수 있습니다 단일 바꾸거나 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)합니다.

명명 된 새 프로젝트를 시작 **HelloRelativeLayout**합니다.

엽니다는 **Resources/Layout/Main.axml** 파일과 다음 삽입:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
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

각 확인 합니다 `android:layout_*` 같은 특성 `layout_below`, `layout_alignParentRight`, 및 `layout_toLeftOf`합니다.
사용 하는 경우는 [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)에서 이러한 특성을 사용 하 여 각 배치 하고자 하는 방법을 설명 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)합니다. 이러한 특성의 각각 다른 종류의 상대 위치를 정의 합니다. 형제의 리소스 ID를 사용 하는 일부 특성 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 자체 상대 위치를 정의 합니다. 예를 들어, 마지막 [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) 를 왼쪽의 정렬 된-the-위쪽-의 사이에 정의 된 합니다 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) ID로 식별 `ok` (이전 는[`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

에 정의 된 모든 사용 가능한 레이아웃 특성 [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)합니다.

이 레이아웃을 로드 해야 합니다 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 메서드:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

합니다 [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) 에 대 한 레이아웃 파일을 로드 하는 메서드를 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)리소스 ID 기준으로 지정 된 &mdash; `Resource.Layout.Main` 가리킵니다는 **리소스/레이아웃 / Main.axml** 레이아웃 파일입니다.

응용 프로그램을 실행합니다. 다음 레이아웃을 표시 됩니다.

[![TextView를, EditText, 및 두 개의 단추를 사용 하 여 상대 레이아웃의 스크린 샷](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>자료

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).
