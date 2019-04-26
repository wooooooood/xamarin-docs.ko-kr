---
title: TableLayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 25b2393d2133c0a1f3f8354584c276fcd7ddaa4b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61305160"
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 가 [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
자식 표시 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
행 및 열에 대 한 요소입니다.

명명 된 새 프로젝트를 시작 **HelloTableLayout**합니다.

엽니다는 **Resources/Layout/Main.axml** 파일과 다음 삽입:

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

어떻게 HTML 테이블의 구조와 유사 알 수 있습니다. 는 [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/)
요소는 HTML과 같은 `<table>` 요소 [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/)
비슷합니다는 `<tr>` 요소로 셀을 사용할 수 있습니다 모든 종류의 하지만 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 요소입니다. 이 예는 [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
각 셀에 사용 됩니다. 일부 행 사이 이기도 기본 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)는 가로줄을 그리기 하는 데 사용 됩니다.

있는지 확인 하 **HelloTableLayout** 활동에서이 레이아웃을 로드 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
방법:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

합니다 [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) 에 대 한 레이아웃 파일을 로드 하는 메서드를 [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/)리소스 ID 기준으로 지정 된 &mdash; `Resource.Layout.Main` 가리킵니다는 **리소스/레이아웃 / Main.axml** 레이아웃 파일입니다.

애플리케이션을 실행합니다. 다음을 참조 해야 합니다.

[![여러 테이블 행을 표시 하는 TableLayout 앱의 스크린샷 예제](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## <a name="references"></a>참조

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).
