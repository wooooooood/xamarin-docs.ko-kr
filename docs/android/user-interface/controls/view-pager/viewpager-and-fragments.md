---
title: 조각이 있는 ViewPager
description: ViewPager는 gestural 탐색을 구현할 수 있도록 하는 레이아웃 관리자입니다. Gestural 탐색을 사용 하면 사용자가 왼쪽 및 오른쪽으로 이동 하 여 데이터 페이지를 단계별로 이동할 수 있습니다. 이 가이드에서는 데이터 페이지로 조각을 사용 하 여 ViewPager로 swipeable UI를 구현 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 7a30d438b5340f071dd3c1c785f1a3fae0485ade
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764580"
---
# <a name="viewpager-with-fragments"></a>조각이 있는 ViewPager

_ViewPager는 gestural 탐색을 구현할 수 있도록 하는 레이아웃 관리자입니다. Gestural 탐색을 사용 하면 사용자가 왼쪽 및 오른쪽으로 이동 하 여 데이터 페이지를 단계별로 이동할 수 있습니다. 이 가이드에서는 데이터 페이지로 조각을 사용 하 여 ViewPager로 swipeable UI를 구현 하는 방법에 대해 설명 합니다._

## <a name="overview"></a>개요

`ViewPager`는에서 `ViewPager`각 페이지의 수명 주기를 보다 쉽게 관리할 수 있도록 조각과 함께 사용 되는 경우가 많습니다. 이 연습에서는를 `ViewPager` 사용 하 여 플래시 카드에 일련의 수학 문제를 제공 하는 **FlashCardPager** 라는 앱을 만듭니다. 각 플래시 카드는 조각으로 구현 됩니다. 사용자는 플래시 카드를 왼쪽 및 오른쪽으로 swipes 수학 문제를 탭 하 여 답변을 표시 합니다. 이 앱은 각 `Fragment` 플래시 카드의 인스턴스를 만들고에서 `FragmentPagerAdapter`파생 된 어댑터를 구현 합니다. [Viewpager 및 Views](~/android/user-interface/controls/view-pager/viewpager-and-views.md)에서 대부분의 작업은 `MainActivity` 수명 주기 메서드에서 수행 되었습니다. **FlashCardPager**에서 대부분의 작업은 수명 주기 방법 중 하나 `Fragment` 에서에 의해 수행 됩니다. 

Xamarin의 조각에 익숙하지 않은 경우이 가이드 &ndash; 는 조각에 대 한 기본 사항에 대해 다루지 않습니다. 조각 시작에 도움이 되는 [조각](~/android/platform/fragments/index.md) 을 참조 하세요. 

## <a name="start-an-app-project"></a>앱 프로젝트 시작

**FlashCardPager**라는 새 Android 프로젝트를 만듭니다. 그런 다음 nuget 패키지 관리자를 시작 합니다 (nuget 패키지를 설치 하 [는 방법에 대 한 자세한 내용은 연습: 프로젝트](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)에 NuGet 포함). [Viewpager 및 Views](~/android/user-interface/controls/view-pager/viewpager-and-views.md)에 설명 된 대로 **xamarin.ios** 패키지를 찾아 설치 합니다. 

## <a name="add-an-example-data-source"></a>예제 데이터 소스 추가

**FlashCardPager**에서 데이터 원본은 `FlashCardDeck` 클래스로 표현 되는 플래시 카드의 데크입니다 .이 `ViewPager` 데이터 소스는 항목 콘텐츠를 제공 합니다. `FlashCardDeck`수학 문제와 답변의 미리 만들어진 컬렉션을 포함 합니다. 생성자 `FlashCardDeck` 에는 인수가 필요 하지 않습니다. 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

에서 `FlashCardDeck` 플래시 카드의 컬렉션은 각 플래시 카드에 인덱서를 통해 액세스할 수 있도록 구성 되어 있습니다. 예를 들어 다음 코드 줄은 데크에서 네 번째 플래시 카드 문제를 검색 합니다. 

