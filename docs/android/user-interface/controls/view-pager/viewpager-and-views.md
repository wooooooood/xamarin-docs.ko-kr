---
title: "보기와 ViewPager"
description: "ViewPager gestural 탐색을 구현할 수 있게 하는 레이아웃 관리자입니다. Gestural 탐색 왼쪽과 오른쪽의 데이터 페이지를 통해 단계로 살짝에 사용자를 수 있습니다. 이 가이드에서는 ViewPager 및 뷰를 사용 하 여 데이터 페이지와 PagerTabStrip swipeable UI를 구현 하는 방법을 설명 (후속 가이드는 페이지 조각을 사용 하는 방법에 설명)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 42E5379F-B0F4-4B87-A314-BF3DE405B0C8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 9c30cf9d76498e95aba6f9a003bc40c7d14e21de
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-views"></a>보기와 ViewPager

_ViewPager gestural 탐색을 구현할 수 있게 하는 레이아웃 관리자입니다. Gestural 탐색 왼쪽과 오른쪽의 데이터 페이지를 통해 단계로 살짝에 사용자를 수 있습니다. 이 가이드에서는 ViewPager 및 뷰를 사용 하 여 데이터 페이지와 PagerTabStrip swipeable UI를 구현 하는 방법을 설명 (후속 가이드는 페이지 조각을 사용 하는 방법에 설명)._

 
## <a name="overview"></a>개요

이 가이드는 단계별 데모를 사용 하는 방법을 제공 하는 연습은 `ViewPager` 낙 및 evergreen 트리의 이미지 갤러리를 구현 하 합니다. 이 응용 프로그램에서 사용자 천공 기와 왼쪽 및 오른쪽 "트리 카탈로그"를 통해 트리 이미지를 볼 수 있습니다. 카탈로그의 각 페이지의 위쪽 트리의 이름은에 나열 됩니다는`PagerTabStrip`, 트리의 이미지에 표시 됩니다는 `ImageView`합니다. 어댑터가 사용 되는 인터페이스에는 `ViewPager` 기본 데이터 모델에 있습니다. 이 앱에서 파생 되는 어댑터 구현 `PagerAdapter`합니다. 

하지만 `ViewPager`-기반된 앱은 종종 사용 하 여 구현 됩니다 `Fragment`s는 비교적 단순한 몇 가지 사례가 있는의 복잡성 `Fragment`s 필요 하지 않습니다. 예를 들어이 연습에서 수행할 기본 이미지 갤러리 응용 프로그램의 사용 필요 하지 않습니다 `Fragment`s입니다. 콘텐츠는 정적이 고는 사용자만 천공 기와 간에 서로 다른 이미지를 구현을 상태로 유지할 수 있지만 간단 표준 Android 보기 및 레이아웃을 사용 하 여 때문에. 



## <a name="start-an-app-project"></a>응용 프로그램 프로젝트를 시작 합니다.

