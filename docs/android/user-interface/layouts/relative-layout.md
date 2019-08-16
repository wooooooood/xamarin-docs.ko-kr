---
title: Xamarin Android에서 RelativeLayout 사용
description: Xamarin Android 응용 프로그램에서 RelativeLayout를 사용 하는 방법
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: af74ae3c7c87f501bff519bcfa361264205ca3f1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522384"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin Android RelativeLayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)는 자식을 표시 하는입니다. [`ViewGroup`](xref:Android.Views.ViewGroup)[`View`](xref:Android.Views.View)
상대 위치의 요소 의 위치는 형제 [`View`](xref:Android.Views.View) 요소 (예: 지정 된 요소 왼쪽 또는 아래)를 기준으로 지정 하거나에 상대적인 위치에 지정할 수 있습니다.[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
영역 (예: 가운데 왼쪽에 맞춰진).

는 [`RelativeLayout`](xref:Android.Widget.RelativeLayout) [중첩`ViewGroup`](xref:Android.Views.ViewGroup)된를 제거할 수 있으므로 사용자 인터페이스를 디자인 하는 매우 강력한 유틸리티입니다. 여러 중첩 된를 사용 하는 경우[`LinearLayout`](xref:Android.Widget.LinearLayout)
그룹을 단일 [`RelativeLayout`](xref:Android.Widget.RelativeLayout)으로 바꿀 수 있습니다.

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

각 `android:layout_*` 특성 (예 `layout_below`:, `layout_alignParentRight`및 `layout_toLeftOf`)을 확인 합니다.
을 사용 하 [`RelativeLayout`](xref:Android.Widget.RelativeLayout)는 경우 이러한 특성을 사용 하 여 각각 [`View`](xref:Android.Views.View)의 위치를 지정할 수 있습니다. 이러한 특성 중 하나는 서로 다른 종류의 상대 위치를 정의 합니다. 일부 특성은 형제의 [`View`](xref:Android.Views.View) 리소스 ID를 사용 하 여 고유한 상대 위치를 정의 합니다. 예를 들어, 마지막 [`Button`](xref:Android.Widget.Button) 는 ID `ok` (이전 [`Button`](xref:Android.Widget.Button))로 [`View`](xref:Android.Views.View) 식별 되는의 왼쪽 및 오른쪽에 맞추어 정의 됩니다.

사용 가능한 모든 레이아웃 특성은에 [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)정의 되어 있습니다.

다음에서이 레이아웃을 로드 해야 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

메서드 [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*) [`Activity`](xref:Android.App.Activity)는 리소스 ID&mdash;로지정 된에 대 한 레이아웃 파일을 로드 하 고 리소스 **/레이아웃/기본. axml** 레이아웃 파일을 참조 합니다. `Resource.Layout.Main`

애플리케이션을 실행합니다. 다음 레이아웃이 표시 됩니다.

[![TextView, EditText 및 두 개의 단추가 있는 상대 레이아웃의 스크린샷](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>리소스

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 [Creative Commons 2.5 특성 라이선스](http://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다._