```csharp
string problem = flashCardDeck[3].Problem;
```

이 코드 줄은 이전 문제에 대 한 해당 대답을 검색 합니다.

```csharp
string answer = flashCardDeck[3].Answer;
```

의 `FlashCardDeck` 구현 세부 정보는 이해 `ViewPager`와 관련이 없으므로 코드는 `FlashCardDeck` 여기에 나열 되지 않습니다.
소스 코드 `FlashCardDeck` 는 [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)에서 사용할 수 있습니다.
이 소스 파일을 다운로드 하거나 코드를 복사 하 여 새 **FlashCardDeck.cs** 파일에 붙여 넣은 다음 프로젝트에 추가 합니다.

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
```

이 XML은 전체 `ViewPager` 화면을 차지 하는를 정의 합니다. 이 지원 라이브러리에 패키지 되어 있기 때문 `ViewPager` 에 정규화 된 이름 **android** . v a v. `ViewPager`[Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)에서만 사용할 수 있습니다. Android SDK에서 사용할 수 없습니다.

## <a name="set-up-viewpager"></a>ViewPager 설정

**MainActivity.cs** 를 편집 하 고 다음 `using` 문을 추가 합니다.

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

`MainActivity` 에서`FragmentActivity`파생 되도록 클래스 선언을 변경 합니다.

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity`는 조각 지원을`FragmentActivity` 관리 하는 `Activity`방법을 알고 `FragmentActivity` 있으므로가 아니라에서 파생 됩니다. `OnCreate` 메서드를 다음 코드로 바꿉니다. 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

이 코드는 다음을 수행 합니다.

1. **주. axml** 레이아웃 리소스에서 뷰를 설정 합니다.

2. 레이아웃 `ViewPager` 에서에 대 한 참조를 검색 합니다.

3. 새 `FlashCardDeck` 를 데이터 소스로 인스턴스화합니다.

이 코드를 작성 하 고 실행 하면 다음 스크린샷에 표시 됩니다. 

