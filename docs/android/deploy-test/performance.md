---
title: Xamarin.Android Performance
description: Xamarin.Android로 빌드된 응용 프로그램의 성능을 높이기 위한 많은 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 응용 프로그램에서 소비하는 메모리의 양을 크게 줄일 수 있습니다. 이 문서에서는 이러한 기술에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 10e45ec438f1e698a9f09223cecea5934de54da8
ms.sourcegitcommit: 6be6374664cd96a7d924c2e0c37aeec4adf8be13
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51617716"
---
# <a name="xamarinandroid-performance"></a>Xamarin.Android 성능

_Xamarin.Android로 빌드된 응용 프로그램의 성능을 높이기 위한 많은 기술이 있습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 응용 프로그램에서 소비하는 메모리의 양을 크게 줄일 수 있습니다. 이 문서에서는 이러한 기술에 대해 설명합니다._

## <a name="performance-overview"></a>성능 개요

낮은 응용 프로그램 성능은 여러 가지 방법으로 나타납니다. 이 경우에 응용 프로그램이 응답하지 않는 것처럼 보이고, 스크롤 속도가 느려지고, 배터리 수명이 줄어들 수 있습니다. 그러나 성능을 최적화하려면 효율적인 코드를 구현하는 것 이상이 필요합니다. 응용 프로그램 성능에 대한 사용자 환경도 고려해야 합니다. 예를 들어 사용자가 다른 활동을 수행하지 못하도록 차단하지 않고 작업을 실행하면 사용자 환경을 향상시키는 데 도움이 될 수 있습니다.

Xamarin.Android로 빌드된 응용 프로그램의 성능과 인식 성능을 높이는 여러 가지 기술이 있습니다. 다음과 같은 변경 내용이 해당됩니다.

