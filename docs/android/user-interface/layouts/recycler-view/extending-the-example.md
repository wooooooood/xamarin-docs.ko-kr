---
title: RecyclerView 예제 확장
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 83147261a2d5458272f7e2bc105154da4308f4b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769265"
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView 예제 확장


기본 응용 프로그램에 설명 된 [기본 RecyclerView 예제에서는](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) 실제로 템플릿이 그다지 &ndash; 단순히 스크롤 하 고 쉽게 찾을 수 사진 항목의 고정된 목록을 표시 합니다. 실제 응용 프로그램에서 사용자가 화면에서 항목을 탭 하 여 앱과 상호 작용할 수 수 있어야 합니다. 또한 기본 데이터 원본을 변경할 수 있습니다 (또는 응용 프로그램에서 변경할 수) 및 디스플레이 내용을 이러한 변경 내용으로 일관성을 유지 해야 합니다. 다음 섹션의 항목 클릭 이벤트를 처리 하 고 업데이트 하는 방법을 알아봅니다 `RecyclerView` 때 기본 데이터 소스가 변경 합니다.


### <a name="handling-item-click-events"></a>항목 클릭 이벤트를 처리합니다.

사용자와 항목에 연결 하는 경우는 `RecyclerView`, 항목이 처리 된와 관련 된 응용 프로그램에 알리기 위해 항목 클릭 이벤트가 생성 됩니다. 이 이벤트에서 생성 되지 않습니다 `RecyclerView` &ndash; (있음 보기 소유자에서 래핑된) 항목 보기가 작업을 검색 하 고 이벤트를 클릭 합니다. 이러한 작업을 보고 하는 대신 합니다.

를 항목 클릭 이벤트를 처리 하는 방법을 보여 주기 위해 다음 단계는 사진 사용자에 의해 처리 된가 보고서를 기본 사진 보기 응용 프로그램은 수정 하는 방법을 설명 합니다. 샘플 응용 프로그램에서 항목을 클릭 이벤트가 발생 하는 경우 다음 순서 대로 수행이 됩니다.

1.  사진 `CardView` 항목 click 이벤트를 검색 하 고 어댑터에 알립니다.

