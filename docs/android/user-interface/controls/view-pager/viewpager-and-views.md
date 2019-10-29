---
title: 보기가 있는 ViewPager
description: ViewPager는 gestural 탐색을 구현할 수 있도록 하는 레이아웃 관리자입니다. Gestural 탐색을 사용 하면 사용자가 왼쪽 및 오른쪽으로 이동 하 여 데이터 페이지를 단계별로 이동할 수 있습니다. 이 가이드에서는 뷰를 데이터 페이지로 사용 하 여 ViewPager 및 PagerTabStrip를 사용 하 여 swipeable UI를 구현 하는 방법에 대해 설명 합니다. 이후 가이드에서는 페이지에 대 한 조각을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: 65a613f229f04a4ab01ca73a9c53c026add49f84
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029043"
---
# <a name="viewpager-with-views"></a>보기가 있는 ViewPager

_ViewPager는 gestural 탐색을 구현할 수 있도록 하는 레이아웃 관리자입니다. Gestural 탐색을 사용 하면 사용자가 왼쪽 및 오른쪽으로 이동 하 여 데이터 페이지를 단계별로 이동할 수 있습니다. 이 가이드에서는 뷰를 데이터 페이지로 사용 하 여 ViewPager 및 PagerTabStrip를 사용 하 여 swipeable UI를 구현 하는 방법에 대해 설명 합니다. 이후 가이드에서는 페이지에 대 한 조각을 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

이 가이드는 `ViewPager`를 사용 하 여 낙 엽 및가 중 트리의 이미지 갤러리를 구현 하는 방법에 대 한 단계별 데모를 제공 하는 연습입니다. 이 앱에서 사용자는 "트리 카탈로그"를 swipes 하 여 트리 이미지를 볼 수 있습니다. 카탈로그의 각 페이지 맨 위에서 트리의 이름이`PagerTabStrip`에 나열 되 고 트리의 이미지가 `ImageView`에 표시 되는 경우 어댑터는 기본 데이터 모델에 대 한 `ViewPager`를 인터페이스 하는 데 사용 됩니다. 이 앱은 `PagerAdapter`에서 파생 된 어댑터를 구현 합니다. 

`ViewPager`기반 앱은 `Fragment`s를 사용 하 여 구현 되는 경우가 많지만 `Fragment`s의 추가 복잡성이 필요 하지 않은 비교적 간단한 사용 사례가 있습니다. 예를 들어이 연습에서 설명 하는 기본 이미지 갤러리 앱은 `Fragment`s를 사용할 필요가 없습니다. 콘텐츠는 정적이 고 사용자는 서로 다른 이미지 사이에서 앞뒤로 swipes 표준 Android 보기 및 레이아웃을 사용 하 여 구현을 보다 간단 하 게 유지할 수 있습니다. 

## <a name="start-an-app-project"></a>앱 프로젝트 시작

**TreePager** 라는 새 android 프로젝트를 만듭니다 (새 android 프로젝트를 만드는 방법에 대 한 자세한 내용은 [Hello, android](~/android/get-started/hello-android/hello-android-quickstart.md) 참조). 그런 다음 NuGet 패키지 관리자를 시작 합니다. NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 [연습: 프로젝트에 Nuget 포함](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)을 참조 하세요. **Android 지원 라이브러리 v4**찾기 및 설치: 

