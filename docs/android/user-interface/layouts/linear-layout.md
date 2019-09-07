---
title: Xamarin Android LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
ms.openlocfilehash: 14e9b352a309de94a374b52141e3fd61715d8f75
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764375"
---
# <a name="xamarinandroid-linearlayout"></a>Xamarin Android LinearLayout

[`LinearLayout`](xref:Android.Widget.LinearLayout)는입니다.[`ViewGroup`](xref:Android.Views.ViewGroup)
자식을 표시 합니다.[`View`](xref:Android.Views.View)
가로 또는 세로로 선형 방향의 요소

을 사용 하는 [`LinearLayout`](xref:Android.Widget.LinearLayout)데 주의 해야 합니다.
여러 [`LinearLayout`](xref:Android.Widget.LinearLayout)를 중첩 하려면를 사용 하는 것이 좋습니다.[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
사용합니다.

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

이 XML을 신중 하 게 검사 합니다. 루트가 있습니다.[`LinearLayout`](xref:Android.Widget.LinearLayout)
세로 &ndash; 방향으로 모든 자식 [`View`](xref:Android.Views.View)(2 개 포함)을 정의 하는 것은 세로로 누적 됩니다. 첫 번째 자식은 다른 자식입니다.[`LinearLayout`](xref:Android.Widget.LinearLayout)
가로 방향을 사용 하 고 두 번째 자식은 인 경우[`LinearLayout`](xref:Android.Widget.LinearLayout)
세로 방향을 사용 하는입니다. 이러한 중첩 [`LinearLayout`](xref:Android.Widget.LinearLayout)된 각에는 여러 개의[`TextView`](xref:Android.Widget.TextView)
요소-부모 [`LinearLayout`](xref:Android.Widget.LinearLayout)에 의해 정의 된 방식으로 서로 지향 됩니다.

이제 **HelloLinearLayout.cs** 을 열고 **리소스/레이아웃/Main. axml** 레이아웃을 로드 해야 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[`Activity`](xref:Android.App.Activity) &ndash;)메서드 는 리소스 ID`Resources.Layout.Main` 로 지정 된에 대 한 레이아웃 파일을 로드 하 고 리소스 **/레이아웃/기본. axml** 레이아웃 파일을 참조 합니다. [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)

애플리케이션을 실행합니다. 다음이 표시 됩니다.

[![세로로 정렬 된 app first LinearLayout의 스크린샷, 두 번째 세로](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

XML 특성이 각 뷰의 동작을 정의 하는 방법을 확인 합니다. 에 대해 `android:layout_weight` 다른 값을 시험해 보고 각 요소의 가중치를 기준으로 화면 부동산을 분산 하는 방법을 확인 합니다. 방법에 대 한 자세한 내용은 [일반 레이아웃 개체](https://developer.android.com/guide/topics/ui/declaring-layout.html) 문서를 참조 하세요.[`LinearLayout`](xref:Android.Widget.LinearLayout)
특성을 `android:layout_weight` 처리 합니다.

## <a name="references"></a>참조 항목

- [`LinearLayout`](xref:Android.Widget.LinearLayout)
- [`TextView`](xref:Android.Widget.TextView)

_이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 [Creative Commons 2.5 특성 라이선스](http://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다._
