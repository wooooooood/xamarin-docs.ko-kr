---
title: RecyclerView 예제 확장
description: RecyclerView 예제 앱에 항목 클릭 이벤트 처리기를 추가 합니다.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: eca0f58a470228ce8e6331defe88c1ef727cef57
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036094"
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView 예제 확장


에 설명 된 기본 앱 [는 기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) 실제로 그다지 &ndash; 단순히 스크롤 하 고 사진 쉽게 찾을 수 있도록 하는 항목의 고정된 목록을 표시 합니다. 실제 응용 프로그램에 사용자 표시에서 항목을 탭 하 여 앱과 상호 작용할 수를 기대 합니다. 또한 데이터 원본 변경할 수 있습니다 (또는 앱에서 변경할 수) 및 디스플레이의 내용을 다음 변경이 내용으로 일관성을 유지 해야 합니다. 다음 섹션의 항목 클릭 이벤트를 처리 하 고 업데이트 하는 방법을 알아봅니다 `RecyclerView` 때 기본 데이터 원본 변경 합니다.


### <a name="handling-item-click-events"></a>항목 클릭 이벤트 처리

사용자가 항목을 터치 하는 경우는 `RecyclerView`, 항목 했습니다 touched는 대 한 앱에 알리기 위해 항목 클릭 이벤트가 생성 됩니다. 이 이벤트에서 생성 되지 않습니다 `RecyclerView` &ndash; 항목 뷰 (래핑됩니다 뷰 소유자)이 터치를 검색 하 고 이벤트를 클릭 합니다. 이러한 터치를 보고 하는 대신 합니다.

항목 클릭 이벤트를 처리 하는 방법을 설명 하기 위해, 다음 단계는 보고서 사용자는 사진을 접한 되었습니다 했습니다 하도록 기본 사진 보기 응용 프로그램을 수정 하는 방법을 설명 합니다. 샘플 앱에서 항목 클릭 이벤트가 발생 하는 경우 다음 순서 대로 수행이 됩니다.

1.  사진 `CardView` 항목 클릭 이벤트를 검색 하 고 어댑터에 알립니다.

