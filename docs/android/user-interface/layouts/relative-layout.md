---
title: Xamarin Android에서 RelativeLayout 사용
description: Xamarin Android 응용 프로그램에서 RelativeLayout를 사용 하는 방법
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 6cac771f46242cc0475be0a7ec0d475950f4b4e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028803"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin Android RelativeLayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)은 [`View`](xref:Android.Views.View) 자식을 표시하는 [`ViewGroup`](xref:Android.Views.ViewGroup)이다.
상대 위치의 요소 [`View`](xref:Android.Views.View) 위치는 형제 요소 (예: 지정 된 요소 왼쪽 또는 아래)에 상대적으로 지정 하거나에 상대적인 위치에 지정할 수 있습니다 [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
영역 (예: 가운데 왼쪽에 맞춰진).

[`RelativeLayout`](xref:Android.Widget.RelativeLayout) 는 중첩 된 [`ViewGroup`](xref:Android.Views.ViewGroup)를 제거할 수 있으므로 사용자 인터페이스를 디자인 하는 매우 강력한 유틸리티입니다. 여러 중첩 된 [`LinearLayout`](xref:Android.Widget.LinearLayout) 를 사용 하는 경우
그룹을 사용 하면 단일 [`RelativeLayout`](xref:Android.Widget.RelativeLayout)로 바꿀 수 있습니다.

**HelloRelativeLayout**라는 새 프로젝트를 시작 합니다.

**Resources/Layout/Main. axml** 파일을 열고 다음을 삽입 합니다.

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

`layout_below`, `layout_alignParentRight`, `layout_toLeftOf`등의 각 `android:layout_*` 특성을 확인 합니다.
[`RelativeLayout`](xref:Android.Widget.RelativeLayout)를 사용 하는 경우 이러한 특성을 사용 하 여 각 [`View`](xref:Android.Views.View)의 위치를 지정할 수 있습니다. 이러한 특성 중 하나는 서로 다른 종류의 상대 위치를 정의 합니다. 일부 특성은 형제 [`View`](xref:Android.Views.View) 의 리소스 ID를 사용 하 여 자체 상대 위치를 정의 합니다. 예를 들어 마지막 [`Button`](xref:Android.Widget.Button) 는 ID `ok` (이전 [`Button`](xref:Android.Widget.Button))로 식별 되는 [`View`](xref:Android.Views.View) 의 왼쪽 및 정렬 된의 상단에 배치 되도록 정의 됩니다.

사용 가능한 모든 레이아웃 특성은 [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)에서 정의 됩니다.

[`OnCreate()`](xref:Android.App.Activity.OnCreate*) 에서이 레이아웃을 로드 해야 합니다.
방법이

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*) 메서드는 리소스 ID &mdash;으로 지정 된 [`Activity`](xref:Android.App.Activity)에 대 한 레이아웃 파일을 로드 `Resource.Layout.Main`는 **Resources/layout/Main. axml** 레이아웃 파일을 참조 합니다.

애플리케이션을 실행합니다. 다음 레이아웃이 표시 됩니다.

[TextView, EditText 및 두 개의 단추가 있는 상대 레이아웃의 스크린샷![](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>리소스

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 [Creative Commons 2.5 특성 라이선스](https://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다._
