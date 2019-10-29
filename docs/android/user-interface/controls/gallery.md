---
title: Android 갤러리 컨트롤
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
ms.openlocfilehash: 93eb7f98da6f3fe06f288eae5823f7173e58585f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029240"
---
# <a name="xamarinandroid-gallery-control"></a>Xamarin Android 갤러리 컨트롤

[`Gallery`](xref:Android.Widget.Gallery) 는 가로 스크롤 목록에 항목을 표시 하 고 현재 선택 항목을 뷰의 가운데에 배치 하는 데 사용 되는 레이아웃 위젯입니다.

> [!IMPORTANT]
> 이 위젯은 Android 4.1 (API 수준 16)에서 더 이상 사용 되지 않습니다. 

이 자습서에서는 사진 갤러리를 만든 다음 갤러리 항목이 선택 될 때마다 알림 메시지를 표시 합니다.

콘텐츠 뷰에 대해 `Main.axml` 레이아웃이 설정 된 후에는 [`FindViewById`](xref:Android.App.Activity.FindViewById*)레이아웃에서 `Gallery` 캡처됩니다.
[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
그런 다음 속성은 dallery에 표시 될 모든 항목의 원본으로 사용자 지정 어댑터 (`ImageAdapter`)를 설정 하는 데 사용 됩니다. `ImageAdapter`은 다음 단계에서 생성 됩니다.

갤러리의 항목을 클릭할 때이 작업을 수행 하려면 익명 대리자가 [`ItemClick`](xref:Android.Widget.AdapterView.ItemClick) 를 구독 합니다.
입니다. [`Toast`](xref:Android.Widget.Toast) 표시
선택 된 항목의 인덱스 위치 (0부터 시작)를 표시 합니다. 실제 시나리오에서는 위치를 사용 하 여 다른 작업의 전체 크기 이미지를 가져올 수 있습니다.

먼저, 그릴 수 있는 리소스 디렉터리 (**리소스/그릴**수 있는)에 저장 된 이미지를 참조 하는 id 배열을 포함 하는 몇 가지 멤버 변수가 있습니다.

다음은 [`Context`](xref:Android.Content.Context) 클래스 생성자입니다.
`ImageAdapter` 인스턴스는 정의 되 고 로컬 필드에 저장 됩니다.
다음으로 [`BaseAdapter`](xref:Android.Widget.BaseAdapter)에서 상속 되는 일부 필수 메서드를 구현 합니다.
생성자 및 [`Count`](xref:Android.Widget.BaseAdapter.Count)
속성은 설명이 필요 없습니다. 일반적으로 [`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
는 어댑터의 지정 된 위치에 있는 실제 개체를 반환 하지만이 예제에서는 무시 됩니다. 마찬가지로 [`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
항목의 행 id를 반환 해야 하지만 여기에는 필요 하지 않습니다.

메서드는 이미지를 [`ImageView`](xref:Android.Widget.ImageView) 에 적용 하는 작업을 수행 합니다.
[`Gallery`](xref:Android.Widget.Gallery) 에 포함 됩니다.
이 메서드에서는 멤버 [`Context`](xref:Android.Content.Context)
새 [`ImageView`](xref:Android.Widget.ImageView)을 만드는 데 사용 됩니다.
[`ImageView`](xref:Android.Widget.ImageView)
는 그릴 수 있는 리소스의 로컬 배열에서 이미지를 적용 하 고 [`Gallery.LayoutParams`](xref:Android.Widget.Gallery.LayoutParams) 설정 하 여 준비 됩니다.
이미지의 높이와 너비, 크기를 [`ImageView`](xref:Android.Widget.ImageView) 에 맞게 설정
그리고 마지막으로 생성자에서 획득 한 styleable 특성을 사용 하도록 배경을 설정 합니다.

다른 이미지 크기 조정 옵션에 대 한 [`ImageView.ScaleType`](xref:Android.Widget.ImageView.ScaleType) 를 참조 하세요.

## <a name="walkthrough"></a>연습

*HelloGallery*라는 새 프로젝트를 시작 합니다.

[새 솔루션 대화 상자에서 새 Android 프로젝트의![스크린샷](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

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

`MainActivity.cs`를 열고 [`OnCreate()`](xref:Android.App.Activity.OnCreate*) 에 대해 다음 코드를 삽입 합니다.
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

서브 클래스 [`BaseAdapter`](xref:Android.Widget.BaseAdapter)`ImageAdapter` 이라는 새 클래스를 만듭니다.

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

## <a name="references"></a>참조 항목

- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)
- [`Gallery`](xref:Android.Widget.Gallery)
- [`ImageView`](xref:Android.Widget.ImageView)

_이 페이지의 일부는 Android 오픈 소스 프로젝트에서 만들고 공유 하 고 [Creative Commons 2.5 특성 라이선스](https://creativecommons.org/licenses/by/2.5/)에 설명 된 용어에 따라 사용 되는 작업을 기반으로 수정 됩니다._
