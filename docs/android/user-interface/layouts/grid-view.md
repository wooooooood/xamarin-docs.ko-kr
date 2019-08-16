---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: f71c275dd2beee6aedf41ecd19c8a4a39ab5a36f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522635"
---
# <a name="xamarinandroid-gridview"></a>Xamarin Android GridView

[`GridView`](xref:Android.Widget.GridView)는입니다.[`ViewGroup`](xref:Android.Views.ViewGroup)
2 차원 스크롤 가능한 모눈에 항목을 표시 하는입니다. 표 항목은를 [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)사용 하 여 레이아웃에 자동으로 삽입 됩니다.

이 자습서에서는 이미지 미리 보기의 그리드를 만듭니다. 항목을 선택 하면 알림 메시지에 이미지의 위치가 표시 됩니다.

**HelloGridView**라는 새 프로젝트를 시작 합니다.

사용 하려는 사진을 찾거나 [이 샘플 이미지를 다운로드](https://developer.android.com/shareables/sample_images.zip)하세요. 프로젝트의 **리소스/그릴** 수 있는 디렉터리에 이미지 파일을 추가 합니다. **속성** 창에서 각에 대 한 빌드 동작을 **Androidresource**로 설정 합니다.

**Resources/Layout/Main. axml** 파일을 열고 다음을 삽입 합니다.

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

그러면 [`GridView`](xref:Android.Widget.GridView) 전체 화면이 채워집니다. 특성은 설명이 필요 하지 않습니다. 유효한 특성에 대 한 자세한 내용은 [`GridView`](xref:Android.Widget.GridView) 참조를 참조 하세요.

을 `HelloGridView.cs` 열고 다음 코드를 삽입 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

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

콘텐츠 뷰에 대해 **기본. axml** 레이아웃이 설정 된 후는 [`GridView`](xref:Android.Widget.GridView) 레이아웃에서로 [`FindViewById`](xref:Android.App.Activity.FindViewById*)캡처됩니다. 여[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
그런 다음 속성을 사용 하 여 표에 표시 될`ImageAdapter`모든 항목의 원본으로 사용자 지정 어댑터 ()를 설정 합니다. 는 `ImageAdapter` 다음 단계에서 생성 됩니다.

모눈의 항목을 클릭할 때이 작업을 수행 하려면 익명 대리자가 [`ItemClick`](xref:Android.Widget.AdapterView.ItemClick) 이벤트를 구독 합니다.
선택 된 항목 [`Toast`](xref:Android.Widget.Toast) 의 인덱스 위치 (0부터 시작)를 표시 하는를 보여 줍니다. 실제 시나리오에서는 위치를 사용 하 여 다른 작업의 전체 크기 이미지를 가져올 수 있습니다. Java 스타일 수신기 클래스는 .NET 이벤트 대신 사용할 수 있습니다.

해당 하위 클래스 라는 `ImageAdapter` 새 클래스를 만듭니다. [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

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

먼저에서 [`BaseAdapter`](xref:Android.Widget.BaseAdapter)상속 된 일부 필수 메서드를 구현 합니다. 생성자와 [`Count`](xref:Android.Widget.BaseAdapter.Count) 속성은 설명이 필요 없습니다. 일반적으로[`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
는 어댑터의 지정 된 위치에 있는 실제 개체를 반환 하지만이 예제에서는 무시 됩니다. 마찬가지[`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
항목의 행 id를 반환 해야 하지만 여기에는 필요 하지 않습니다.

필요한 첫 번째 방법은 [`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)입니다.
이 메서드는 새를 만듭니다.[`View`](xref:Android.Views.View)
에 추가 된 `ImageAdapter`각 이미지에 대해입니다. 이를 호출 하는 경우[`View`](xref:Android.Views.View)
이 전달 됩니다 .이 개체는 일반적으로이를 한 번 호출한 후에 재활용 된 개체 이므로 개체가 null 인지 여부를 확인 합니다. Null 인 경우[`ImageView`](xref:Android.Widget.ImageView)
가 인스턴스화되어 이미지 표현의 desired 속성으로 구성 됩니다.

- [`LayoutParams`](xref:Android.Views.View.LayoutParameters)보기&mdash;의 높이와 너비를 설정 합니다 .이를 위해 그릴 수 있는 크기에 관계 없이 각 이미지 크기를 조정 하 고 적절 하 게 이러한 크기에 맞게 자릅니다.

- [`SetScaleType()`](xref:Android.Widget.ImageView.SetScaleType*)필요한 경우 중앙에서 이미지를 잘라내는 것을 선언 합니다.

- [`SetPadding(int, int, int, int)`](xref:Android.Views.View.SetPadding*)모든 변의 안쪽 여백을 정의 합니다. 이미지의 가로 세로 비율이 서로 다른 경우에는 이미지가 ImageView에 지정 된 차원과 일치 하지 않는 경우에는 더 많은 여백이 줄어듭니다.

에 [`View`](xref:Android.Views.View) [전달`GetView()`](xref:Android.Widget.BaseAdapter.GetView*) 된가 null 이 아니면 로컬[`ImageView`](xref:Android.Widget.ImageView)
는 재활용 [`View`](xref:Android.Views.View) 된 개체를 사용 하 여 초기화 됩니다.

끝에[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)
메서드로 전달 되는 `thumbIds` [`ImageView`](xref:Android.Widget.ImageView)정수는의 이미지 리소스로 설정 된 배열에서 이미지를 선택 하는 데 사용 됩니다. `position`

왼쪽의 모든 것은 그릴 수 있는 `thumbIds` 리소스의 배열을 정의 하는 것입니다.

애플리케이션을 실행합니다. 그리드 레이아웃은 다음과 같이 표시 됩니다.

[![15 개의 이미지를 표시 하는 GridView의 예제 스크린샷](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

[`GridView`](xref:Android.Widget.GridView) 및의 동작을 시험해 보세요.[`ImageView`](xref:Android.Widget.ImageView)
요소 속성을 조정 합니다. 예를 들어를 사용 하 [`LayoutParams`](xref:Android.Views.View.LayoutParameters) 는 [`SetAdjustViewBounds()`](xref:Android.Widget.ImageView.SetAdjustViewBounds*)대신를 사용 하십시오.

## <a name="references"></a>참조 항목

- [`GridView`](xref:Android.Widget.GridView)
- [`ImageView`](xref:Android.Widget.ImageView)
- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

_이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 [Creative Commons 2.5 특성 라이선스](http://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다._
