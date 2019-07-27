---
title: Android 갤러리 컨트롤
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 6fe6b5a11473827eb716b0adf0fb0f3ae28a3538
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510276"
---
# <a name="xamarinandroid-gallery-control"></a>Xamarin Android 갤러리 컨트롤

[`Gallery`](xref:Android.Widget.Gallery)는 가로 스크롤 목록에 항목을 표시 하 고 현재 선택 항목을 뷰의 가운데에 배치 하는 데 사용 되는 레이아웃 위젯입니다.

> [!IMPORTANT]
> 이 위젯은 Android 4.1 (API 수준 16)에서 더 이상 사용 되지 않습니다. 

이 자습서에서는 사진 갤러리를 만든 다음 갤러리 항목이 선택 될 때마다 알림 메시지를 표시 합니다.

콘텐츠 뷰에 대해 `Gallery` [`FindViewById`](xref:Android.App.Activity.FindViewById*)레이아웃을 설정한 후에는를 사용 하 여 레이아웃에서를 캡처합니다. `Main.axml`
여[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
그런 다음 속성을 사용 하 여 dallery에 표시 `ImageAdapter`되는 모든 항목의 원본으로 사용자 지정 어댑터 ()를 설정 합니다. 는 `ImageAdapter` 다음 단계에서 생성 됩니다.

갤러리의 항목을 클릭할 때이 작업을 수행 하려면 익명 대리자가[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
합니다. 다음을 보여 줍니다.[`Toast`](xref:Android.Widget.Toast)
선택 된 항목의 인덱스 위치 (0부터 시작)를 표시 합니다. 실제 시나리오에서는 위치를 사용 하 여 다른 작업의 전체 크기 이미지를 가져올 수 있습니다.

먼저, 그릴 수 있는 리소스 디렉터리 (**리소스/그릴**수 있는)에 저장 된 이미지를 참조 하는 id 배열을 포함 하는 몇 가지 멤버 변수가 있습니다.

다음은 클래스 생성자 이며, 여기서[`Context`](xref:Android.Content.Context)
`ImageAdapter` 인스턴스의 경우가 정의 되 고 로컬 필드에 저장 됩니다.
그런 다음에서 [`BaseAdapter`](xref:Android.Widget.BaseAdapter)상속 된 몇 가지 필수 메서드를 구현 합니다.
생성자와[`Count`](xref:Android.Widget.BaseAdapter.Count)
속성은 설명이 필요 없습니다. 일반적으로[`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
는 어댑터의 지정 된 위치에 있는 실제 개체를 반환 하지만이 예제에서는 무시 됩니다. 마찬가지[`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
항목의 행 id를 반환 해야 하지만 여기에는 필요 하지 않습니다.

메서드는 이미지를에 적용 하는 작업을 수행 합니다.[`ImageView`](xref:Android.Widget.ImageView)
다음에 포함 될[`Gallery`](xref:Android.Widget.Gallery)
이 메서드에서 멤버는[`Context`](xref:Android.Content.Context)
는 새 [`ImageView`](xref:Android.Widget.ImageView)를 만드는 데 사용 됩니다.
여[`ImageView`](xref:Android.Widget.ImageView)
는 그릴 수 있는 리소스의 로컬 배열에서 이미지를 적용 하 여 준비 됩니다.[`Gallery.LayoutParams`](xref:Android.Widget.Gallery.LayoutParams)
이미지의 높이와 너비, 크기를에 맞게 설정[`ImageView`](xref:Android.Widget.ImageView)
그리고 마지막으로 생성자에서 획득 한 styleable 특성을 사용 하도록 배경을 설정 합니다.

다른 [`ImageView.ScaleType`](xref:Android.Widget.ImageView.ScaleType) 이미지 크기 조정 옵션은를 참조 하세요.

## <a name="walkthrough"></a>연습

*HelloGallery*라는 새 프로젝트를 시작 합니다.

[![새 솔루션 대화 상자에서 새 Android 프로젝트의 스크린샷](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

사용 하려는 사진을 찾거나 [이 샘플 이미지를 다운로드](https://developer.android.com/shareables/sample_images.zip)하세요.
프로젝트의 **리소스/그릴** 수 있는 디렉터리에 이미지 파일을 추가 합니다. **속성** 창에서 각에 대 한 빌드 동작을 **Androidresource**로 설정 합니다.

**리소스/레이아웃/기본. axml** 을 열고 다음을 삽입 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

을 `MainActivity.cs` 열고 다음 코드를 삽입 합니다.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
방법이

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

해당 하위 클래스 라는 `ImageAdapter` 새 클래스를 만듭니다. [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

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

애플리케이션을 실행합니다. 아래 스크린샷에서와 같이 표시 됩니다.

![샘플 이미지를 표시 하는 HelloGallery의 스크린샷](gallery-images/hellogallery3.png)

## <a name="references"></a>참조

- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)
- [`Gallery`](xref:Android.Widget.Gallery)
- [`ImageView`](xref:Android.Widget.ImageView)

*이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고*
[*Creative Commons 2.5 특성 라이선스*](http://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다.