2.  어댑터는 활동의 항목 클릭 처리기에 위치 정보 항목) (사용 하 여 이벤트를 전달합니다.

3.  활동의 항목 클릭 처리기 항목 클릭 이벤트에 응답합니다.

이벤트 처리기 멤버를 호출 하는 먼저 `ItemClick` 에 추가 되는 `PhotoAlbumAdapter` 클래스 정의:

```csharp
public event EventHandler<int> ItemClick;
```

항목 클릭 이벤트 처리기 메서드를 추가할 어 `MainActivity`합니다.
이 처리기에는 간단 하 게 사진을 항목 했습니다 touched 나타내는 알림을 표시 합니다.

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

등록 하는 데 필요한 코드 줄을 다음으로 `OnItemClick` 처리기 `PhotoAlbumAdapter`합니다. 이 작업을 수행 하기에 좋은 위치는 바로 다음 `PhotoAlbumAdapter` 만들어집니다. 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

이 기본 예제에서는 처리기 등록은 기본 작업의 수행 `OnCreate` 메서드이지만 프로덕션 앱에서 처리기를 등록할 수 있습니다 `OnResume` 에서 등록 취소 `OnPause` &ndash; 참조 [작업 수명 주기 ](~/android/app-fundamentals/activity-lifecycle/index.md) 자세한 내용은 합니다.

`PhotoAlbumAdapter` 이제 호출 `OnItemClick` 항목 클릭 이벤트를 수신 하는 것입니다. 이 발생 하는 어댑터에서 처리기를 만들려면 다음 단계는 `ItemClick` 이벤트입니다. 다음 메서드를 `OnClick`, 어댑터의 바로 뒤에 추가 됩니다 `ItemCount` 메서드:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

이렇게 `OnClick` 메서드는 어댑터 *수신기* 항목 보기에서 항목 클릭 이벤트에 대 한 합니다. 이 수신기는 항목 보기 (항목 보기의 뷰 소유자)를 통해 등록할 수 전에 합니다 `PhotoViewHolder` 생성자를이 메서드에 추가 인수로 받아 등록 하기 위해 수정 해야 합니다 `OnClick` 항목 보기를 사용 하 여 `Click` 이벤트입니다.
수정 된 같습니다 `PhotoViewHolder` 생성자:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

합니다 `itemView` 에 대 한 참조를 포함 하는 매개 변수는 `CardView` 가 사용자가 수행 하는 합니다. 뷰 소유자 기본 클래스 항목의 레이아웃 위치를 알고 있는지 확인 (`CardView`) 나타내는 (통해 합니다 `LayoutPosition` 속성),이 위치는 어댑터에 전달 되 고 `OnClick` 메서드 항목 클릭 이벤트가 발생 하는 경우. 어댑터가 `OnCreateViewHolder` 메서드는 어댑터의 전달 하도록 수정 됩니다 `OnClick` 보기 자리 표시자의 생성자에 메서드:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

이제을 빌드하고 샘플 사진 보기 응용 프로그램을 실행 하는 경우 디스플레이에서 사진을 탭 하면 표시는 사진을 했습니다 touched 보고 하는 알림.

[![카드 사진을 때 표시 되는 예제에서는 알림 메시지를 탭 할](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

이 예제에서는 이벤트 처리기를 구현 하기 위한 방법 중 하나일 뿐 `RecyclerView`합니다. 여기에 사용 될 수 있는 또 다른 방법은 보기 소유자에 이벤트를 배치 하 고 이러한 이벤트에 대 한 구독 어댑터를가지고 하는 것입니다. 샘플 사진 앱 사진 편집 기능을 제공 하는 경우 별도 이벤트에 필요한 것을 `ImageView` 및 `TextView` 각 `CardView`:에 대해를 `TextView` 을 실행할 때는 `EditView` 사용자가 편집할 수 있는 대화 상자 캡션 및에 영향을 주고는 `ImageView` 사용자가 사진을 회전 하거나 자를 수 있는 사진 손질 도구를 시작 합니다. 앱의 요구에 따라 터치 이벤트에 응답 하 고 처리를 위한 최선의 방법을 디자인 해야 합니다.

보여 주기 위해 어떻게 `RecyclerView` 임의로 데이터 원본에서 사진을 선택 하 고 첫 번째 사진을 사용 하 여 교환에 데이터 집합이 변경, 샘플 사진 보기 응용 프로그램을 수정할 수 있는 경우에 업데이트할 수 있습니다. 첫 번째는 **임의 선택** 단추가 예제 사진 앱에 추가 됩니다 **Main.axml** 레이아웃:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

그런 다음, 코드 기본 활동의 끝에 추가 됩니다 `OnCreate` 메서드를 찾습니다는 `Random Pick` 레이아웃에서 단추 및 처리기를 연결할:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

사진 앨범을 호출 하는이 처리기 `RandomSwap` 메서드는 경우는 **임의 선택** 단추를 탭 할. `RandomSwap` 메서드는 임의로 데이터 소스의 첫 번째 사진을 사용 하 여 사진을 교환 다음 임의로 교환 사진의 인덱스를 반환 합니다. 컴파일하고이 코드를 사용 하 여 샘플 앱을 실행 하는 경우 탭 합니다 **임의 선택** 때문에 단추 표시 변경에서 되지는지 않습니다를 `RecyclerView` 는 데이터 원본에 변경 내용을 인식 하지 못합니다.

되도록 `RecyclerView` 데이터 원본 변경 후에 업데이트를 **임의 선택** 클릭 처리기 어댑터의 호출을 수정 해야 합니다 `NotifyItemChanged` 변경 된 컬렉션의 각 항목에 대 한 메서드 (이 경우 두 항목에 변경: 첫 번째 사진과 바뀌었으면된 사진). 이 인해 `RecyclerView` 데이터 원본의 새 상태와 일치 되도록 표시를 업데이트 합니다.

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

이제, 합니다 **임의 선택** 단추를 탭 할 `RecyclerView` 는 사진을 추가 아래로 컬렉션에서으로 교환 되는 컬렉션의 첫 번째 사진 표시 하도록 디스플레이가 업데이트:

[![교환 후 두 번째 스크린샷 교환 하기 전에 첫 번째 스크린샷](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

물론 `NotifyDataSetChanged` 를 두 번 호출 하는 대신 호출할 수도 있습니다 `NotifyItemChanged`를 수행 하므로 강제로 수행 하지만 `RecyclerView` 가 컬렉션에 두 개의 항목을 변경 하는 경우에 전체 컬렉션을 새로 고치려면 합니다. 호출 `NotifyItemChanged` 호출 보다 훨씬 더 효율적입니다 `NotifyDataSetChanged`합니다.


## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