라는 새 Android 프로젝트 만들기 **TreePager** (참조 [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md) 새 Android 프로젝트 만들기에 대 한 자세한 내용은). 그런 다음 NuGet 패키지 관리자를 시작 합니다. (NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 [연습: 프로젝트에 포함 하는 NuGet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). 찾기 및 설치 **Android 지원 라이브러리 v4**: 

[![지원 스크린샷 v4 Nuget NuGet 패키지 관리자를 선택 합니다.](viewpager-and-views-images/01-install-support-lib-sml.png)](viewpager-and-views-images/01-install-support-lib.png#lightbox)

이 설치 하 여 모든 추가 패키지 reaquired **Android 지원 라이브러리 v4**합니다.



## <a name="add-an-example-data-source"></a>예제 데이터 소스 추가

이 예제에서는 트리 카탈로그 데이터 소스 (나타내는 `TreeCatalog` 클래스)를 제공는 `ViewPager` 항목 콘텐츠로 합니다. 
`TreeCatalog` 트리 이미지와 어댑터에서 만드는 데 사용할 트리 제목이 미리 만들어진 컬렉션을 포함 `View`s입니다. `TreeCatalog` 생성자에 인수가 필요 합니다.

```csharp
TreeCatalog treeCatalog = new TreeCatalog();
```

에 이미지의 컬렉션 `TreeCatalog` 인덱서가 각 이미지에 액세스할 수 있도록 구성 됩니다. 예를 들어 다음 코드 줄 컬렉션에 세 번째 이미지에 대 한 이미지 리소스 ID를 검색합니다. 

```csharp
int imageId = treeCatalog[2].imageId;
```

때문에 구현 세부 사항 `TreeCatalog` 이해 관련이 없는 `ViewPager`, `TreeCatalog` 코드가 여기에 나열 되어 있지 합니다. 소스 코드를 `TreeCatalog` 에 [TreeCatalog.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/TreePager/TreeCatalog.cs)합니다. 이 소스 파일을 다운로드 (복사한 새 코드를 붙여 넣습니다. **TreeCatalog.cs** 파일)를 프로젝트에 추가 합니다. 또한 다운로드 하 고 압축을 풀면는 [이미지 파일](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/TreePager/Resources/tree-images.zip?raw=true) 에 프로그램 **리소스/그릴** 폴더를 프로젝트에 포함 합니다. 



## <a name="create-a-viewpager-layout"></a>ViewPager 레이아웃 만들기

열기 **Resources/layout/Main.axml** 해당 내용을 다음 XML로 바꿉니다.

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

이 코드는 다음을 수행합니다.

1.  보기에서 설정 하는 **Main.axml** 레이아웃 리소스입니다.

2.  검색에 대 한 참조는 `ViewPager` 레이아웃에서 합니다.

3.  새 인스턴스화합니다 `TreeCatalog` 데이터 원본으로 합니다.

빌드하고이 코드를 실행 하는 경우 다음 스크린 샷에서 유사한 디스플레이 표시 되어야 합니다. 

[![빈 ViewPager 표시 하는 응용 프로그램의 스크린 샷](viewpager-and-views-images/02-initial-screen-sml.png)](viewpager-and-views-images/02-initial-screen.png#lightbox)

이 시점에서 `ViewPager` 에서 콘텐츠에 액세스 하기 위한 어댑터 되어 있지 않아서 비어 **TreeCatalog**합니다. 다음 섹션에는 **PagerAdapter** 연결할 만들어집니다는 `ViewPager` 에 **TreeCatalog**합니다. 


## <a name="create-the-adapter"></a>어댑터 만들기

`ViewPager` 사이 있는 어댑터 컨트롤러 개체를 사용 하 여는 `ViewPager` 와 데이터 원본 (에 그림을 참조 하십시오 [어댑터](~/android/user-interface/controls/view-pager/index.md#adapter)). 이 데이터에 액세스 하기 위해 `ViewPager` 에서 파생 된 사용자 지정 어댑터를 제공 해야 `PagerAdapter`합니다. 이 어댑터를 채우는 각 `ViewPager` 데이터 소스에서 콘텐츠가 포함 된 페이지입니다. 이 데이터 원본을 응용 프로그램별 이기 때문에 사용자 지정 어댑터 데이터에 액세스 하는 방법을 이해 하는 코드입니다. 페이지를 통해 사용자 천공 기와로 `ViewPager`, 어댑터 정보를 데이터 원본에서 추출에 대 한 페이지에 로드 합니다.는 `ViewPager` 표시 합니다. 

구현 하는 경우는 `PagerAdapter`, 다음 재정의 해야 합니다.

-   **InstantiateItem** &ndash; 페이지를 만듭니다 (`View`) 지정된 된 위치에 대 한에 추가 된 `ViewPager`의 뷰의 컬렉션입니다. 

-   **DestroyItem** &ndash; 지정된 된 위치에서 페이지를 제거 합니다.

-   **Count** &ndash; 보기 (페이지) 사용할 수 있는 수를 반환 하는 읽기 전용 속성입니다. 

-   **IsViewFromObject** &ndash; 페이지는 특정 키 개체와 연결 인지를 확인 합니다. (이 개체가 만들어집니다는 `InstantiateItem` 메서드.) 키 개체의이 예제는 `TreeCatalog` 데이터 개체입니다.

라는 새 파일 추가 **TreePagerAdapter.cs** 해당 내용을 다음 코드로 바꿉니다. 

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

이 코드는 필수적이 지 끊고 `PagerAdapter` 구현 합니다. 다음 섹션에서 이러한 각 방법의 작업 코드로 대체 됩니다. 



### <a name="implement-the-constructor"></a>생성자를 구현 합니다.

앱을 인스턴스화하는 `TreePagerAdapter`, 컨텍스트를 제공 (의 `MainActivity`)와 인스턴스화된 `TreeCatalog`합니다. 다음 멤버 변수와 생성자의 맨 위에 추가 `TreePagerAdapter` 클래스 **TreePagerAdapter.cs**: 

```csharp
Context context;
TreeCatalog treeCatalog;

public TreePagerAdapter (Context context, TreeCatalog treeCatalog)
{
    this.context = context;
    this.treeCatalog = treeCatalog;
}
```

이 생성자의 목적은 컨텍스트를 저장 하는 것 및 `TreeCatalog` 인스턴스는 `TreePagerAdapter` 사용 합니다. 



### <a name="implement-count"></a>구현 개수

`Count` 구현은 비교적 간단: 트리 카탈로그에 있는 트리 수를 반환 합니다. `Count`를 다음 코드로 바꿉니다.

```csharp
public override int Count
{
    get { return treeCatalog.NumTrees; }
}
```

`NumTrees` 속성 `TreeCatalog` 데이터 집합의 트리 (페이지 수)의 수를 반환 합니다.



### <a name="implement-instantiateitem"></a>InstantiateItem 구현

`InstantiateItem` 메서드는 지정된 된 위치에 대 한 페이지를 만듭니다. 새로 만든 뷰를 추가 해야는 `ViewPager`의 컬렉션을 확인 합니다. 이러한 구별이 가능 하 고 `ViewPager` 자체 컨테이너 매개 변수를 전달 합니다. 

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

이 코드는 다음을 수행합니다.

1.  새 인스턴스화합니다 `ImageView` 트리 이미지가 지정된 된 위치에 표시 되도록 합니다. 응용 프로그램의 `MainActivity` 에 전달 되는 컨텍스트는 `ImageView` 생성자입니다.

2.  설정의 `ImageView` 리소스에는 `TreeCatalog` 이미지 리소스 ID 지정된 된 위치에 있습니다.

3.  전달 된 컨테이너를 캐스팅 `View` 에 `ViewPager` 참조 합니다.
    사용 해야 하는 참고 `JavaCast<ViewPager>()` 제대로 (이 필요 하 여 Android에는 런타임 검사 형식 변환을 수행)이이 캐스트를 수행 하려면.

4.  인스턴스화된 추가 `ImageView` 에 `ViewPager` 반환는 `ImageView` 호출자에 게 있습니다.

때는 `ViewPager` 에서 이미지 표시 `position`,이 표시 `ImageView`합니다. 처음에 `InstantiateItem` 뷰가 포함 된 처음 두 페이지를 채우는를 두 번 호출 됩니다. 사용자가 스크롤할 때 바로 뒤와 현재 표시 된 항목 미리 보기를 유지 하기 위해 다시 호출 됩니다. 



### <a name="implement-destroyitem"></a>DestroyItem 구현

`DestroyItem` 메서드는 지정된 된 위치에서 페이지를 제거 합니다. 뷰 위치에서 든 지 변경할 수 있는 앱에서 `ViewPager` 한 새 보기를 교체 하기 전에 해당 위치에서 오래 된 보기를 제거할 경우의 몇 가지 방법이 있어야 합니다. 에 `TreeCatalog` 예제에서는 각 위치에 보기 변경 되지 않으면 보기에 의해 제거 되므로 `DestroyItem` 는 단순히 다시 추가 될 때 `InstantiateItem` 해당 위치에 대해 호출 됩니다. (더 나은 효율성을 높이기 위해 재생할 풀을 구현할 수 하나 `View`s 같은 위치에 다시 표시 됩니다.) 

`DestroyItem` 메서드를 다음 코드로 바꿉니다. 

```csharp
public override void DestroyItem(View container, int position, Java.Lang.Object view)
{
    var viewPager = container.JavaCast<ViewPager>();
    viewPager.RemoveView(view as View);
}
```

이 코드는 다음을 수행합니다.

1.  전달 된 컨테이너를 캐스팅 `View` 에 `ViewPager` 참조 합니다.

2.  전달된 된 Java 개체 캐스팅 (`view`)을 C# `View` (`view as View`);

3.  해당 뷰가 제거는 `ViewPager`합니다. 



### <a name="implement-isviewfromobject"></a>IsViewFromObject 구현

사용자 슬라이드 왼쪽 및 오른쪽의 콘텐츠, 페이지를 통해 `ViewPager` 호출 `IsViewFromObject` 되었는지 확인 하는 자식 `View` 지정된 된 위치에서 같은 위치에 대 한 어댑터의 개체와 연결 된 (이라고 따라서 어댑터의 개체는 *개체 키*). 상대적으로 간단한 응용 프로그램에 대 한 연결 id의 하나는 &ndash; 해당 인스턴스에 어댑터의 개체 키가 이전에 반환 된 보기는 `ViewPager` 통해 `InstantiateItem`합니다. 그러나 다른 앱에 대 한 개체 키 수와 연결 된 몇 가지 다른 어댑터 관련 클래스 인스턴스 (있지만 같지는 않습니다) 자식이 뷰의 `ViewPager` 해당 위치에 표시 됩니다. 어댑터만 전달 된 뷰 및 개체 키 연결 되어 있는지 여부를 알고 있습니다. 

`IsViewFromObject` 에 대 한 구현 해야 `PagerAdapter` 제대로 기능 해야 합니다. 경우 `IsViewFromObject` 반환 `false` 지정된 된 위치에 대 한 `ViewPager` 해당 위치에서 보기를 표시 하지 것입니다. 에 `TreePager` 응용 프로그램, 개체에서 반환 된 키 `InstantiateItem` *은* 페이지 `View` 코드만이 id에 대 한 확인 해야 하므로 트리 (즉, 개체 키 및 보기는 동일). `IsViewFromObject`를 다음 코드로 바꿉니다. 

```csharp
public override bool IsViewFromObject(View view, Java.Lang.Object obj)
{
    return view == obj;
}
```


## <a name="add-the-adapter-to-the-viewpager"></a>어댑터는 ViewPager를 추가

이제는 `TreePagerAdapter` 은 구현 하는 추가 하는 `ViewPager`합니다. **MainActivity.cs**, 코드의 다음 줄의 끝에 추가 `OnCreate` 메서드:

```csharp
viewPager.Adapter = new TreePagerAdapter(this, treeCatalog);
```

이 코드를 인스턴스화하는 `TreePagerAdapter`에 전달 하는 `MainActivity` 컨텍스트로 (`this`). 인스턴스화된 `TreeCatalog` 생성자의 두 번째 인수에 전달 됩니다. `ViewPager`의 `Adapter` 속성은 인스턴스화된에 `TreePagerAdapter` 개체;이 플러그는 `TreePagerAdapter` 에 `ViewPager`합니다. 

핵심 구현을 완료 되었습니다. &ndash; 빌드하고 응용 프로그램을 실행 합니다. 다음 스크린샷에서 왼쪽에 표시 된 것 처럼 화면에 나타나는 트리 카탈로그의 첫 번째 이미지에 표시 됩니다. 살짝 왼쪽에서 더 많은 트리 보기를 참조 한 다음 오른쪽으로 살짝 트리 카탈로그를 통해 다시 이동 하려면: 

[![트리 이미지를 통한 넘기기가 TreePager 스크린샷 앱](viewpager-and-views-images/03-example-views-sml.png)](viewpager-and-views-images/03-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>호출기 표시기 추가

이 최소 `ViewPager` 구현 트리 카탈로그의 이미지를 표시 하지만 사용자가 카탈로그 내에서 지표 없습니다 제공 합니다. 다음 단계는 추가 하는 `PagerTabStrip`합니다. `PagerTabStrip` 어느 페이지가 표시 되 고 이전 및 다음 페이지의 한 힌트를 표시 하 여 탐색 컨텍스트를 제공를 사용자에 게 알립니다. `PagerTabStrip` 현재 페이지에 대 한 표시기로 사용 하도록는 `ViewPager`; 스크롤하고 각 페이지를 통해 사용자 천공 기와로 업데이트 합니다. 

열기 **Resources/layout/Main.axml** 추가 `PagerTabStrip` 레이아웃에:

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

`ViewPager` 및 `PagerTabStrip` 함께 작동 하도록 설계 되었습니다. 선언 하는 경우는 `PagerTabStrip` 내는 `ViewPager` 레이아웃의 `ViewPager` 자동으로 찾을 수는 `PagerTabStrip` 어댑터에 연결 합니다. 빈 빌드하고 앱을 실행할 때 나타납니다 `PagerTabStrip` 각 화면 위쪽에 표시 됩니다. 

[![빈 PagerTabStrip의 클로즈업 스크린 샷](viewpager-and-views-images/04-empty-pagetabstrip-cap-sml.png)](viewpager-and-views-images/04-empty-pagetabstrip-cap.png#lightbox)



### <a name="display-a-title"></a>제목 표시

각 페이지 탭에 제목을 추가 하려면 구현는 `GetPageTitleFormatted` 에서 메서드는 `PagerAdapter`-클래스를 파생 합니다. `ViewPager` 호출 `GetPageTitleFormatted` (구현) 하는 경우 지정된 된 위치에 페이지를 설명 하는 제목 문자열을 가져올 수 있습니다. 다음 메서드를 추가 `TreePagerAdapter` 클래스 **TreePagerAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String(treeCatalog[position].caption);
}
```

이 코드는 트리 카탈로그에서 지정된 된 페이지 (위치)에서 트리 캡션 문자열을 검색, Java로 변환 `String`를 반환 하는 `ViewPager`합니다. 앱이 새 메서드를 실행 하면 각 페이지의 트리 캡션을 표시 됩니다는 `PagerTabStrip`합니다. 밑줄 없이 화면 맨 위에 있는 트리 이름이 표시 되어야 합니다. 

[![텍스트 채워진 PagerTabStrip 탭이 포함 된 페이지의 스크린샷](viewpager-and-views-images/05-final-pagetabstrip-sml.png)](viewpager-and-views-images/05-final-pagetabstrip.png#lightbox)

주고받는 각 캡션된 트리에서 이미지를 보려면 카탈로그 살짝 수 있습니다. 



### <a name="pagertitlestrip-variation"></a>PagerTitleStrip Variation

`PagerTitleStrip` 매우 비슷하지만 `PagerTabStrip` 점을 제외 하 고 `PagerTabStrip` 현재 선택 된 탭에는 밑줄을 추가 합니다. 바꿀 수 `PagerTabStrip` 와 `PagerTitleStrip` 위의 레이아웃 및 앱으로 어떻게 나타나는지 확인을 다시 실행 `PagerTitleStrip`: 

[![텍스트에서 제거 하는 밑줄로 PagerTitleStrip](viewpager-and-views-images/06-pagetitlestrip-example-sml.png)](viewpager-and-views-images/06-pagetitlestrip-example.png#lightbox)

에 밑줄은 제거할를 변환할 때 `PagerTitleStrip`합니다. 


 
## <a name="summary"></a>요약

이 연습에서는 기본을 작성 하는 방법의 단계별 예제를 제공 `ViewPager`-기반 앱을 사용 하지 않고 `Fragment`s입니다. 이미지와 캡션 문자열을 포함 하는 예제 데이터 소스를 표시 한 `ViewPager` 이미지를 표시 하는 레이아웃 및 `PagerAdapter` 연결 하는 하위 클래스는 `ViewPager` 데이터 원본에 합니다. 지침이 포함 된 데이터 집합을 탐색 하는 사용자를 위해 추가 하는 방법을 설명 하는 `PagerTabStrip` 또는 `PagerTitleStrip` 각 페이지의 위쪽 이미지 캡션을 표시 합니다. 


## <a name="related-links"></a>관련 링크

- [TreePager (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/TreePager)
