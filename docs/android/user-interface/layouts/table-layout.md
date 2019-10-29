---
title: Xamarin Android TableLayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 4175b1fa62b2bc0e4209d13934c2bdbdd1e2a085
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028743"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin Android TableLayout

[`TableLayout`](xref:Android.Widget.TableLayout) [`ViewGroup`](xref:Android.Views.ViewGroup)
자식 [`View`](xref:Android.Views.View) 를 표시 합니다.
행 및 열의 요소

**HelloTableLayout**라는 새 프로젝트를 시작 합니다.

**Resources/Layout/Main. axml** 파일을 열고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

이것은 HTML 테이블의 구조와 유사 합니다. [`TableLayout`](xref:Android.Widget.TableLayout)
요소가 HTML `<table>` 요소와 유사 합니다. [`TableRow`](xref:Android.Widget.TableRow)
는 `<tr>` 요소와 유사 합니다. 그러나 셀의 경우 모든 종류의 [`View`](xref:Android.Views.View) 요소를 사용할 수 있습니다. 이 예제에서는 [`TextView`](xref:Android.Widget.TextView)
각 셀에 사용 됩니다. 일부 행에는 가로 선을 그리는 데 사용 되는 기본 [`View`](xref:Android.Views.View)있습니다.

**HelloTableLayout** 활동이 [`OnCreate()`](xref:Android.App.Activity.OnCreate*) 에서이 레이아웃을 로드 하는지 확인 합니다.
방법이

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)) 메서드는 리소스 ID &mdash;으로 지정 된 [`Activity`](xref:Android.App.Activity)에 대 한 레이아웃 파일을 로드 `Resource.Layout.Main`는 **Resources/layout/Main. axml** 레이아웃 파일을 참조 합니다.

애플리케이션을 실행합니다. 다음이 표시 됩니다.

[![여러 테이블 행을 표시 하는 TableLayout 앱의 예제 스크린샷](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)

## <a name="references"></a>참조 항목

- [`TableLayout`](xref:Android.Widget.TableLayout)
- [`TableRow`](xref:Android.Widget.TableRow)
- [`TextView`](xref:Android.Widget.TextView)

_이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 [Creative Commons 2.5 특성 라이선스](https://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다._
