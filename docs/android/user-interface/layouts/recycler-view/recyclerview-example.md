---
title: 기본 RecyclerView 예제
description: RecyclerView를 사용 하는 방법을 보여 주는 예제 앱입니다.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/30/2018
ms.openlocfilehash: 0c6568f9283491f0e55c79c77fb8efb2ae75f33c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758614"
---
# <a name="a-basic-recyclerview-example"></a>기본 RecyclerView 예제

일반적인 응용 프로그램 `RecyclerView` 에서 작동 하는 방식을 이해 하기 위해이 항목에서는를 사용 `RecyclerView` 하 여 많은 사진 컬렉션을 표시 하는 간단한 코드 예제 인 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) 샘플 앱에 대해 살펴봅니다. 

[![사진 표시를 위해 RecyclerView 보기를 사용 하는 두 가지 스크린샷](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** 는 [CardView](~/android/user-interface/controls/card-view.md) 를 사용 하 여 `RecyclerView` 레이아웃에서 각 사진 항목을 구현 합니다. `RecyclerView`성능상의 장점 때문에이 샘플 앱은 크게 지연 되지 않고 많은 사진을 원활 하 게 신속 하 게 스크롤할 수 있습니다.

### <a name="an-example-data-source"></a>예제 데이터 원본

이 예제 앱에서 "사진 앨범" 데이터 원본 ( `PhotoAlbum` 클래스로 나타냄)은 항목 콘텐츠와 함께 제공 `RecyclerView` 됩니다.
`PhotoAlbum`는 캡션이 있는 사진 컬렉션입니다. 이를 인스턴스화하면 미리 만들어진 32 사진 컬렉션을 얻게 됩니다.

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

의 `PhotoAlbum` 각 사진 인스턴스는 이미지 리소스 ID, `PhotoID`및 캡션 문자열를 읽을 수 있는 속성을 `Caption`노출 합니다. 각 사진은 인덱서를 통해 액세스할 수 있도록 사진 컬렉션이 구성 됩니다. 예를 들어 다음 코드 줄은 컬렉션에서 열 번째 사진의 이미지 리소스 ID 및 캡션에 액세스 합니다.

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum`는 컬렉션의 `RandomSwap` 첫 번째 사진을 컬렉션의 다른 위치에서 무작위로 선택 된 사진과 교환 하기 위해 호출할 수 있는 메서드를 제공 합니다.

```csharp
mPhotoAlbum.RandomSwap ();
```

의 `PhotoAlbum` 구현 세부 정보는 이해 `RecyclerView` `PhotoAlbum` 와 관련이 없으므로 소스 코드는 여기에 표시 되지 않습니다. 소스 코드 `PhotoAlbum` 는 [RecyclerViewer](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer) 샘플 앱의 [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) 에서 사용할 수 있습니다.

### <a name="layout-and-initialization"></a>레이아웃 및 초기화

**주. axml**레이아웃 파일은 `RecyclerView` `LinearLayout`다음 내에서 단일으로 구성 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

이 지원 라이브러리에 패키지 되어 있으므로 `RecyclerView` 정규화 된 이름 **v7** 를 사용 해야 합니다. 이 `OnCreate` 레이아웃을 `MainActivity` 초기화 하 고, 어댑터를 인스턴스화하고, 기본 데이터 소스를 준비 하는 메서드는 다음과 같습니다.

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

이 코드는 다음을 수행 합니다.

1. 데이터 소스 `PhotoAlbum` 를 인스턴스화합니다.

2. 사진 앨범 데이터 원본을 어댑터의 생성자에 전달 합니다 .이 내용은 `PhotoAlbumAdapter` 이 가이드의 뒷부분에서 정의 됩니다. 
   데이터 원본을 어댑터의 생성자에 매개 변수로 전달 하는 것이 모범 사례로 간주 됩니다. 

3. 레이아웃에서 `RecyclerView` 을 가져옵니다.

4. 위에 표시 된 대로 메서드 `RecyclerView` 를 `RecyclerView` `SetAdapter` 호출 하 여 어댑터를 인스턴스에 연결 합니다.

### <a name="layout-manager"></a>레이아웃 관리자

의 각 항목 `RecyclerView` 은 사진 이미지 및 사진 캡션을 `CardView` 포함 하는로 구성 됩니다 (자세한 내용은 아래의 [소유자 보기](#view-holder) 섹션에서 설명). 미리 정의 `LinearLayoutManager` 된는 세로 스크롤 정렬에서 `CardView` 각 레이아웃을 레이아웃 하는 데 사용 됩니다.

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

이 코드는 주 활동의 `OnCreate` 메서드에 있습니다. 레이아웃 관리자에 대 한 생성자에는 *컨텍스트가*필요 하므로 `MainActivity` 이 위에 표시 된 `this` 대로를 사용 하 여 전달 됩니다.

미리 정의 `LinearLayoutManager`된를 사용 하는 대신 두 `CardView` 항목을 나란히 표시 하는 사용자 지정 레이아웃 관리자를 연결 하 여 사진 컬렉션을 통과 하는 페이지 넘기기 애니메이션 효과를 구현할 수 있습니다. 이 가이드의 뒷부분에서 다른 레이아웃 관리자에서 교체 하 여 레이아웃을 수정 하는 방법의 예를 확인할 수 있습니다.

<a name="view-holder" />

### <a name="view-holder"></a>소유자 보기

뷰 홀더 클래스가 호출 `PhotoViewHolder`됩니다. 각 `PhotoViewHolder` 인스턴스는 연결 된 행 `ImageView` 항목 `TextView` 의 및에 대 한 참조를 보유 하며 여기에는 `CardView` 여기에 사실로 배치 됩니다.

[![ImageView 및 TextView를 포함 하는 CardView의 다이어그램](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder`에서 `RecyclerView.ViewHolder` 파생 되며, 위의 레이아웃에 표시 된 `ImageView` 및 `TextView` 에 대 한 참조를 저장 하는 속성을 포함 합니다.
`PhotoViewHolder`는 다음과 같은 두 개의 속성과 하나의 생성자로 구성 됩니다.

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

이 코드 예제에서 생성자는 `PhotoViewHolder` `PhotoViewHolder` 래핑하는 부모 항목 뷰 `CardView`()에 대 한 참조를 전달 합니다. 항상 부모 항목 뷰를 기본 생성자에 전달 합니다. 생성자 `PhotoViewHolder` 는 부모 `FindViewById` 항목 뷰에서를 호출 하 여 각 `TextView`자식 뷰 참조를 `ImageView` 찾고 결과를 각각 `Image` 및 `Caption` 속성에 저장 합니다. 어댑터는 나중에 새 데이터로이 `CardView`자식 뷰를 업데이트할 때 이러한 속성에서 뷰 참조를 검색 합니다.

에 대 한 `RecyclerView.ViewHolder`자세한 내용은 [RecyclerView. ViewHolder 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)를 참조 하세요.

### <a name="adapter"></a>어댑터

어댑터는 특정 사진 `RecyclerView` 에 대 한 데이터가 포함 된 각 행을 로드 합니다. 예를 들어, 행 위치 *p*의 지정 된 사진에 대해 어댑터는 데이터 원본 내에서 위치 *p* 의 연결 된 데이터를 찾고이 데이터를 `RecyclerView` 컬렉션의 *p* 위치에 있는 행 항목에 복사 합니다. 어댑터는 보기 소유자를 사용 하 여 해당 `ImageView` 위치에서 및 `TextView` 에 대 한 참조를 조회 하므로 사용자가 사진 컬렉션 `FindViewById` 을 스크롤하고 뷰를 다시 사용할 때 해당 보기에 대해를 반복적으로 호출할 필요가 없습니다.

**RecyclerViewer**에서 어댑터 클래스는를 만드는 `RecyclerView.Adapter` `PhotoAlbumAdapter`에서 파생 됩니다.

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

멤버 `mPhotoAlbum` 는 생성자에 전달 되는 데이터 소스 (사진 앨범)를 포함 합니다. 생성자는이 멤버 변수에 사진 앨범을 복사 합니다. 구현 되는 `RecyclerView.Adapter` 필수 메서드는 다음과 같습니다.

- **`OnCreateViewHolder`** &ndash; 항목 레이아웃 파일 및 보기 소유자를 인스턴스화합니다.

- **`OnBindViewHolder`** &ndash; 지정 된 위치에 있는 데이터를 참조가 지정 된 뷰 보유자에 저장 된 뷰로 로드 합니다.

- **`ItemCount`** &ndash; 데이터 소스의 항목 수를 반환 합니다.

레이아웃 관리자는 내에서 항목의 `RecyclerView`위치를 지정 하는 동안 이러한 메서드를 호출 합니다. 이러한 메서드의 구현은 다음 섹션에서 검사 합니다.

#### <a name="oncreateviewholder"></a>OnCreateViewHolder

레이아웃 관리자는에서 `OnCreateViewHolder` 항목을 `RecyclerView` 나타내기 위해 새 뷰 소유자가 필요한 경우를 호출 합니다. `OnCreateViewHolder`보기의 레이아웃 파일에서 항목 보기를 늘어납니다 새 `PhotoViewHolder` 인스턴스에서 뷰를 래핑합니다. 생성자 `PhotoViewHolder` 는 이전에 [뷰 보유자](#view-holder)에서 설명한 대로 레이아웃의 자식 뷰에 대 한 참조를 찾아 저장 합니다.

각 행 항목은 `CardView` `ImageView` (사진의 경우) 및 (캡션의)를 `TextView` 포함 하는로 표시 됩니다. 이 레이아웃은 PhotoCardView 파일에 **있습니다.**

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

이 레이아웃은 `RecyclerView`의 단일 행 항목을 나타냅니다. 아래 `OnBindViewHolder` 에서 설명 하는 메서드는 데이터 원본의 데이터를이 레이아웃의 `ImageView` 및 `TextView` 로 복사 합니다.
`OnCreateViewHolder`에서 `RecyclerView` 지정 된 사진 위치에 대해이 레이아웃을 늘어납니다 하 고, `PhotoViewHolder` 새 인스턴스를 인스턴스화합니다 .이는 연결 `CardView` 된 `ImageView` 레이아웃 `TextView` 의 및 자식 뷰에 대 한 참조를 찾고 캐시 합니다.

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

결과 뷰 보유자 인스턴스 `vh`는 호출자 (레이아웃 관리자)로 다시 반환 됩니다.

#### <a name="onbindviewholder"></a>OnBindViewHolder

레이아웃 관리자가 화면에 표시 되는 화면 영역에 `RecyclerView`특정 뷰를 표시할 준비가 되 면 어댑터의 `OnBindViewHolder` 메서드를 호출 하 여 지정 된 행 위치의 항목을 데이터 소스의 콘텐츠로 채웁니다. `OnBindViewHolder`지정 된 행 위치 (사진의 이미지 리소스 및 사진의 캡션에 대 한 문자열)에 대 한 사진 정보를 가져오고이 데이터를 연결 된 보기에 복사 합니다. 뷰는 `holder` 매개 변수를 통해 전달 되는 뷰 보유자 개체에 저장 된 참조를 통해 찾을 수 있습니다.

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

전달 된 뷰 보유자 개체를 사용 하기 전에 먼저 파생 된 뷰 홀더 유형 (이 경우 `PhotoViewHolder`)으로 캐스팅 해야 합니다.
어댑터는 뷰 보유자의 `Image` 속성에서 참조 하는 뷰에 이미지 리소스를 로드 하 고 캡션 텍스트를 뷰 보유자의 `Caption` 속성에서 참조 하는 뷰로 복사 합니다. 연결 된 뷰를 해당 데이터와 *바인딩합니다* .

`OnBindViewHolder` 는 데이터의 구조를 직접 처리 하는 코드입니다. 이 경우에서는 `OnBindViewHolder` 데이터 소스의 연결 된 데이터 항목 `RecyclerView` 에 항목 위치를 매핑하는 방법을 이해 합니다. 이 경우에는 위치를 사진 앨범에 배열 인덱스로 사용할 수 있기 때문에 매핑이 간단 합니다. 그러나 더 복잡 한 데이터 원본에는 이러한 매핑을 설정 하기 위해 추가 코드가 필요할 수 있습니다.

#### <a name="itemcount"></a>ItemCount

메서드 `ItemCount` 는 데이터 컬렉션의 항목 수를 반환 합니다. 예제 사진 뷰어 앱에서 항목 수는 사진 앨범의 사진 수입니다.

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

에 대 한 `RecyclerView.Adapter`자세한 내용은 [RecyclerView 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)를 참조 하세요.

### <a name="putting-it-all-together"></a>모두 함께 배치

예제 사진 `RecyclerView` 앱에 대 한 결과 구현은 데이터 원본 `MainActivity` , 레이아웃 관리자 및 어댑터를 만드는 코드로 구성 됩니다. `MainActivity``mRecyclerView` 인스턴스를 만들고, 데이터 원본 및 어댑터를 인스턴스화하고, 레이아웃 관리자와 어댑터를 연결 합니다.

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder`뷰 참조를 찾아서 캐시 합니다.

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter`세 가지 필수 메서드 재정의를 구현 합니다.

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

이 코드를 컴파일하고 실행 하면 다음 스크린샷에 표시 된 것 처럼 기본 사진 보기 앱이 만들어집니다.

[![사진 카드를 세로로 스크롤 하는 사진 보기 앱의 두 스크린샷](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

위의 스크린샷에 표시 된 것 처럼 그림자를 그리지 않는 경우에는 **Properties/androidmanifest .xml** 을 편집 하 고 `<application>` 요소에 다음 특성 설정을 추가 합니다.

```xml
android:hardwareAccelerated="true"
```

이 기본 앱은 사진 앨범의 탐색만 지원 합니다. 항목 터치 이벤트에 응답 하지 않으며 기본 데이터의 변경 내용도 처리 하지 않습니다. 이 기능은 [RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)에 추가 되었습니다.

### <a name="changing-the-layoutmanager"></a>LayoutManager 변경

`RecyclerView`의 유연성으로 인해 다른 레이아웃 관리자를 사용 하도록 앱을 쉽게 수정할 수 있습니다. 다음 예제에서는 세로 선형 레이아웃이 아니라 가로로 스크롤되는 그리드 레이아웃을 사용 하 여 사진 앨범을 표시 하도록 수정 되었습니다. 이렇게 하려면 레이아웃 관리자 인스턴스화가 다음과 같이를 `GridLayoutManager` 사용 하도록 수정 됩니다.

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

이 코드 변경 내용은 세로 `LinearLayoutManager` `GridLayoutManager` 를 가로 방향으로 스크롤 하는 두 개의 행으로 구성 된 표를 표시 하는으로 바꿉니다. 응용 프로그램을 다시 컴파일하고 실행 하면 사진이 표에 표시 되 고 세로 방향으로 스크롤이 아닌 가로 방향으로 표시 됩니다.

[![세로로 사진을 가로 방향으로 스크롤 하는 앱의 예제 스크린샷](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

코드 한 줄만 변경 하 여 다른 동작을 포함 하는 다른 레이아웃을 사용 하도록 사진 보기 앱을 수정할 수 있습니다.
레이아웃 스타일을 변경 하기 위해 어댑터 코드와 레이아웃 XML을 수정 하지 않았습니다. 

다음 항목에서 [RecyclerView 예제를 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)하면이 기본 샘플 앱이 확장 되어 항목 클릭 이벤트를 처리 하 고 기본 데이터 `RecyclerView` 소스가 변경 될 때 업데이트 됩니다.

## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
