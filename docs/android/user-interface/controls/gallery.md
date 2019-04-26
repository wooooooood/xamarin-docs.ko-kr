---
title: 갤러리
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: f9b73428531deeacc7bdea271cdc0c2872038e99
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218563"
---
# <a name="gallery"></a>갤러리

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) 레이아웃 위젯의 가로 스크롤 목록에 항목을 표시 하는 데 사용 되며 현재 선택 영역 뷰의 가운데에 배치 합니다.

> [!IMPORTANT]
> 이 위젯은 Android 4.1 (API 레벨 16)에서 사용 되지 않았습니다. 

이 자습서에서는 사진 갤러리를 만들고 갤러리 항목이 선택 될 때마다 알림 메시지를 표시 합니다.

후는 `Main.axml` 레이아웃 콘텐츠 뷰의 경우에 대해 설정 되어 합니다 `Gallery` 사용 하 여 레이아웃에서 캡처된 [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/)합니다.
는 [`Adapter`](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)
속성은 다음 사용자 지정 어댑터를 설정 하는 데 사용 됩니다 ( `ImageAdapter`)는 dallery에 표시할 모든 항목에 대 한 원본으로 합니다. `ImageAdapter` 다음 단계에서 만들어집니다.

갤러리에서 항목을 클릭할 때 무언가 익명 대리자가 구독에 [`ItemClick`](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)
합니다. 표시는 [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
제거할 항목의 인덱스 위치 (0부터 시작)를 표시 하는 (실제 시나리오에서는 위치 수 사용할 다른 작업에 대 한 전체 크기 이미지를 가져오려고).

먼저은 드로어 블 리소스 디렉터리에 저장 하 여 이미지를 참조 하는 Id의 배열을 포함 하는 몇 가지 멤버 변수 (**리소스/drawable**).

다음 클래스 생성자에는 위치는 [`Context`](https://developer.xamarin.com/api/type/Android.Content.Context/)
에 대 한는 `ImageAdapter` 인스턴스 정의 되 고 로컬 필드에 저장 합니다.
상속 되며, 필요한 일부 메서드 구현 어 [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)합니다.
생성자 및 [`Count`](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/)
속성은 설명이 따로 필요 없습니다. 일반적으로 [`GetItem(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/)
어댑터에서 지정된 된 위치에 실제 개체를 반환 해야 하지만이 예제에서는 무시 됩니다. 마찬가지로, [`GetItemId(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/)
항목의 행 id를 반환 해야 하지만 여기 필요 하지 않습니다.

메서드는 이미지를 적용 하는 작업을 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
에 포함 될 합니다 [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
이 방법에서는 멤버 [`Context`](https://developer.xamarin.com/api/type/Android.Content.Context/)
새로 만드는 데 사용 됩니다 [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)합니다.
는 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
로컬 설정 드로어 블 리소스 배열에서 이미지를 적용 하 여 준비 되는 [`Gallery.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/)
높이 너비에 맞게 크기 조정 설정 이미지는 [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
차원 및 생성자에서 획득 한 숨기고 스타일 지정 특성을 사용 하는 백그라운드에 마지막으로 설정 합니다.

참조 [ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/) 다른 이미지 크기 조정 옵션에 대 한 합니다.

## <a name="walkthrough"></a>연습

명명 된 새 프로젝트를 시작 *HelloGallery*합니다.

[![새 솔루션 대화 상자에서 새 Android 프로젝트의 스크린샷](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

를 사용 하려는 일부 사진 찾기 또는 [이러한 샘플 이미지를 다운로드](https://developer.android.com/shareables/sample_images.zip)합니다.
프로젝트의 이미지 파일을 추가할 **리소스/Drawable** 디렉터리입니다. 에 **속성** 창에서 빌드 작업을 각각에 대 한 설정 **AndroidResource**합니다.

오픈 **Resources/Layout/Main.axml** 하 고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

열기 `MainActivity.cs` 에 다음 코드를 삽입 합니다 [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
방법:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

라는 새 클래스를 만듭니다 `ImageAdapter` 해당 서브 클래스 [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

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
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

애플리케이션을 실행합니다. 아래 스크린샷 처럼 표시 됩니다.

![스크린 샷의 HelloGallery 샘플 이미지를 표시 합니다.](gallery-images/hellogallery3.png)



## <a name="references"></a>참조

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*이 페이지의 일부는 생성 하 고 Android Open Source Project에서 공유 된 조건에 따라 사용 되는 작업에 따라 수정 합니다*
[*Creative Commons 2.5 Attribution License* ](http://creativecommons.org/licenses/by/2.5/).


