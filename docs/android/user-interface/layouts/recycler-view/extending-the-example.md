---
title: RecyclerView 예제 확장
description: RecyclerView 예제 앱에 항목 클릭 이벤트 처리기를 추가 합니다.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: d50971c1ef0064463e576a895729ad84577e1788
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028877"
---
# <a name="extending-the-recyclerview-example"></a>RecyclerView 예제 확장

[Basic RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) 에서 설명 하는 기본 앱은 실제로는 간단 하 게 표시 &ndash; 되지 않으며 쉽게 탐색할 수 있도록 고정 된 사진 항목 목록을 표시 합니다. 실제 응용 프로그램에서 사용자는 디스플레이에서 항목을 탭 하 여 앱과 상호 작용할 수 있는 것으로 간주 합니다. 또한 기본 데이터 원본은 변경 되거나 앱에서 변경 될 수 있으며, 표시의 내용이 이러한 변경 내용과 일관 되 게 유지 되어야 합니다. 다음 섹션에서는 기본 데이터 소스가 변경 될 때 항목 클릭 이벤트를 처리 하 고 `RecyclerView`를 업데이트 하는 방법을 알아봅니다.

### <a name="handling-item-click-events"></a>항목 클릭 이벤트 처리

사용자가 `RecyclerView`의 항목에 대 한 작업을 수행 하면 항목 클릭 이벤트가 생성 되어 해당 항목에 대해 작업 한 항목에 대해 앱에 알립니다. 이 이벤트는 `RecyclerView` &ndash;에 의해 생성 되지 않으며, 항목 보기 (view 보유자에 래핑된)는이를 클릭 이벤트로 탐색 하 고 보고 하는 것을 감지 합니다.

항목 클릭 이벤트를 처리 하는 방법을 설명 하기 위해 다음 단계에서는 사용자가 작업 한 사진을 보고 하기 위해 기본 사진 보기 앱이 수정 되는 방법에 대해 설명 합니다. 샘플 앱에서 항목 클릭 이벤트가 발생 하면 다음 시퀀스가 수행 됩니다.

1. 사진의 `CardView`는 항목 클릭 이벤트를 검색 하 여 어댑터에 알립니다.

2. 어댑터는 이벤트 (항목 위치 정보 포함)를 활동의 항목 클릭 처리기에 전달 합니다.

3. 활동의 항목 클릭 처리기는 항목 클릭 이벤트에 응답 합니다.

먼저 `ItemClick` 이라는 이벤트 처리기 멤버가 `PhotoAlbumAdapter` 클래스 정의에 추가 됩니다.

```csharp
public event EventHandler<int> ItemClick;
```

그런 다음 항목 클릭 이벤트 처리기 메서드가 `MainActivity`에 추가 됩니다.
이 처리기는 작업 항목을 표시 하는 알림 메시지를 잠깐 표시 합니다.

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

다음으로 `PhotoAlbumAdapter`에 `OnItemClick` 처리기를 등록 하는 코드 줄이 필요 합니다. 이 작업을 수행 하는 적절 한 위치는 `PhotoAlbumAdapter`를 만든 직후입니다. 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

이 기본 예제에서 처리기 등록은 주 활동의 `OnCreate` 메서드에서 발생 하지만 프로덕션 앱은 `OnResume`에 처리기를 등록 하 고 &ndash; `OnPause`에서 등록을 취소할 수 있습니다. 자세한 내용은 [활동 수명 주기](~/android/app-fundamentals/activity-lifecycle/index.md) 를 참조 하세요.

이제 `PhotoAlbumAdapter`는 항목 클릭 이벤트를 받을 때 `OnItemClick`를 호출 합니다. 다음 단계는이 `ItemClick` 이벤트를 발생 시키는 처리기를 어댑터에 만드는 것입니다. 다음 메서드 `OnClick`는 어댑터의 `ItemCount` 메서드 바로 뒤에 추가 됩니다.

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

이 `OnClick` 메서드는 항목 보기에서 항목 클릭 이벤트에 대 한 어댑터의 *수신기* 입니다. 항목 뷰를 사용 하 여이 수신기를 항목 뷰로 등록 하려면 먼저이 메서드를 추가 인수로 사용 하도록 `PhotoViewHolder` 생성자를 수정 하 고 항목 뷰 `Click` 이벤트를 사용 하 여 `OnClick`을 등록 해야 합니다.
수정 된 `PhotoViewHolder` 생성자는 다음과 같습니다.

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

