---
title: GridView
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2fb3133833dbaa0b174c4611d204f6c8ceb42a2b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 이 [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) 스크롤할 수 있는 2 차원 표에 항목을 표시 하 합니다. 모눈 항목을 사용 하 여 레이아웃을 자동으로 삽입 되는 [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/)합니다.

이 자습서에서는 포함 된 표를 축소판 이미지를 만듭니다. 항목을 선택 하면 알림 메시지에는 이미지의 위치를 표시 됩니다.

라는 새 프로젝트를 시작 **HelloGridView**합니다.

를 사용 하려면 원하는 일부 사진 찾기 또는 [이러한 샘플 이미지 다운로드](http://developer.android.com/shareables/sample_images.zip)합니다. 프로젝트에 이미지 파일 추가 **리소스/Drawable** 디렉터리입니다. 에 **속성** 창에 각각에 대 한 빌드 작업 설정 **AndroidResource**합니다.

열기는 **Resources/Layout/Main.axml** 파일을 다음에 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

이 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 전체 화면을 채웁니다. 특성은 대신 자체 설명 합니다. 유효한 특성에 대 한 자세한 내용은 참조는 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 참조 합니다.

열기 `HelloGridView.cs` 에 대 한 다음 코드를 삽입 하 고는 [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) 메서드:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

후의 **Main.axml** 레이아웃 콘텐츠 보기에 대해 설정 되어는 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 사용 하 여 레이아웃에서 캡처된 [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/)합니다. [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) 속성은 다음 사용자 지정 어댑터를 설정 하는 데 사용 됩니다 (`ImageAdapter`) 모눈에 표시할 모든 항목에 대 한 소스로 합니다. `ImageAdapter` 다음 단계에서 생성 됩니다.

작업을 수행할 눈금에 항목을 클릭할 때, 익명 대리자가 구독 하는 [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) 이벤트입니다.
표시는 [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) 선택한 항목의 인덱스 위치 (0부터 시작)를 표시 하는 (에서 실제 시나리오에서는 위치 사용할 수도 일부 다른 작업에 대 한 전체 크기의 이미지를 가져오려는). 참고 Java 스타일 수신기 클래스.NET 이벤트 대신 사용할 수 있습니다.

라는 새 클래스를 만들고 `ImageAdapter` 서브클래싱하는 [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

    public override Java.Lang.Object GetItem (int position)
    {
        return null;
    }

    public override long GetItemId (int position)
    {
        return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

상속 되며, 일부 필요한 메서드를 구현 하이 먼저 [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)합니다. 생성자 및 [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) 속성은 설명이 필요 없습니다. 일반적으로 [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/) 어댑터에서 지정된 된 위치에 실제 개체를 반환 해야 하지만이 예제에 대 한 무시 됩니다. 마찬가지로, [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/) 항목의 행 id를 반환 해야 하지만 여기 필요 하지 않습니다.

첫 번째 방법은 필요한 [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)합니다.
이 메서드가 만드는 새 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 에 추가 하는 각 이미지는 `ImageAdapter`합니다. 이 메서드가 호출 되는 경우는 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 전달는 일반적으로 개체를 재활용된 (적어도이 호출 된 후에 한 번) 개체가 null 인지 확인 하는 검사 되어 있으므로 합니다. 경우 것 *은* null는 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) 인스턴스화되고 이미지 표시에 대 한 원하는 속성을 사용 하 여 구성:

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) 뷰에 대 한 높이 너비를 설정&mdash;는 그릴의 크기에 관계 없이 각 이미지는 크기가 조정를 적절 하 게 이러한 크기에 맞게 잘릴 이렇게 합니다.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) 이미지 (필요한 경우)의 중심 쪽 자를 해야 선언 합니다.

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) 모든 면에 대해 안쪽 여백을 정의합니다. (즉, 이미지의 서로 다른 가로 세로 비율 있으면 다음 덜 패딩 발생 합니다는 ImageView에 지정 된 차원과 일치 하지 않는 경우 이미지의 자세한 자르기 note).

경우는 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 에 전달 된 [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) 은 *하지* null, 다음의 로컬 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) 사용 하 여 초기화 재활용 [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) 개체입니다.

끝에는 [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) 메서드는 `position` 메서드에 전달 된 정수는에서 이미지를 선택 하는 데 사용 되는 `thumbIds` 배열에 대 한 이미지 리소스로 설정 된는 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

즉 정의 하는 것은 `thumbIds` 그릴 수 있는 리소스의 배열입니다.

응용 프로그램을 실행합니다. 모눈 레이아웃에는 다음과 같이 표시 됩니다.

[![15 이미지를 표시 하는 GridView의 예제 스크린 샷](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

동작을 사용 하 여 실험해는 [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 및 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) 해당 속성을 조정 하 여 요소입니다. 예를 들어, 사용 하는 대신 [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) 사용해 [ `SetAdjustViewBounds()` ](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/)합니다.


## <a name="references"></a>참조

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*이 페이지의 일부는 Android 열려 있는 소스 프로젝트에서 공유 하 고 만들고에 설명 된 조건에 따라 사용 작업에 따라 수정 된*
[*Creative Commons 2.5 Attribution 라이선스* ](http://creativecommons.org/licenses/by/2.5/).
