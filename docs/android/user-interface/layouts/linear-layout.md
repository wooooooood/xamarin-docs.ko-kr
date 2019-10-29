---
title: Xamarin Android LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/07/2018
ms.openlocfilehash: b1cff01c66ae2581a68286e62bd8c8c5fb7f9d72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028951"
---
# <a name="xamarinandroid-linearlayout"></a>Xamarin Android LinearLayout

[`LinearLayout`](xref:Android.Widget.LinearLayout) [`ViewGroup`](xref:Android.Views.ViewGroup)
자식 [`View`](xref:Android.Views.View) 를 표시 합니다.
가로 또는 세로로 선형 방향의 요소

[`LinearLayout`](xref:Android.Widget.LinearLayout)를 과도 하 게 사용 하는 데 주의 해야 합니다.
여러 [`LinearLayout`](xref:Android.Widget.LinearLayout)s를 중첩 하기 시작 하는 경우 [`RelativeLayout`](xref:Android.Widget.RelativeLayout) 사용을 고려할 수 있습니다.
대신.

**HelloLinearLayout**라는 새 프로젝트를 시작 합니다.

**리소스/레이아웃/기본. axml** 을 열고 다음을 삽입 합니다.

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

이 XML을 신중 하 게 검사 합니다. 루트 [`LinearLayout`](xref:Android.Widget.LinearLayout)
세로 방향으로 해당 방향을 정의 하는 &ndash; 모든 자식 [`View`](xref:Android.Views.View)s (2 개 포함)는 세로로 누적 됩니다. 첫 번째 자식은 다른 [`LinearLayout`](xref:Android.Widget.LinearLayout)
가로 방향을 사용 하 고 두 번째 자식은 [`LinearLayout`](xref:Android.Widget.LinearLayout)
세로 방향을 사용 하는입니다. 이러한 각각의 중첩 된 [`LinearLayout`](xref:Android.Widget.LinearLayout)s에는 여러 [`TextView`](xref:Android.Widget.TextView) 포함 되어 있습니다.
요소는 부모 [`LinearLayout`](xref:Android.Widget.LinearLayout)에서 정의 된 방식으로 서로 지향 됩니다.

이제 **HelloLinearLayout.cs** 을 열고 **리소스/레이아웃/주. axml** [`OnCreate()`](xref:Android.App.Activity.OnCreate*) 레이아웃을 로드 하도록 합니다.
방법이

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)) 메서드는 리소스 ID &ndash;으로 지정 된 [`Activity`](xref:Android.App.Activity)에 대 한 레이아웃 파일을 로드 `Resources.Layout.Main`는 **Resources/layout/Main. axml** 레이아웃 파일을 참조 합니다.

애플리케이션을 실행합니다. 다음이 표시 됩니다.

[세로로 정렬 된 앱의![스크린샷, 세로로 두 번째 LinearLayout](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML 특성이 각 뷰의 동작을 정의 하는 방법을 확인 합니다. `android:layout_weight`에 대 한 다양 한 값을 시험해 보고 각 요소의 가중치를 기준으로 화면 부동산을 분산 하는 방법을 확인 합니다. 방법에 대 한 자세한 내용은 [공용 레이아웃 개체](https://developer.android.com/guide/topics/ui/declaring-layout.html) 문서를 참조 하세요 [`LinearLayout`](xref:Android.Widget.LinearLayout)
`android:layout_weight` 특성을 처리 합니다.

## <a name="references"></a>참조 항목

- [`LinearLayout`](xref:Android.Widget.LinearLayout)
- [`TextView`](xref:Android.Widget.TextView)

_이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 [Creative Commons 2.5 특성 라이선스](https://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다._