[NuGet 패키지 관리자에서 선택한 v4 Nuget 지원의![스크린샷](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

그러면 **Android Support Library v4**로 reaquired 추가 패키지도 설치 됩니다.

## <a name="add-an-example-data-source"></a>예제 데이터 소스 추가

이 예제에서 `TreeCatalog` 클래스가 나타내는 트리 카탈로그 데이터 소스는 항목 내용으로 `ViewPager`를 제공 합니다. 
`TreeCatalog`에는 어댑터가 `View`를 만드는 데 사용 하는 트리 이미지 및 트리 제목의 미리 작성 된 컬렉션이 포함 됩니다. `TreeCatalog` 생성자에는 인수가 필요 하지 않습니다.

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

`TreeCatalog`의 이미지 컬렉션은 각 이미지에 인덱서를 통해 액세스할 수 있도록 구성 되어 있습니다. 예를 들어 다음 코드 줄은 컬렉션의 세 번째 이미지에 대 한 이미지 리소스 ID를 검색 합니다. 

```csharp
int imageId = treeCatalog[2].imageId;
```

`TreeCatalog`의 구현 세부 정보는 `ViewPager`를 이해 하는 것과 관련이 없기 때문에 `TreeCatalog` 코드는 여기에 나열 되어 있지 않습니다. `TreeCatalog`에 대 한 소스 코드는 [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)에서 사용할 수 있습니다. 이 소스 파일을 다운로드 하거나 코드를 복사 하 여 새 **TreeCatalog.cs** 파일에 붙여 넣은 다음 프로젝트에 추가 합니다. 또한 [이미지 파일](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) 을 다운로드 하 여 **리소스/그릴** 수 있는 폴더에 압축을 풀고 프로젝트에 포함 합니다. 

## <a name="create-a-viewpager-layout"></a>ViewPager 레이아웃 만들기

**Resources/layout/Main. axml** 을 열고 내용을 다음 xml로 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</android.support.v4.view.ViewPager>

```csharp
This XML defines a `ViewPager` that occupies the entire screen. Note that
you must use the fully-qualified name **android.support.v4.view.ViewPager**
because `ViewPager` is packaged in a support library. `ViewPager` is
available only from 
[Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/);
it is not available in the Android SDK. 

## Set up ViewPager

Edit **MainActivity.cs** and add the following `using` statement:

```csharp
using Android.Support.V4.View;
```

`OnCreate` 메서드를 다음 코드로 바꿉니다.

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    TreeCatalog treeCatalog = new TreeCatalog();
}
```

이 코드는 다음을 수행 합니다.

1. **주. axml** 레이아웃 리소스에서 뷰를 설정 합니다.

2. 레이아웃에서 `ViewPager`에 대 한 참조를 검색 합니다.

3. 새 `TreeCatalog`를 데이터 소스로 인스턴스화합니다.

이 코드를 작성 하 고 실행 하면 다음 스크린샷에 표시 됩니다. 

[빈 ViewPager를 표시 하는 앱의![스크린샷](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

이 시점에서 **TreeCatalog**의 콘텐츠에 액세스 하기 위한 어댑터가 부족 하기 때문에 `ViewPager` 비어 있습니다. 다음 섹션에서는 `ViewPager`을 **TreeCatalog**에 연결 하기 위해 **PagerAdapter** 를 만듭니다. 

## <a name="create-the-adapter"></a>어댑터 만들기

`ViewPager`는 `ViewPager`와 데이터 원본 사이에 있는 어댑터 컨트롤러 개체를 사용 합니다 ( [어댑터](~/android/user-interface/controls/view-pager/index.md#adapter)의 그림 참조). 이 데이터에 액세스 하기 위해 `ViewPager` `PagerAdapter`에서 파생 된 사용자 지정 어댑터를 제공 해야 합니다. 이 어댑터는 각 `ViewPager` 페이지를 데이터 원본의 콘텐츠로 채웁니다. 이 데이터 소스는 앱 마다 다르므로 사용자 지정 어댑터는 데이터에 액세스 하는 방법을 이해 하는 코드입니다. 사용자가 `ViewPager`페이지를 swipes 어댑터는 데이터 원본에서 정보를 추출 하 여 `ViewPager` 표시 하기 위해 페이지에 로드 합니다. 

`PagerAdapter`를 구현 하는 경우 다음을 재정의 해야 합니다.

- **InstantiateItem** &ndash; 지정 된 위치에 대 한 페이지 (`View`)를 만들고이를 `ViewPager`의 뷰 컬렉션에 추가 합니다. 

- **DestroyItem** &ndash; 지정 된 위치에서 페이지를 제거 합니다.

- **개수** &ndash; 사용 가능한 뷰 (페이지) 수를 반환 하는 읽기 전용 속성입니다. 

- **IsViewFromObject** &ndash;는 페이지가 특정 키 개체와 연결 되어 있는지 여부를 확인 합니다. 이 개체는 `InstantiateItem` 메서드에서 생성 됩니다. 이 예제에서 키 개체는 `TreeCatalog` 데이터 개체입니다.

**TreePagerAdapter.cs** 라는 새 파일을 추가 하 고 해당 내용을 다음 코드로 바꿉니다. 

```csharp
using System;
using Android.App;
using Android.Runtime;
using Android.Content;
using Android.Views;
using Android.Widget;
using Android.Support.V4.View;
using Java.Lang;

namespace TreePager
{
    class TreePagerAdapter : PagerAdapter
    {
        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override bool IsViewFromObject(View view, Java.Lang.Object obj)
        {
            throw new NotImplementedException();
        }

        public override Java.Lang.Object InstantiateItem (View container, int position)
        {
            throw new NotImplementedException();
        }

        public override void DestroyItem(View container, int position, Java.Lang.Object view)
        {
            throw new NotImplementedException();
        }
    }
}
```

이 코드는 필수 `PagerAdapter` 구현을 스텁 합니다. 다음 섹션에서는 이러한 각 메서드가 작업 코드로 대체 되었습니다. 

### <a name="implement-the-constructor"></a>생성자 구현

앱은 `TreePagerAdapter`를 인스턴스화하면 컨텍스트 (`MainActivity`)와 인스턴스화된 `TreeCatalog`를 제공 합니다. **TreePagerAdapter.cs**의 `TreePagerAdapter` 클래스 맨 위에 다음 멤버 변수 및 생성자를 추가 합니다. 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

이 생성자의 목적은 `TreePagerAdapter` 사용할 컨텍스트 및 `TreeCatalog` 인스턴스를 저장 하는 것입니다. 

### <a name="implement-count"></a>개수 구현

`Count` 구현은 비교적 간단 합니다. 트리 카탈로그의 트리 수를 반환 합니다. `Count`를 다음 코드로 바꿉니다.

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`TreeCatalog`의 `NumTrees` 속성은 데이터 집합의 트리 수 (페이지 수)를 반환 합니다.

### <a name="implement-instantiateitem"></a>InstantiateItem 구현

`InstantiateItem` 메서드는 지정 된 위치에 대 한 페이지를 만듭니다. 또한 `ViewPager`의 뷰 컬렉션에 새로 만든 뷰를 추가 해야 합니다. 이러한 작업을 가능 하 게 하기 위해 `ViewPager`은 자신을 컨테이너 매개 변수로 전달 합니다. 

`InstantiateItem` 메서드를 다음 코드로 바꿉니다.

```csharp
public override Java.Lang.Object InstantiateItem (View container, int position)
{
    var imageView = new ImageView (context);
    imageView.SetImageResource (treeCatalog[position].imageId);
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.AddView (imageView);
    return imageView;
}
```

이 코드는 다음을 수행 합니다.

1. 새 `ImageView`을 인스턴스화하여 지정 된 위치에 트리 이미지를 표시 합니다. 앱의 `MainActivity`은 `ImageView` 생성자에 전달 되는 컨텍스트입니다.

2. `ImageView` 리소스를 지정 된 위치의 `TreeCatalog` 이미지 리소스 ID로 설정 합니다.

3. 전달 된 컨테이너 `View` `ViewPager` 참조로 캐스팅 합니다.
    이 캐스트를 제대로 수행 하려면 `JavaCast<ViewPager>()`를 사용 해야 합니다 .이는 Android에서 런타임 검사 형식 변환을 수행 하는 데 필요 합니다.

4. 인스턴스화된 `ImageView` `ViewPager`에 추가 하 고 `ImageView`를 호출자에 게 반환 합니다.

`ViewPager`에 `position`이미지가 표시 되 면이 `ImageView`표시 됩니다. 처음에는 `InstantiateItem`를 두 번 호출 하 여 처음 두 페이지를 뷰로 채웁니다. 사용자가 스크롤하면 현재 표시 된 항목의 바로 뒤에 있는 뷰를 유지 하기 위해 다시 호출 됩니다. 

### <a name="implement-destroyitem"></a>DestroyItem 구현

`DestroyItem` 메서드는 지정 된 위치에서 페이지를 제거 합니다. 지정 된 위치의 보기가 변경 될 수 있는 응용 프로그램에서는 `ViewPager` 새 보기로 바꾸기 전에 해당 위치에서 오래 된 보기를 제거 하는 방법이 있어야 합니다. `TreeCatalog` 예제에서는 각 위치의 뷰가 변경 되지 않으므로 `DestroyItem`에 의해 제거 된 뷰는 해당 위치에 대해 `InstantiateItem`가 호출 될 때만 다시 추가 됩니다. 효율성을 높이기 위해 풀을 구현 하 여 동일한 위치에 다시 표시 되는 `View`를 재활용할 수 있습니다. 

`DestroyItem` 메서드를 다음 코드로 바꿉니다. 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

이 코드는 다음을 수행 합니다.

1. 전달 된 컨테이너 `View`를 `ViewPager` 참조로 캐스팅 합니다.

2. 전달 된 Java 개체 (`view`)를 C#`View`(`view as View`)로 캐스팅 합니다.

3. `ViewPager`에서 뷰를 제거 합니다. 

### <a name="implement-isviewfromobject"></a>IsViewFromObject 구현

사용자가 콘텐츠 페이지에서 왼쪽과 오른쪽으로 이동 하면 `IsViewFromObject`를 호출 하 여 지정 된 위치의 자식 `View`이 동일한 위치에 대 한 어댑터의 개체와 연결 되어 있는지 확인 `ViewPager`. 따라서 어댑터의 개체를 *개체 키* 라고 합니다. ). 비교적 간단한 앱의 경우 연결은 id &ndash; 중 하나 이며, 해당 인스턴스에서 어댑터의 개체 키는 이전에 `InstantiateItem`를 통해 `ViewPager`에 반환 된 뷰입니다. 그러나 다른 앱의 경우에는 개체 키가 해당 위치에 표시 `ViewPager`는 자식 뷰와 연결 된 다른 어댑터별 클래스 인스턴스일 수 있습니다. 어댑터 에서만 전달 된 뷰와 개체 키가 연결 되어 있는지 여부를 알 수 있습니다. 

`PagerAdapter` 제대로 작동 하려면 `IsViewFromObject`를 구현 해야 합니다. 지정 된 `ViewPager` 위치에 대 한 `false` 반환 하 `IsViewFromObject` 경우에는 해당 위치에 뷰가 표시 되지 않습니다. `TreePager` 앱 *에서 `InstantiateItem`에 의해 반환 되는* 개체 키는 트리의 페이지 `View` 이므로 코드는 id를 확인 하기만 하면 됩니다. 즉, 개체 키와 뷰가 서로 동일 하 고 뷰가 있습니다. `IsViewFromObject`를 다음 코드로 바꿉니다. 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```

## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager에 어댑터를 추가 합니다.

`TreePagerAdapter` 구현 되었으므로 이제 `ViewPager`에 추가 해야 합니다. **MainActivity.cs**에서 `OnCreate` 메서드의 끝에 다음 코드 줄을 추가 합니다.

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

이 코드는 `TreePagerAdapter`를 인스턴스화하고 `MainActivity`를 컨텍스트로 전달 합니다 (`this`). 인스턴스화된 `TreeCatalog` 생성자의 두 번째 인수로 전달 됩니다. `ViewPager`의 `Adapter` 속성은 인스턴스화된 `TreePagerAdapter` 개체로 설정 됩니다. 이렇게 하면 `TreePagerAdapter` `ViewPager`에 연결 됩니다. 

이제 코어 구현이 완료 되어 앱을 빌드하고 실행 &ndash;. 다음 스크린샷에 왼쪽에 표시 된 대로 트리 카탈로그의 첫 번째 이미지가 화면에 나타납니다. 왼쪽으로 살짝 밀어 트리 보기를 표시 한 다음 오른쪽으로 살짝 밀어 트리 카탈로그를 통해 다시 이동 합니다. 

[트리 이미지를 통해 TreePager 앱의![스크린샷 살짝 밀기](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>호출기 표시기 추가

이 최소 `ViewPager` 구현에서는 트리 카탈로그의 이미지를 표시 하지만 사용자가 카탈로그 내에 있는 위치에 대 한 표시는 제공 하지 않습니다. 다음 단계는 `PagerTabStrip`을 추가 하는 것입니다. `PagerTabStrip`는 사용자에 게 표시 되는 페이지를 알리고 이전 및 다음 페이지의 힌트를 표시 하 여 탐색 컨텍스트를 제공 합니다. `PagerTabStrip`은 `ViewPager`의 현재 페이지에 대 한 표시기로 사용 하기 위한 것입니다. 각 페이지를 통해 사용자가 swipes 때이를 스크롤하고 업데이트 합니다. 

**리소스/레이아웃/기본. axml** 을 열고 레이아웃에 `PagerTabStrip`를 추가 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <android.support.v4.view.PagerTabStrip
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          android:layout_gravity="top"
          android:paddingBottom="10dp"
          android:paddingTop="10dp"
          android:textColor="#fff" />

</android.support.v4.view.ViewPager>
```

`ViewPager` 및 `PagerTabStrip`는 함께 작동 하도록 설계 되었습니다. `ViewPager` 레이아웃 내에 `PagerTabStrip`을 선언 하면 `ViewPager`에서 자동으로 `PagerTabStrip`를 찾아 어댑터에 연결 합니다. 앱을 빌드하고 실행할 때 각 화면 맨 위에 빈 `PagerTabStrip` 표시 되어야 합니다. 

[빈 PagerTabStrip의![확대/확대 스크린 샷](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)

### <a name="display-a-title"></a>제목 표시

각 페이지 탭에 제목을 추가 하려면 `PagerAdapter`파생 클래스에서 `GetPageTitleFormatted` 메서드를 구현 합니다. `ViewPager` `GetPageTitleFormatted` (구현 된 경우)를 호출 하 여 지정 된 위치에서 페이지를 설명 하는 제목 문자열을 가져옵니다. **TreePagerAdapter.cs**의 `TreePagerAdapter` 클래스에 다음 메서드를 추가 합니다. 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

이 코드는 트리 카탈로그의 지정 된 페이지 (위치)에서 트리 캡션 문자열을 검색 하 고, Java `String`로 변환한 후 `ViewPager`에 반환 합니다. 이 새 메서드를 사용 하 여 앱을 실행 하면 각 페이지에 `PagerTabStrip`의 트리 캡션이 표시 됩니다. 화면 맨 위에 밑줄 없이 트리 이름이 표시 됩니다. 

[텍스트 채우기 PagerTabStrip 탭이 있는 페이지의![스크린샷](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

앞뒤로 살짝 밀어 카탈로그에서 각 캡션 트리 이미지를 볼 수 있습니다. 

### <a name="pagertitlestrip-variation"></a>PagerTitleStrip 변형

`PagerTitleStrip`은 현재 선택 된 탭에 밑줄을 추가 `PagerTabStrip` 한다는 점을 제외 하 고 `PagerTabStrip`와 매우 비슷합니다. 위의 레이아웃에서 `PagerTabStrip` `PagerTitleStrip`로 바꾸고 앱을 다시 실행 하 여 `PagerTitleStrip`표시 되는 모양을 확인할 수 있습니다. 

[텍스트에서 밑줄이 제거 된![PagerTitleStrip](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

`PagerTitleStrip`로 변환할 때 밑줄이 제거 됩니다. 

## <a name="summary"></a>요약

이 연습에서는 `Fragment`를 사용 하지 않고 기본적인 `ViewPager`기반 앱을 빌드하는 방법에 대 한 단계별 예제를 제공 했습니다. 이미지 및 캡션 문자열을 포함 하는 예제 데이터 소스, 이미지를 표시 하는 `ViewPager` 레이아웃 및 데이터 소스에 `ViewPager`를 연결 하는 `PagerAdapter` 하위 클래스를 제공 합니다. 사용자가 데이터 집합을 탐색 하는 데 도움이 되도록 `PagerTabStrip` 또는 `PagerTitleStrip`를 추가 하 여 각 페이지의 맨 위에 이미지 캡션을 표시 하는 방법을 설명 하는 지침이 포함 되어 있습니다. 

## <a name="related-links"></a>관련 링크

- [TreePager (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-treepager)
