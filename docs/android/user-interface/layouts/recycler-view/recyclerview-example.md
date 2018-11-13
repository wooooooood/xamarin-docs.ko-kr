---
title: 기본 RecyclerView 예제
description: 예제 앱 RecyclerView를 사용 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/30/2018
ms.openlocfilehash: d71c4f0f3221d06c22876329a5933273d8d6f92d
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526769"
---
# <a name="a-basic-recyclerview-example"></a>기본 RecyclerView 예제

알아야 하는 방법 `RecyclerView` 일반적인 응용 프로그램에서 작동 하는,이 항목에서는 살펴봅니다를 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) 샘플 앱을 사용 하는 간단한 코드 예제 `RecyclerView` 큰 사진 컬렉션을 표시 하려면: 

[![사진 표시할 CardViews를 사용 하는 RecyclerView 앱의 두 가지 스크린샷](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** 사용 하 여 [CardView](~/android/user-interface/controls/card-view.md) 의 각 사진을 항목을 구현 하는 `RecyclerView` 레이아웃 합니다. 때문에 `RecyclerView`의 샘플 앱이 원활 하 고 눈에 띄는 지연 없이 큰 사진 컬렉션을 통해 신속 하 게 스크롤할 수는 성능상의 이점이 있습니다.


### <a name="an-example-data-source"></a>예제 데이터 원본

이 예제에서는 앱에서 "사진 앨범" 데이터 원본 (나타내는 합니다 `PhotoAlbum` 클래스)를 제공 `RecyclerView` 항목 콘텐츠를 사용 하 여 합니다.
`PhotoAlbum` 캡션;를 사용 하 여 사진 컬렉션 를 인스턴스화할 때 32 사진 바로 사용할 수 있는 컬렉션을 가져옵니다.

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

사진 인스턴스마다 `PhotoAlbum` 해당 이미지 리소스 ID를 읽을 수 있도록 허용 하는 속성을 노출 `PhotoID`, 및 해당 캡션 문자열을 `Caption`입니다. 사진 컬렉션은 각 사진에 인덱서 액세스할 수 있도록 구성 됩니다. 예를 들어, 다음 코드 줄에는 이미지 리소스 ID 및 컬렉션의 열 번째 사진의 캡션에 액세스:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` 또한 제공 된 `RandomSwap` 컬렉션의 다른 곳에서 임의로 선택한 사진을 사용 하 여 컬렉션의 첫 번째 사진을 교환에 호출할 수 있는 메서드:

```csharp
mPhotoAlbum.RandomSwap ();
```

때문에 구현 세부 사항 `PhotoAlbum` 이해에 관련 되지 않은 `RecyclerView`, `PhotoAlbum` 소스 코드를 제공 하지 않습니다. 소스 코드를 `PhotoAlbum` 에서 확인할 수 있습니다 [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) 에 [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) 샘플 앱입니다.


### <a name="layout-and-initialization"></a>레이아웃 및 초기화

레이아웃 파일 **Main.axml**, 개로만 이루어진 `RecyclerView` 내에서 한 `LinearLayout`:

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

정규화 된 이름을 사용 해야 합니다 **android.support.v7.widget.RecyclerView** 있으므로 `RecyclerView` 지원 라이브러리에 패키지 됩니다. 합니다 `OnCreate` 메서드의 `MainActivity` 이 레이아웃을 초기화, 어댑터를 인스턴스화하고, 내부 데이터 소스를 준비 합니다.

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

이 코드는 다음을 수행합니다.

1. 인스턴스화하는 `PhotoAlbum` 데이터 원본입니다.

2. 사진 앨범 데이터 원본 어댑터의 생성자에 전달 `PhotoAlbumAdapter` (이 가이드의 뒷부분에 정의 된). 
   참고 하는 것이 데이터 소스를 어댑터의 생성자에 매개 변수로 전달 하는 것이 좋습니다. 

3. 가져옵니다는 `RecyclerView` 레이아웃에서.

4. 어댑터를 연결 합니다 `RecyclerView` 를 호출 하 여 인스턴스를 `RecyclerView` `SetAdapter` 메서드 위에 표시 된 대로 합니다.

### <a name="layout-manager"></a>레이아웃 관리자

각 항목에는 `RecyclerView` 이루어집니다를 `CardView` 사진 이미지 및 사진 캡션을 포함 하는 (세부 정보에 나와 합니다 [뷰 소유자](#view-holder) 섹션 아래). 미리 정의 된 `LinearLayoutManager` 각각의 레이아웃에 `CardView` 를 세로로 스크롤 정렬:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

이 코드는 기본 활동에 있는 `OnCreate` 메서드. 레이아웃 관리자에 게 생성자를 사용 하려면를 *상황에 맞는*이므로 `MainActivity` 사용 하 여 전달 됩니다 `this` 위와 같이 합니다.

미리 정의 된 사용 하는 대신 `LinearLayoutManager`를 두 개를 표시 하는 사용자 지정 레이아웃 관리자를 연결 하 여 `CardView` 항목 side-by-side, 사진 컬렉션을 이동할 페이지 넘기기 애니메이션 효과 구현 합니다. 이 가이드의 뒷부분에 나오는 다른 레이아웃 관리자에서 교환 하 여 레이아웃을 수정 하는 방법의 예가 표시 됩니다.

<a name="view-holder" />

### <a name="view-holder"></a>뷰 소유자

뷰 소유자 클래스 라고 `PhotoViewHolder`합니다. 각 `PhotoViewHolder` 에 대 한 참조를 보유 하는 인스턴스를 `ImageView` 하 고 `TextView` 에 배치 되는 연결 된 행 항목의는 `CardView` 여기 다이어그램으로:

[![ImageView 및 TextView를 포함 하는 CardView 다이어그램](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` 파생 `RecyclerView.ViewHolder` 에 대 한 참조를 저장 하는 속성을 포함 합니다 `ImageView` 및 `TextView` 위의 레이아웃에 표시 된 것입니다.
`PhotoViewHolder` 두 속성 및 생성자가 하나 이루어져 있습니다.

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
이 코드 예제에서는 합니다 `PhotoViewHolder` 생성자에는 부모 항목 보기에 대 한 참조가 전달 됩니다 (합니다 `CardView`)는 `PhotoViewHolder` 래핑합니다. 항상 전달 하면 부모 항목 보기 기본 생성자를 note 합니다. 합니다 `PhotoViewHolder` 생성자 호출 `FindViewById` 각 해당 자식 뷰 참조를 찾으려는 부모 항목 보기에서 `ImageView` 및 `TextView`에 결과 저장 합니다 `Image` 및 `Caption` 속성을 각각. 어댑터가이 업데이트할 때 이러한 속성에서 참조 보기 나중에 검색 `CardView`의 새 데이터를 사용 하 여 자식 뷰.

에 대 한 자세한 내용은 `RecyclerView.ViewHolder`를 참조 합니다 [RecyclerView.ViewHolder 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)합니다.


### <a name="adapter"></a>어댑터

각 어댑터 로드 `RecyclerView` 특정 사진에 대 한 데이터가 포함 된 행입니다. 행 위치 지정된 사진에 대 한 *P*, 어댑터 위치에 연결 된 데이터를 찾는 예를 들어 *P* 위치의 항목 행에이 데이터 내에서 데이터 원본 및 복사본 *P* 에 `RecyclerView` 컬렉션입니다. 어댑터에 대 한 참조를 조회 하는 뷰의 소유자를 사용 합니다 `ImageView` 및 `TextView` 반복적으로 호출 하지 않아도 되므로 해당 위치에서 `FindViewById` 사용자 사진 컬렉션을 스크롤하고 뷰를 다시 사용 하는 대로 해당 보기에 대 한 합니다.

**RecyclerViewer**에 어댑터 클래스에서 파생 됩니다 `RecyclerView.Adapter` 만들려는 `PhotoAlbumAdapter`:

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

`mPhotoAlbum` 생성자에 전달 되는 데이터 원본을 (사진 앨범)를 포함 하는 멤버;이 멤버 변수에 사진 앨범을 복사 하는 생성자입니다. 다음과 같은 필수 `RecyclerView.Adapter` 메서드가 구현 됩니다.

-   **`OnCreateViewHolder`** &ndash; 항목 레이아웃 파일 및 뷰의 소유자를 인스턴스화합니다.

-   **`OnBindViewHolder`** &ndash; 지정 된 보기 소유자에 해당 참조가 저장 되는 뷰를 지정된 된 위치에 데이터를 로드 합니다.

-   **`ItemCount`** &ndash; 데이터 소스의 항목 수를 반환합니다.

레이아웃 관리자 내에서 항목을 배치 하는 동안 이러한 메서드를 호출 합니다 `RecyclerView`합니다. 다음 섹션에서 이러한 메서드 구현이 검사 합니다.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

레이아웃 관리자 호출 `OnCreateViewHolder` 경우는 `RecyclerView` 항목을 나타내는 새 뷰 소유자를 해야 합니다. `OnCreateViewHolder` 뷰의 레이아웃 파일에서 항목 보기를 확장 하 고 새 보기를 래핑하고 `PhotoViewHolder` 인스턴스. 합니다 `PhotoViewHolder` 생성자를 찾아 앞에서 설명한 대로 레이아웃에서 자식 뷰에 대 한 참조를 저장 [뷰 소유자](#view-holder)합니다.

각 행 항목으로 표시 됩니다는 `CardView` 를 포함 하는 `ImageView` (사진)에 대 한 및 `TextView` (캡션)에 대 한 합니다. 이 레이아웃 파일에 있는 **PhotoCardView.axml**:

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

이 레이아웃의 단일 행 항목을 나타내는 `RecyclerView`합니다. 합니다 `OnBindViewHolder` 메서드 (아래 설명 참조)에 데이터 원본에서 데이터를 복사 합니다 `ImageView` 및 `TextView` 이 레이아웃의 합니다.
`OnCreateViewHolder` 사진 지정 된 위치에 대 한이 레이아웃을 확장 합니다 `RecyclerView` 하 고 새 인스턴스화합니다 `PhotoViewHolder` 인스턴스 (찾고에 대 한 참조를 캐시 하는 `ImageView` 및 `TextView` 연결 된 자식 뷰 `CardView` 레이아웃):

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

결과 뷰 소유자 인스턴스로 `vh`, 호출자 (레이아웃 관리자)에 게 반환 됩니다.


#### <a name="onbindviewholder"></a>OnBindViewHolder

레이아웃 관리자에서 특정 보기를 표시할 준비가 되 면 합니다 `RecyclerView`의 표시 화면 영역 어댑터의 호출 `OnBindViewHolder` 데이터 원본에서 콘텐츠를 사용 하 여 지정 된 행 위치에 있는 항목을 입력 하는 방법입니다. `OnBindViewHolder` 지정 된 행 위치 (사진의 이미지 리소스 및 사진의 캡션에 대 한 문자열)에 대 한 사진 정보를 가져옵니다 하 고 관련된 보기에이 데이터를 복사 합니다. 뷰는 뷰 소유자 개체에 저장 된 참조를 통해 배치 (통해 전달 되는 `holder` 매개 변수):

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

전달 된 뷰 소유자 개체를 먼저 파생 된 뷰 소유자 형식으로 캐스팅 해야 합니다 (이 예제의 경우 `PhotoViewHolder`) 사용 하기 전에 합니다.
어댑터 보기 보유자가 참조 되는 뷰 이미지 리소스를 로드 `Image` 속성과 캡션 텍스트 보기 보유자가 참조 되는 뷰 복사 `Caption` 속성입니다. 이렇게 *바인딩합니다* 데이터와 관련 된 보기입니다.

`OnBindViewHolder` 데이터의 구조를 사용 하 여 직접 처리 하는 코드입니다. 이 예에서 `OnBindViewHolder` 매핑하는 방법에 대 한 이해를 `RecyclerView` 항목 데이터 원본에 연결 된 데이터 항목의 위치입니다. 매핑을 간단한이 경우에 사진 앨범;에 위치 배열 인덱스로 사용 될 수 있으므로 그러나 더 복잡 한 데이터 원본에는 이러한 매핑을 설정 하려면 추가 코드가 필요할 수 있습니다.


#### <a name="itemcount"></a>ItemCount

`ItemCount` 메서드 데이터 컬렉션의 항목 수를 반환 합니다. 예제에서는 사진 뷰어 앱 항목 횟수가 많은 사진 앨범에서 사진:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

에 대 한 자세한 내용은 `RecyclerView.Adapter`를 참조 합니다 [RecyclerView.Adapter 클래스 참조](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html)합니다.


### <a name="putting-it-all-together"></a>모든 함께 배치

결과 `RecyclerView` 예제에서는 사진 앱에 대 한 구현으로 이루어져 `MainActivity` 데이터 원본, 레이아웃 관리자 및 어댑터를 만드는 코드입니다. `MainActivity` 만듭니다는 `mRecyclerView` 인스턴스, 데이터 원본 및 어댑터를 인스턴스화하고 레이아웃 관리자와 어댑터에 연결 합니다.

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

`PhotoViewHolder` 찾아서 보기 파일에 대 한 참조를 캐시 합니다.

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

`PhotoAlbumAdapter` 세 가지 필수 메서드 재정의 구현합니다.

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

이 코드를 컴파일 및 실행 하는 경우 다음 스크린샷과에서 같이 앱을 보기 기본 사진을 생성 됩니다.

[![사진 보기 사진 카드를 세로 방향으로 스크롤 하는 앱의 두 가지 스크린샷](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

(처럼 위의 스크린샷에) 그림자를 그릴 되 고, 편집할 **Properties/AndroidManifest.xml** 에 다음 특성 설정을 추가 하 고는 `<application>` 요소:

```xml
android:hardwareAccelerated="true"
```

이 기본 앱 사진 앨범의 검색만 지원 합니다. 항목-터치 이벤트에 응답 하지 않는 것도 기본 데이터의 변경 내용을 처리 합니다. 이 기능이 추가 됨 [RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)합니다.




### <a name="changing-the-layoutmanager"></a>LayoutManager 변경

때문에 `RecyclerView`의 유연성을 제공 하기 쉽습니다 다른 레이아웃 관리자를 사용 하도록 앱을 수정 합니다. 다음 예제에서는 세로 선형 레이아웃을 사용 하지 않고 가로 방향으로 스크롤 하는 표 형태 레이아웃을 사용 하 여 사진 앨범에 표시할 수정입니다. 이 위해 레이아웃 관리자 인스턴스화를 사용 하도록 수정 됩니다는 `GridLayoutManager` 다음과 같습니다.

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

이 코드 변경이 대체 세로 `LinearLayoutManager` 사용 하 여는 `GridLayoutManager` 가로 방향으로 스크롤 하는 두 행의 구성 된 표를 표시 하는 합니다. 를 컴파일하고 앱을 다시 실행 하는 경우 표에 사진 표시 되는 및 스크롤은 세로 대신 가로 표시 됩니다.

[![표에 사진 가로 스크롤을 사용 하 여 앱의 스크린샷 예제](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

한 줄의 코드를 변경 하 여 사진 보기 응용 프로그램에서 다른 레이아웃을 사용 하 여 다른 동작을 사용 하 여 수정 하는 것이 같습니다.
알림 어댑터 코드 아니고 레이아웃 XML 레이아웃 스타일을 변경 하도록 수정 했습니다. 

다음 항목인 [RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md),이 기본 샘플 앱 항목 클릭 이벤트를 처리 하 고 업데이트 하도록 확장 되었습니다 `RecyclerView` 때 기본 데이터 원본 변경 합니다.



## <a name="related-links"></a>관련 링크

- [RecyclerViewer (샘플)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView 파트 및 기능](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [RecyclerView 예제 확장](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
