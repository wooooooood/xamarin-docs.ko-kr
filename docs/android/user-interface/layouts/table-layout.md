---
title: TableLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 80147de0f639b4b597e11b41a6854f550edd61a9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 이 [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) 자식을 표시 하는 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 행과 열에 있는 요소입니다.

라는 새 프로젝트를 시작 **HelloTableLayout**합니다.

열기는 **Resources/Layout/Main.axml** 파일을 다음에 삽입 합니다.

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

확인 방법의 HTML 테이블 구조와 유사 합니다. [ `TableLayout` ](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 요소는 HTML 같은 `<table>` 요소 [ `TableRow` ](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 비슷합니다는 `<tr>` 요소로 셀을 사용할 수 있습니다 모든 종류의 있지만 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 요소입니다. 이 예제는 [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 각 셀에 대해 사용 됩니다. 일부 행 사이의 이기도 기본 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/), 가로 선을 그리는 데 사용 되는 합니다.

있는지 확인 하면 **HelloTableLayout** 작업 로드에이 레이아웃의 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 메서드:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) 메서드 로드에 대 한 레이아웃 파일은 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)리소스 ID에 지정 된 &mdash; `Resource.Layout.Main` 참조 하는 **리소스/레이아웃 / Main.axml** 레이아웃 파일.

응용 프로그램을 실행합니다. 다음을 참조 합니다.

[![여러 테이블 행을 표시 하는 TableLayout 응용 프로그램의 예제 스크린 샷](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png)


<a name="References" />

## <a name="references"></a>참조

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/).