- [레이아웃 계층 최적화](#optimizelayout)
- [목록 보기 최적화](#optimizelistviews)
- [작업에서 이벤트 처리기 제거](#removeeventhandlers)
- [서비스의 수명 제한](#limitservices)
- [알림을 받을 때 리소스 릴리스](#releaseresources)
- [사용자 인터페이스가 숨겨질 때 리소스 릴리스](#releaseresourcesuihidden)
- [이미지 리소스 최적화](#optimizeimages)
- [사용되지 않는 이미지 리소스 삭제](#disposeimages)
- [부동 소수점 연산 방지](#avoidfloats)
- [대화 상자 해제](#dismissdialogs)


> [!NOTE]
> 이 문서를 읽기 전에 먼저 Xamarin 플랫폼을 사용하여 빌드된 응용 프로그램의 메모리 사용 및 성능을 향상시키기 위한 비플랫폼 특정 기술에 대해 설명하는 [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)을 참조해야 합니다.

<a name="optimizelayout" />

## <a name="optimize-layout-hierarchies"></a>레이아웃 계층 최적화

응용 프로그램에 추가된 각 레이아웃에는 초기화, 레이아웃 및 그리기가 필요합니다. 각 자식을 두 번 측정하기 때문에 `weight` 매개 변수를 사용하는 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 인스턴스를 중첩할 때 레이아웃 단계는 비용이 많이 들 수 있습니다. `LinearLayout`의 중첩된 인스턴스를 사용하면 심층 보기 계층 구조가 발생할 수 있습니다. 그러면 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)와 같이 여러 번 팽창된 레이아웃의 성능이 저하될 수 있습니다. 따라서 성능이 증가하도록 이러한 레이아웃을 최적화해야 합니다.

예를 들어 아이콘, 제목 및 설명을 포함하는 목록 보기 행에 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)을 사용합니다. `LinearLayout`에는 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) 및 두 개의 [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 인스턴스를 포함하는 `LinearLayout`가 포함됩니다.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

이 레이아웃은 3단계로 구성되며 각 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) 행을 팽창할 때 불필요입니다. 그러나 다음 코드 예제에 표시된 대로 레이아웃을 병합하여 개선할 수 있습니다.

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

이전 3단계 계층 구조는 2단계 계층 구조로 축소되었으며 단일 [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)은 두 개의 [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 인스턴스로 대체되었습니다. 각 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) 행의 레이아웃을 팽창할 때 상당한 성능 향상을 얻을 수 있습니다.

<a name="optimizelistviews" />

## <a name="optimize-list-views"></a>목록 보기 최적화

사용자에게는 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) 인스턴스에 대한 부드러운 스크롤 및 빠른 로드 시간이필요합니다. 그러나 각 목록 보기 행에 깊게 중첩된 보기 계층 구조나 복잡한 레이아웃이 포함되어 있으면 스크롤 성능이 저하될 수 있습니다. 그러나 `ListView` 성능이 저하되지 않도록 방지하는 데 사용할 수 있는 방법이 있습니다.

- 행 보기 재사용 자세한 내용은 [행 보기 재사용](#reuserowviews)을 참조하세요.
- 가능한 경우 레이아웃을 평면화합니다.
- 웹 서비스에서 검색된 행 콘텐츠를 캐시합니다.
- 이미지 크기 조정을 방지합니다.

이러한 기술은 전체적으로 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/) 인스턴스 스크롤을 부드럽게 유지하는 데 도움이 될 수 있습니다.

<a name="reuserowviews" />

### <a name="reuse-row-views"></a>행 보기 재사용

[`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)에 수백 개의 행을 표시하는 경우 작은 수의 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/) 개체만 한 번에 화면에 표시할 때 이 개체를 수백 개 만드는 것은 메모리 낭비입니다. 대신, 화면에 표시되는 `View` 개체만 메모리에 로드될 수 있으며, 이 경우 **콘텐츠**가 다시 사용된 개체에 로드됩니다. 이렇게 하면 수백 개의 추가 개체를 인스턴스화하지 않으므로 시간과 메모리가 절약됩니다.

따라서 행이 화면에서 사라지면 다음 코드 예제와 같이 해당 보기를 큐에 배치하여 다시 사용할 수 있습니다.

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

사용자가 스크롤하면 [`ListView`](https://developer.xamarin.com/api/type/Android.Widget.ListView/)은 `GetView` 재정의를 호출하여 표시할 새 보기를 요청합니다. 가능하면 `convertView` 매개 변수에서 사용되지 않은 보기를 전달합니다. 이 값이 `null`인 경우 코드에서는 새 [`View`](https://developer.xamarin.com/api/type/Android.Views.View/) 인스턴스를 만들고, 그렇지 않으면 `convertView` 속성을 다시 설정하고 다시 사용합니다.

자세한 내용은 [데이터로 ListView 채우기](~/android/user-interface/layouts/list-view/populating.md)에서 [행 보기 재사용](~/android/user-interface/layouts/list-view/populating.md#row-view-re-use)을 참조하세요.

<a name="removeeventhandlers" />

## <a name="remove-event-handlers-in-activities"></a>작업에서 이벤트 처리기 제거

작업이 Android 런타임에서 소멸될 때 Mono 런타임에서 살아있을 수 있습니다. 따라서 `Activity.OnPause`에서 외부 개체에 대한 이벤트 처리기를 제거하여 런타임이 소멸된 작업에 대한 참조를 유지하지 않도록 설정합니다.

작업에서는 클래스 수준의 이벤트 처리기를 선언합니다.

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

그러면 작업(예: `OnResume`)에서 처리기를 구현합니다.

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

작업이 실행 상태를 종료할 때 `OnPause`가 호출됩니다. `OnPause` 구현에서 다음과 같이 처리기를 제거합니다.

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices" />

## <a name="limit-the-lifespan-of-services"></a>서비스의 수명 제한

서비스가 시작되면 Android는 서비스가 프로세스를 실행하도록 유지합니다. 이렇게 하면 메모리를 페이징하거나 다른 곳에서 사용할 수 없기 때문에 비용이 많이 듭니다. 서비스가 필요하지 않을 때에도 실행하므로 메모리 제약 조건으로 인해 응용 프로그램의 성능이 저하될 위험이 증가합니다. Android가 캐시할 수 있는 프로세스 수가 감소하므로 응용 프로그램의 효율성이 저하될 수 있습니다.

`IntentService`를 사용하여 서비스의 수명을 제한할 수 있습니다. 이 설정은 서비스를 시작하려고 할 때 종료됩니다.

<a name="releaseresources" />

## <a name="release-resources-when-notified"></a>알림을 받을 때 리소스 릴리스

응용 프로그램 수명 주기 동안 디바이스 메모리가 부족한 경우 [`OnTrimMemory`](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) 콜백은 알림을 제공합니다. 이 콜백을 구현하여 다음 메모리 수준 알림을 수신해야 합니다.

- [`TrimMemoryRunningModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate/) – 응용 프로그램이 필요없는 일부 리소스를 릴리스하려고 *할 수 있습니다*.
- [`TrimMemoryRunningLow`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningLow/) – 응용 프로그램이 필요없는 리소스를 릴리스*해야 합니다*.
- [`TrimMemoryRunningCritical`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical/) - 응용 프로그램이 중요하지 않은 프로세스를 가능한 릴리스*해야 합니다*.

또한 응용 프로그램 프로세스를 캐시할 때 다음과 같은 메모리 수준 알림을 [`OnTrimMemory`](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) 콜백에서 받을 수 있습니다.

- [`TrimMemoryBackground`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryBackground/) – 사용자가 앱에 반환하는 경우 빠르고 효율적으로 다시 빌드할 수 있는 리소스를 릴리스합니다.
- [`TrimMemoryModerate`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryModerate/) - 리소스를 해제하면 전반적인 성능 향상을 위해 시스템이 다른 프로세스를 캐시할 수 있습니다.
- [`TrimMemoryComplete`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryComplete/) - 추가 메모리가 곧 복구되지 않는 경우 응용 프로그램 프로세스가 곧 종료됩니다.

수신된 수준에 따라 리소스를 릴리스하여 알림에 응답해야 합니다.

<a name="releaseresourcesuihidden" />

## <a name="release-resources-when-the-user-interface-is-hidden"></a>사용자 인터페이스가 숨겨질 때 리소스 릴리스

캐시된 프로세스에 대한 Android의 용량을 크게 높일 수 있으므로 사용자가 다른 앱으로 이동하면 앱의 사용자 인터페이스에서 사용하는 리소스를 릴리스합니다. 그러면 사용자 환경 품질에 영향을 줄 수 있습니다.

사용자가 UI를 종료할 때 알림을 받으려면 `Activity` 클래스에서 [`OnTrimMemory`](https://developer.xamarin.com/api/member/Android.App.Activity.OnTrimMemory/p/Android.Content.TrimMemory/) 콜백을 구현하고 [`TrimMemoryUiHidden`](https://developer.xamarin.com/api/field/Android.Content.ComponentCallbacks2.TrimMemoryUiHidden/) 레벨을 수신 대기합니다. 이는 UI가 보기에서 숨겨진다는 것을 나타냅니다. *모든* 응용 프로그램의 UI 구성 요소가 사용자로부터 숨겨질 때에만 이 알림이 수신됩니다. 이 알림을 수신할 때 UI 리소스를 릴리스하면 사용자가 앱의 다른 작업에서 다시 탐색하는 경우 UI 리소스를 사용하여 작업을 다시 신속하게 시작할 수 있습니다.

<a name="optimizeimages" />

## <a name="optimize-image-resources"></a>이미지 리소스 최적화

이미지는 응용 프로그램에서 사용하는 가장 비싼 리소스 중 일부이며, 고해상도로 캡처되는 경우가 많습니다. 따라서 이미지를 표시할 때 디바이스 화면에 필요한 해상도로 표시합니다. 이미지의 해상도가 화면보다 더 높은 경우 규모를 축소해야 합니다.

자세한 내용은 [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md) 가이드의 [이미지 리소스 최적화](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)를 참조하세요.

<a name="disposeimages" />

## <a name="dispose-of-unused-image-resources"></a>사용되지 않는 이미지 리소스 삭제

메모리 사용을 절약하려면 더 이상 필요 없는 큰 이미지 리소스를 삭제하는 것이 좋습니다. 그러나 이미지를 제대로 삭제해야 합니다. 명시적 `.Dispose()` 호출을 사용하는 대신 [using](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/using-statement) 문을 활용하여 `IDisposable` 개체를 바르게 사용하도록 할 수 있습니다. 

예를 들어 [비트맵](https://developer.xamarin.com/api/type/Android.Graphics.Bitmap/) 클래스는 `IDisposable`을 구현합니다. `using` 블록에서 `BitMap` 개체의 인스턴스화를 래핑하면 블록에서 종로 시 올바르게 삭제되도록 합니다.

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

삭제 가능한 리소스를 릴리스하는 방법에 대한 자세한 내용은 [IDisposable 리소스 릴리스](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable)를 참조하세요.  


<a name="avoidfloats" />

## <a name="avoid-floating-point-arithmetic"></a>부동 소수점 연산 방지

Android 디바이스에서 부동 소수점 연산은 정수 연산보다 2배 정도 느립니다. 따라서 가능하면 부동 소수점 연산을 정수 연산으로 바꿉니다. 그러나 최근 하드웨어에서 `float`와 `double` 연산 사이에 실행 시간이 다르지 않습니다.

> [!NOTE]
> 정수 연산에 대해서도 일부 CPU는 하드웨어 나누기 기능이 부족합니다. 따라서 정수 나누기 및 모듈러스 연산은 소프트웨어에서 자주 수행됩니다.

<a name="dismissdialogs" />

## <a name="dismiss-dialogs"></a>대화 상자 해제

[`ProgressDialog`](https://developer.xamarin.com/api/type/Android.App.ProgressDialog/) 클래스(또는 대화 상자 또는 경고)를 사용하는 경우 대화 상자의 목적이 완료되면 [`Hide`](https://developer.xamarin.com/api/member/Android.App.Dialog.Hide()/) 메서드를 호출하는 대신 [`Dismiss`](https://developer.xamarin.com/api/member/Android.App.Dialog.Dismiss()/) 메서드를 호출합니다. 그렇지 않으면 대화 상자가 계속 활성 상태이며 이에 대한 참조를 보유하여 작업이 누출됩니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Android로 빌드된 응용 프로그램의 성능을 높이는 기술에 대해 설명했습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 응용 프로그램에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.


## <a name="related-links"></a>관련 링크

- [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)