[![빈 ViewPager를 사용 하는 FlashCardPager 앱의 스크린샷](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

이 시점 `ViewPager` 에서는를 `ViewPager`채우는 데 사용 되는 조각이 부족 하 고 **FlashCardDeck**의 데이터에서 이러한 조각을 만들기 위한 어댑터가 부족 하기 때문에 비어 있습니다. 

다음 섹션 `FlashCardFragment` 에서는를 만들어 각 플래시 카드의 기능을 구현 하 `FragmentPagerAdapter` 고,를 만들어의 데이터에서 만든 조각 `ViewPager` `FlashCardDeck`에 연결 합니다. 

## <a name="create-the-fragment"></a>조각 만들기

각 플래시 카드는 이라는 `FlashCardFragment`UI 조각으로 관리 됩니다. `FlashCardFragment`보기는 단일 플래시 카드에 포함 된 정보를 표시 합니다. 의 `FlashCardFragment` 각 인스턴스는에서 호스팅됩니다 `ViewPager`. 
`FlashCardFragment`의 보기는 플래시 카드 문제 `TextView` 텍스트를 표시 하는로 구성 됩니다. 이 뷰는 사용자가 플래시 카드 질문을 누를 `Toast` 때를 사용 하 여 답변을 표시 하는 이벤트 처리기를 구현 합니다. 

### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment 레이아웃 만들기

을 `FlashCardFragment` 구현 하려면 먼저 해당 레이아웃을 정의 해야 합니다. 이 레이아웃은 단일 조각에 대 한 조각 컨테이너 레이아웃입니다. **Flashcard_layout**라는 **리소스/레이아웃** 에 새 Android 레이아웃을 추가 합니다. **Resources/layout/flashcard_layout** 을 열고 내용을 다음 코드로 바꿉니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/flash_card_question"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:textAppearance="@android:style/TextAppearance.Large"
            android:textSize="100sp"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:text="Question goes here" />
    </RelativeLayout>
```

이 레이아웃은 단일 플래시 카드 조각을 정의 합니다. 각 조각은 큼 (100sp `TextView` ) 글꼴을 사용 하 여 수학 문제를 표시 하는로 구성 됩니다. 이 텍스트는 플래시 카드에서 세로 및 가로로 가운데에 배치 됩니다. 

### <a name="create-the-initial-flashcardfragment-class"></a>초기 FlashCardFragment 클래스 만들기

**FlashCardFragment.cs** 라는 새 파일을 추가 하 고 해당 내용을 다음 코드로 바꿉니다.

```csharp
using System;
using Android.OS;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    public class FlashCardFragment : Android.Support.V4.App.Fragment
    {
        public FlashCardFragment() { }

        public static FlashCardFragment newInstance(String question, String answer)
        {
            FlashCardFragment fragment = new FlashCardFragment();
            return fragment;
        }
        public override View OnCreateView (
            LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
        {
            View view = inflater.Inflate (Resource.Layout.flashcard_layout, container, false);
            TextView questionBox = (TextView)view.FindViewById (Resource.Id.flash_card_question);
            return view;
        }
    }
}
```

이 코드는 플래시 카드를 `Fragment` 표시 하는 데 사용 되는 필수 정의를 스텁 합니다. 는에 `Fragment` `FlashCardFragment` 정의된의지원라이브러리버전`Android.Support.V4.App.Fragment`에서 파생 됩니다. 생성자가 아닌 새 `newInstance` `FlashCardFragment` 를 만드는 데 팩터리 메서드가 사용 되도록 생성자가 비어 있습니다. 

수명 `OnCreateView` 주기 메서드는를 `TextView`만들고 구성 합니다. 조각 `TextView` 에 대 한 레이아웃을 늘어납니다 하 고 호출자에 게 `TextView` 팽창을 반환 합니다. `LayoutInflater`및 `ViewGroup` 는 레이아웃을 `OnCreateView` 확장할 수 있도록에 전달 됩니다. 번들 `savedInstanceState` 은를 사용 하 `OnCreateView` 여 저장 된 상태 `TextView` 에서를 다시 만드는 데 사용 하는 데이터를 포함 합니다. 

조각의 뷰는를 `inflater.Inflate`호출 하 여 명시적으로 팽창 됩니다. 인수는 뷰의 부모 이며, 플래그는 inflater에 `false` 게 뷰의 부모에 팽창 뷰를 추가 하지 않도록 지시 합니다 .이 플래그는 나중에이에 있는 어댑터 `ViewPager` 의 `GetItem` 메서드를 호출할 때 추가 됩니다. `container` 연습). 

### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment에 상태 코드 추가

작업과 마찬가지로 조각에는 `Bundle` 상태를 저장 하 고 검색 하는 데 사용 하는가 있습니다. FlashCardPager`Bundle` 에서 연결 된 플래시 카드의 질문과 대답 텍스트를 저장 하는 데 사용 됩니다. **FlashCardFragment.cs**에서 `FlashCardFragment` 클래스 정의의 맨 `Bundle` 위에 다음 키를 추가 합니다. 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

팩터리 메서드 `newInstance` 를 수정 하 여 개체를 `Bundle` 만들고 위의 키를 사용 하 여 전달 된 질문 및 대답 텍스트를 인스턴스화한 후 조각에 저장 합니다. 

```csharp
public static FlashCardFragment newInstance(String question, String answer)
{
    FlashCardFragment fragment = new FlashCardFragment();

    Bundle args = new Bundle();
    args.PutString(FLASH_CARD_QUESTION, question);
    args.PutString(FLASH_CARD_ANSWER, answer);
    fragment.Arguments = args;

    return fragment;
}
```

조각 수명 주기 메서드 `OnCreateView` 를 수정 하 여 전달 된 번들에서이 정보를 검색 하 고 질문 텍스트를에 로드 합니다. `TextBox` 

```csharp
public override View OnCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState)
{
    string question = Arguments.GetString(FLASH_CARD_QUESTION, "");
    string answer = Arguments.GetString(FLASH_CARD_ANSWER, "");

    View view = inflater.Inflate(Resource.Layout.flashcard_layout, container, false);
    TextView questionBox = (TextView)view.FindViewById(Resource.Id.flash_card_question);
    questionBox.Text = question;

    return view;
}
```

`answer` 변수는 여기에서 사용 되지 않지만 나중에이 파일에 이벤트 처리기 코드가 추가 될 때 사용 됩니다. 

## <a name="create-the-adapter"></a>어댑터 만들기

`ViewPager`는 `ViewPager` 와 데이터 원본 사이에 있는 어댑터 컨트롤러 개체를 사용 합니다 (viewpager [어댑터](~/android/user-interface/controls/view-pager/index.md#adapter) 문서의 그림 참조). 이 데이터 `ViewPager` 에 액세스 하려면에서 `PagerAdapter`파생 된 사용자 지정 어댑터를 제공 해야 합니다. 이 예제에서는 조각을 사용 `FragmentPagerAdapter` 하므로에서 `PagerAdapter`파생 된를 사용 &ndash; `FragmentPagerAdapter` 합니다. 
`FragmentPagerAdapter`사용자가 페이지로 돌아올 수 `Fragment` 있는 한, 조각 관리자에서 영구적으로 유지 되는로 각 페이지를 나타냅니다. `ViewPager`사용자가의 페이지를 swipes는 `FragmentPagerAdapter` 데이터 소스에서 정보를 추출 하 고이 `ViewPager` 를 사용 하 여를 `Fragment`표시 하기 위한를 만듭니다. 

을 구현 `FragmentPagerAdapter`하는 경우 다음을 재정의 해야 합니다.

- **개수** &ndash; 사용 가능한 뷰 (페이지) 수를 반환 하는 읽기 전용 속성입니다.

- **GetItem** &ndash; 지정 된 페이지에 대해 표시할 조각을 반환 합니다.

**FlashCardDeckAdapter.cs** 라는 새 파일을 추가 하 고 해당 내용을 다음 코드로 바꿉니다.

```csharp
using System;
using Android.Views;
using Android.Widget;
using Android.Support.V4.App;

namespace FlashCardPager
{
    class FlashCardDeckAdapter : FragmentPagerAdapter
    {
        public FlashCardDeckAdapter (Android.Support.V4.App.FragmentManager fm, FlashCardDeck flashCards)
            : base(fm)
        {
        }

        public override int Count
        {
            get { throw new NotImplementedException(); }
        }

        public override Android.Support.V4.App.Fragment GetItem(int position)
        {
            throw new NotImplementedException();
        }
    }
}
```

이 코드는 필수 `FragmentPagerAdapter` 구현을 스텁 합니다. 다음 섹션에서는 이러한 각 메서드가 작업 코드로 대체 되었습니다. 생성자의 용도는 `FlashCardDeckAdapter`의 기본 클래스 생성자에 조각 관리자를 전달 하는 것입니다. 

### <a name="implement-the-adapter-constructor"></a>어댑터 생성자 구현

앱은를 `FlashCardDeckAdapter`인스턴스화할 때 조각 관리자 및 인스턴스화된 `FlashCardDeck`에 대 한 참조를 제공 합니다. `FlashCardDeckAdapter` **FlashCardDeckAdapter.cs**의 클래스 맨 위에 다음 멤버 변수를 추가 합니다. 

```csharp
public FlashCardDeck flashCardDeck;
```

`FlashCardDeckAdapter` 생성자에 다음 코드 줄을 추가 합니다. 

```csharp
this.flashCardDeck = flashCards;
```

이 코드 줄은가 사용할 `FlashCardDeck` 인스턴스 `FlashCardDeckAdapter` 를 저장 합니다. 

### <a name="implement-count"></a>개수 구현

`Count` 구현은 비교적 간단 하며, 플래시 카드 데크에서 플래시 카드의 수를 반환 합니다. `Count`를 다음 코드로 바꿉니다. 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```

`NumCards` 의`FlashCardDeck` 속성은 데이터 집합의 플래시 카드 (조각 수) 수를 반환 합니다. 

### <a name="implement-getitem"></a>GetItem 구현

메서드 `GetItem` 는 지정 된 위치와 연결 된 조각을 반환 합니다. 가 `GetItem` 플래시 카드 데크 위치에 대해 호출 되 면 해당 위치에서 플래시 카드 `FlashCardFragment` 문제를 표시 하도록 구성 된을 반환 합니다. `GetItem` 메서드를 다음 코드로 바꿉니다. 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

이 코드는 다음을 수행 합니다.

1. 지정 된 위치에 대 한 `FlashCardDeck` 데크에서 수학 문제 문자열을 조회 합니다. 

2. 지정 된 위치에 대 한 `FlashCardDeck` 데크에서 대답 문자열을 조회 합니다. 

3. `FlashCardFragment` 팩터리 메서드`newInstance`를 호출 하 여 플래시 카드 문제 및 응답 문자열을 전달 합니다. 

4. 해당 위치에 대 한 질문 및 `Fragment` 대답 텍스트가 포함 된 새 플래시 카드를 만들어 반환 합니다. 

`position` 에서을 `ViewPager` `TextBox` 렌더링 `Fragment` 하면플래시카드데크에있는수학문제문자열이포함된`position`이 표시 됩니다. 

## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager에 어댑터를 추가 합니다.

가 구현 되었으므로 이제에 추가 `ViewPager`해야 합니다. `FlashCardDeckAdapter` **MainActivity.cs**에서 `OnCreate` 메서드의 끝에 다음 코드 줄을 추가 합니다.

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

이 코드는 첫 `FlashCardDeckAdapter`번째 인수의를 전달 `SupportFragmentManager` 하 여를 인스턴스화합니다. FragmentActivity의 `SupportFragmentManager`속성은 `FragmentManager`&ndash;에 대한 참조를 가져오는데 사용됩니다. `FragmentManager`에 대한 자세한 내용은 [조각관리](~/android/platform/fragments/managing-fragments.md)를 참조하세요. 

이제 코어 구현이 완료 &ndash; 되 고 앱이 실행 됩니다.
다음 스크린샷에 왼쪽에 표시 된 것 처럼 화면에 플래시 카드 데크의 첫 번째 이미지가 표시 됩니다. 왼쪽으로 살짝 밀어 플래시 카드를 확인 한 다음 오른쪽으로 살짝 밀어 플래시 카드 데크를 다시 이동 합니다.

[![호출기 표시기가 없는 FlashCardPager 앱의 예제 스크린샷](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)

## <a name="add-a-pager-indicator"></a>호출기 표시기 추가

이 최소 `ViewPager` 구현은 덱의 각 플래시 카드를 표시 하지만 사용자가 데크 내에 있는 위치에 대 한 표시는 제공 하지 않습니다. 다음 단계는을 `PagerTabStrip`추가 하는 것입니다. 는 `PagerTabStrip` 표시 되는 문제 번호를 사용자에 게 알리고 이전 및 다음 플래시 카드의 힌트를 표시 하 여 탐색 컨텍스트를 제공 합니다. 

**리소스/레이아웃/기본. axml** 을 열고 레이아웃에 `PagerTabStrip` 를 추가 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
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

앱을 빌드하고 실행할 때 각 플래시 카드의 맨 위에 빈 `PagerTabStrip` 가 표시 되어야 합니다. 

[![텍스트가 없는 PagerTabStrip의 확대/확대](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)

### <a name="display-a-title"></a>제목 표시

각 페이지 탭에 제목을 추가 하려면 어댑터에서 메서드를 `GetPageTitleFormatted` 구현 합니다. `ViewPager`( `GetPageTitleFormatted` 구현 된 경우)를 호출 하 여 지정 된 위치에서 페이지를 설명 하는 제목 문자열을 가져옵니다. `FlashCardDeckAdapter` **FlashCardDeckAdapter.cs**의 클래스에 다음 메서드를 추가 합니다. 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

이 코드는 플래시 카드 데크 위치를 문제 번호로 변환 합니다. 결과 문자열은 `ViewPager`로 반환 되는 Java `String` 로 변환 됩니다. 이 새 메서드를 사용 하 여 앱을 실행 하면 각 페이지에 문제 번호가 `PagerTabStrip`표시 됩니다. 

[![각 페이지 위에 표시 되는 문제 번호가 있는 FlashCardPager의 스크린샷](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

앞뒤로 살짝 밀어 각 플래시 카드의 맨 위에 표시 되는 플래시 카드 데크에서 문제 번호를 볼 수 있습니다. 

## <a name="handle-user-input"></a>사용자 입력 처리

**FlashCardPager** 에서는 일련의 조각 기반 플래시 카드 `ViewPager`를 제공 하지만, 아직 각 문제에 대 한 답변을 표시할 수 있는 방법은 없습니다. 이 섹션에서는 이벤트 처리기가에 추가 `FlashCardFragment` 되어 사용자가 플래시 카드 문제 텍스트를 누를 때 답을 표시 합니다. 

**FlashCardFragment.cs** 를 열고 뷰가 호출자에 게 반환 되기 바로 전에 `OnCreateView` 메서드 끝에 다음 코드를 추가 합니다. 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

이 `Click` 이벤트 처리기는 사용자가를 `TextBox`탭 할 때 표시 되는 알림 메시지에 답변을 표시 합니다. 에 전달 된 번들에서 상태 정보를 읽으면 `OnCreateView` 변수가이전에초기화되었습니다.`answer` 앱을 빌드하고 실행 한 다음 각 플래시 카드의 문제 텍스트를 탭 하 여 답변을 확인 합니다. 

[![수학 문제를 탭 할 때 FlashCardPager app 알림을의 스크린샷](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

이 연습에서 제시 하는 **FlashCardPager** 는 `MainActivity` 의 `FragmentActivity`파생 된를 사용 하지만에서 `AppCompatActivity` 파생 `MainActivity` 될 수도 있습니다 (조각 관리를 지 원하는도 제공). `AppCompatActivity` 예제를 보려면 샘플 갤러리에서 [FlashCardPager](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager) 를 참조 하세요.

## <a name="summary"></a>요약

이 연습에서는를 사용 하 여 `ViewPager` `Fragment`기본 기반 앱을 빌드하는 방법에 대 한 단계별 예제를 제공 했습니다. Flash 카드 질문과 대답, `ViewPager` 플래시 카드를 표시 하는 레이아웃 `FragmentPagerAdapter` 및를 `ViewPager` 데이터 원본에 연결 하는 서브 클래스를 포함 하는 예제 데이터 원본을 제공 합니다. 사용자가 플래시 카드를 탐색 하는 데 도움이 되도록를 추가 `PagerTabStrip` 하 여 각 페이지의 맨 위에 문제 번호를 표시 하는 방법을 설명 하는 지침이 포함 되어 있습니다. 마지막으로, 사용자가 플래시 카드 문제를 탭 할 때 답을 표시 하기 위해 이벤트 처리 코드가 추가 되었습니다. 

## <a name="related-links"></a>관련 링크

- [FlashCardPager (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/userinterface-flashcardpager)