`itemView` 매개 변수는 사용자가 작업 한 `CardView`에 대 한 참조를 포함 합니다. 뷰 홀더 기본 클래스는 `LayoutPosition` 속성을 통해 나타내는 항목 (`CardView`)의 레이아웃 위치를 알고 있으며,이 위치는 항목 클릭 이벤트가 발생 하는 경우 어댑터의 `OnClick` 메서드로 전달 됩니다. 어댑터의 `OnCreateViewHolder` 메서드는 어댑터의 `OnClick` 메서드를 뷰 소유자의 생성자에 전달 하도록 수정 됩니다.

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

이제 샘플 사진 보기 앱을 빌드 및 실행 하면 화면에 표시 되는 사진을 누르면 표시 되는 사진을 보고 하는 알림이 표시 됩니다.

[사진 카드를 탭 할 때 표시 되는 예제 알림![](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

이 예제에서는 `RecyclerView`를 사용 하 여 이벤트 처리기를 구현 하는 한 가지 방법을 보여 줍니다. 여기에서 사용할 수 있는 다른 방법은 뷰 보유자에 이벤트를 놓고 어댑터에서 이러한 이벤트를 구독할 수 있도록 하는 것입니다. 샘플 사진 앱에서 사진 편집 기능을 제공 하는 경우 각 `CardView`내의 `ImageView` 및 `TextView`에 대 한 별도의 이벤트가 필요 합니다. `TextView`에 대 한 터치는 사용자가 캡션을 편집할 수 있는 `EditView` 대화 상자를 시작 합니다. 를 사용 하 여 `ImageView`에 접촉 하면 사진 손질 도구를 시작 하 여 사용자가 사진을 자르거나 회전할 수 있습니다. 앱의 요구 사항에 따라 터치 이벤트를 처리 하 고 응답 하기 위한 최상의 방법을 디자인 해야 합니다.

데이터 집합이 변경 될 때 `RecyclerView`을 업데이트 하는 방법을 보여 주기 위해 샘플 사진 보기 앱을 수정 하 여 데이터 원본의 사진을 무작위로 선택 하 고 첫 번째 사진과 교환할 수 있습니다. 먼저 **무작위 선택** 단추가 예제 사진 응용 프로그램의 **Main. axml** 레이아웃에 추가 됩니다.

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

그런 다음 주 활동의 `OnCreate` 메서드 끝에 코드를 추가 하 여 레이아웃에서 `Random Pick` 단추를 찾고 처리기를 연결 합니다.

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

이 처리기는 **임의의 선택** 단추를 누를 때 사진 앨범의 `RandomSwap` 메서드를 호출 합니다. `RandomSwap` 메서드는 데이터 원본의 첫 번째 사진과 사진을 임의로 바꾼 다음 임의로 바뀐 사진의 인덱스를 반환 합니다. 이 코드를 사용 하 여 샘플 앱을 컴파일하고 실행 하는 경우 `RecyclerView`에서 데이터 소스에 대 한 변경 내용을 인식 하지 못하므로 **임의의 선택** 단추를 누르면 표시 되는 변경 내용이 표시 되지 않습니다.

데이터 소스가 변경 된 후 `RecyclerView` 업데이트 된 상태로 유지 하려면 변경 된 컬렉션의 각 항목에 대해 어댑터의 `NotifyItemChanged` 메서드를 호출 하도록 **무작위 선택** 클릭 처리기를 수정 해야 합니다 .이 경우에는 두 항목이 변경 됩니다. 첫 번째 사진과 사진 교체). 이렇게 하면 `RecyclerView`는 데이터 원본의 새 상태와 일치 하도록 표시를 업데이트 합니다.

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

이제 **무작위 선택** 단추를 누르면 컬렉션의 더 아래에 있는 사진이 컬렉션의 첫 번째 사진과 바뀌었으면 표시를 업데이트 `RecyclerView`.

[교환 전![첫 번째 스크린샷, 교환 후 두 번째 스크린샷](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

물론 `NotifyDataSetChanged`는 `NotifyItemChanged`에 대 한 두 호출을 수행 하는 대신 호출 될 수 있지만, 이렇게 하면 컬렉션에서 두 개의 항목만 변경 된 경우에도 전체 컬렉션을 새로 고치도록 강제 `RecyclerView`. `NotifyItemChanged` 호출은 `NotifyDataSetChanged`를 호출 하는 것 보다 훨씬 더 효율적입니다.

## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [기본 RecyclerView 예제](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