2.  어댑터는 활동의 항목 클릭 처리기에 위치 정보 항목) (사용 하 여 이벤트를 전달합니다.

3.  활동의 항목 클릭 처리기 항목 클릭 이벤트에 응답합니다.

첫째, 이벤트 처리기 멤버 호출 `ItemClick` 에 추가 되 고 `PhotoAlbumAdapter` 클래스 정의:

```csharp
public event EventHandler<int> ItemClick;
```

마지막, 항목 클릭 이벤트 처리기 메서드를 추가 `MainActivity`합니다.
이 처리기에는 사진 항목을 작업 했습니다 나타내는 알림을 간단 하 게 표시 됩니다.

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

그런 다음 코드 줄을 해야 하는 등록 된 `OnItemClick` 처리기를 `PhotoAlbumAdapter`합니다. 이 작업을 수행 하는 데는 바로 다음 `PhotoAlbumAdapter` 만들어집니다 (주요 작업에서 `OnCreate` 메서드):

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

`PhotoAlbumAdapter` 이제 호출 `OnItemClick` 항목 클릭 이벤트를 받을 때입니다. 이 발생 하는 어댑터에는 처리기를 만들려면 다음 단계는 `ItemClick` 이벤트입니다. 다음 메서드를 `OnClick`, 어댑터의 직후 추가 `ItemCount` 메서드:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

이 `OnClick` 메서드는 어댑터의 *수신기* 항목 보기에서 항목 클릭 이벤트에 대 한 합니다. 이 수신기는 항목 보기 (항목 보기 뷰 소유자)를 통해 등록할 수 전에 `PhotoViewHolder` 생성자는이 메서드는 추가 인수를 수락 하 고 등록을 수정 해야 `OnClick` 항목 보기와 `Click` 이벤트입니다.
다음 수정 된은 `PhotoViewHolder` 생성자:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` 에 대 한 참조를 포함 하는 매개 변수는 `CardView` 사용자에 의해 처리 된입니다. 뷰 소유자 기본 클래스는이 항목의 레이아웃 위치를 알고 참고 (`CardView`) 나타내는 (통해는 `LayoutPosition` 속성),이 위치는 어댑터에 전달 됩니다 `OnClick` 메서드 항목 클릭 이벤트가 발생 하는 경우. 어댑터의 `OnCreateViewHolder` 메서드는 어댑터를 통과 하도록 수정 됩니다 `OnClick` 메서드 뷰 소유자의 생성자에:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

이제 빌드하고 샘플 사진 보기 응용 프로그램을 실행 하는 경우 디스플레이에서 사진을 탭 표시는 사진 touched가 보고 하는 알림을 발생 합니다.

[![사진 카드 때 표시 되는 예제 메시지는 탭](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

이 예제에서는 이벤트 처리기를 구현 하기 위한 방법 중 하나일 뿐 `RecyclerView`합니다. 뷰 소유자에 이벤트를 배치 하 고 이러한 이벤트를 구독 하는 어댑터를 포함 하 여기에 사용 될 수 있는 다른 방법이입니다. 샘플 사진 앱 사진 편집 기능을 제공 하는 경우 개별 이벤트에 필요한 것은 `ImageView` 및 `TextView` 각 `CardView`:에 `TextView` 시작는 `EditView` 사용자 편집할 수 있는 대화 상자 캡션 및 작업에는 `ImageView` 사용자가 사진을 회전 하거나 자를 수 있는 사진 touchup 도구를 시작 합니다. 응용 프로그램의 요구에 따라 처리 하 고 터치 이벤트에 응답 하 고 있습니다. 가장 좋은 방법은 디자인 해야 합니다.

설명 하기 위해 방법을 `RecyclerView` 샘플 사진 보기 응용 프로그램 데이터 집합이 변경 임의로 데이터 원본에서 사진을 선택 하 고 첫 번째 사진 작동이를 수정할 수 있는 경우에 업데이트할 수 있습니다. 첫째, 한 **임의 선택** 예제 사진 앱에 단추 추가 됩니다 **Main.axml** 레이아웃:

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

다음으로 기본 활동의 끝에 코드가 추가 `OnCreate` 찾을 방법은 `Random Pick` 레이아웃 단추를 선택한 처리기를 여기에 연결:

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

사진 앨범을 호출 하는이 처리기 `RandomSwap` 메서드 때는 **임의 선택** 단추 탭 됩니다. `RandomSwap` 메서드 임의로 데이터 원본에 첫 번째 사진 인 사진을 교환 다음 임의로 교환 사진의 인덱스를 반환 합니다. 컴파일하고이 코드로 샘플 응용 프로그램을 실행 하는 경우 탭는 **임의 선택** 때문에 표시 변경 단추 내보내지지 않습니다는 `RecyclerView` 데이터 소스에 변경 내용을 인식 하지 않습니다.

유지 하려면 `RecyclerView` 데이터 소스가 변경, 후 업데이트는 **임의 선택** 클릭 처리기 어댑터의 호출 하도록 수정 해야 `NotifyItemChanged` 변경 된 컬렉션의 각 항목에 대 한 메서드 (이 경우에 두 항목을 사용할 변경: 첫 번째 사진 및 교체 된 사진). 이 인해 `RecyclerView` 데이터 원본의 새 상태와 일치 되도록 표시를 업데이트 합니다.

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

이제,는 **임의 선택** 단추는 탭 `RecyclerView` 는 사진 추가 아래로 컬렉션에서 교체 된 컬렉션의 첫 번째 사진으로 표시 하도록 디스플레이가 업데이트:

[![교환 후 두 번째 스크린 샷 교체 하기 전에 첫 번째 스크린 샷](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

물론, `NotifyDataSetChanged` 를 두 번 호출 하는 대신 호출 수 `NotifyItemChanged`, 하므로 이렇게 하면을 적용 하지만 `RecyclerView` 가 컬렉션에 두 개의 항목을 변경 하는 경우에 전체 컬렉션을 새로 고칠 수 있습니다. 호출 `NotifyItemChanged` 호출 보다 훨씬 더 효율적 `NotifyDataSetChanged`합니다.


## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
