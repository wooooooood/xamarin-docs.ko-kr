---
title: 조각이 있는 ViewPager
description: ViewPager는 레이아웃 관리자 gestural 탐색을 구현 하면입니다. 왼쪽 및 오른쪽 데이터 페이지를 단계별로 gestural 탐색 안쪽으로 살짝 밀어 사용자를 수 있습니다. 이 가이드에서는 ViewPager, 조각을 사용 하 여 데이터 페이지를 사용 하 여 swipeable UI를 구현 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 1b6e1c8ce91eaad46e779527c5ba12e2187cad24
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61038406"
---
# <a name="viewpager-with-fragments"></a>조각이 있는 ViewPager

_ViewPager는 레이아웃 관리자 gestural 탐색을 구현 하면입니다. 왼쪽 및 오른쪽 데이터 페이지를 단계별로 gestural 탐색 안쪽으로 살짝 밀어 사용자를 수 있습니다. 이 가이드에서는 ViewPager, 조각을 사용 하 여 데이터 페이지를 사용 하 여 swipeable UI를 구현 하는 방법을 설명 합니다._

 
## <a name="overview"></a>개요

`ViewPager` 각 페이지의 수명 주기를 관리할 수 있도록 조각 함께에서 자주 사용 된 `ViewPager`합니다. 이 연습의 `ViewPager` 이라는 앱을 만드는 데 사용 됩니다 **FlashCardPager** 플래시 카드에는 일련의 수학 문제를 표시 하는 합니다. 각 플래시 카드 조각으로 구현 됩니다. 사용자 왼쪽 및 오른쪽 플래시 카드를 통해 천공 기와 및 해당 응답을 표시 하기 위해 수학 문제를 해결할 탭 합니다. 이 앱을 만듭니다는 `Fragment` 어댑터에서 파생 된 각 플래시 카드 및 구현에 대 한 인스턴스 `FragmentPagerAdapter`합니다. [Viewpager 뷰와](~/android/user-interface/controls/view-pager/viewpager-and-views.md), 대부분의 작업에서 수행 된 `MainActivity` 수명 주기 메서드. **FlashCardPager**, 대부분의 작업 수행 됩니다는 `Fragment` 수명 주기 메서드 중 하나에서. 

이 가이드에서는 조각의 기본 사항의 다루지 않습니다 &ndash; 모르는 아직 Xamarin.Android의 조각으로, 참조 [조각을](~/android/platform/fragments/index.md) 조각을 사용 하 여 시작할 수 있도록 합니다. 



## <a name="start-an-app-project"></a>앱 프로젝트를 시작 합니다.

라는 새 Android 프로젝트를 만듭니다 **FlashCardPager**합니다. 그런 다음 NuGet 패키지 관리자를 시작 (NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 하세요. [연습: 프로젝트에서 NuGet을 포함 하 여](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). 찾기 및 설치 합니다 **Xamarin.Android.Support.v4** 에 설명 된 대로 패키지 [Viewpager 뷰와](~/android/user-interface/controls/view-pager/viewpager-and-views.md)합니다. 



## <a name="add-an-example-data-source"></a>예제 데이터 소스 추가

**FlashCardPager**, 데이터 소스를 나타내는 플래시 카드 데크를를 `FlashCardDeck` 클래스를 제공 원본이 데이터는 `ViewPager` 항목 콘텐츠를 사용 하 여 합니다. `FlashCardDeck` 수학 문제의 답을 바로 사용할 수 있는 컬렉션을 포함 합니다. `FlashCardDeck` 생성자에 인수가 필요 합니다. 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

컬렉션에 있는 플래시 카드 `FlashCardDeck` 각 플래시 카드 인덱서에서 액세스할 수 있도록 구성 됩니다. 예를 들어, 다음 코드 줄 묶음에서 네 번째 플래시 카드 문제를 검색합니다. 

```csharp
string problem = flashCardDeck[3].Problem;
```

이 코드 줄 앞에서 설명한 문제가에 대 한 해당 대답을 검색합니다.

```csharp
string answer = flashCardDeck[3].Answer;
```

때문에 구현 세부 사항 `FlashCardDeck` 이해에 관련 되지 않은 `ViewPager`는 `FlashCardDeck` 코드가 여기에 나열 되어 있지.
소스 코드를 `FlashCardDeck` 에서 확인할 수 있습니다 [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)합니다.
이 소스 파일을 다운로드 (또는 복사한 새 코드를 붙여 넣습니다 **FlashCardDeck.cs** 파일) 프로젝트에 추가 합니다.



## <a name="create-a-viewpager-layout"></a>ViewPager 레이아웃 만들기

오픈 **Resources/layout/Main.axml** 다음 XML을 사용 하 여 해당 내용을 바꿉니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.view.ViewPager
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/viewpager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    </android.support.v4.view.ViewPager>
```

이 XML 정의 `ViewPager` 는 전체 화면을 차지 합니다. 정규화 된 이름을 사용 해야 합니다 **android.support.v4.view.ViewPager** 있으므로 `ViewPager` 지원 라이브러리에 패키지 됩니다. `ViewPager` 에서만 사용할 수는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); Android SDK에서 사용할 수 있습니다.


## <a name="set-up-viewpager"></a>ViewPager 설정

편집할 **MainActivity.cs** 추가한 다음 `using` 문:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

변경 된 `MainActivity` 클래스에서 파생 됩니다 있도록 선언 `AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` 파생 됩니다`FragmentActivity` (대신 `Activity`) 때문에 `FragmentActivity` 조각 지원을 관리 하는 방법을 알고 있습니다. `OnCreate` 메서드를 다음 코드로 바꿉니다. 

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);
    ViewPager viewPager = FindViewById<ViewPager>(Resource.Id.viewpager);
    FlashCardDeck flashCards = new FlashCardDeck();
}
```

이 코드는 다음을 수행합니다.

1.  뷰를 설정 합니다 **Main.axml** 레이아웃 리소스입니다.

2.  에 대 한 참조를 검색 합니다 `ViewPager` 레이아웃에서.

3.  새 인스턴스화합니다 `FlashCardDeck` 데이터 원본으로 합니다.

를 빌드 및이 코드를 실행 하는 경우 표시와 관련 된 다음 스크린샷과 유사한 표시 됩니다. 

[![비어 있는 ViewPager 사용 하 여 앱의 FlashCardPager 스크린 샷](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

이 시점에서 합니다 `ViewPager` 채우기 사용 되는 조각 부족 하기 때문에 비어 있지는를 `ViewPager`, 및 데이터에서 이러한 조각을 만들기 위한 어댑터 되어 있지 **FlashCardDeck**합니다. 

다음 섹션에서는 `FlashCardFragment` 만들 각 플래시 카드의 기능을 구현 하는 및 `FragmentPagerAdapter` 연결할 만들어집니다 합니다 `ViewPager` 데이터에서 만든 조각을 `FlashCardDeck`합니다. 



## <a name="create-the-fragment"></a>조각 만들기

플래시 카드 각 호출을 UI 조각으로 관리 되는 `FlashCardFragment`합니다. `FlashCardFragment`보기는 단일 플래시 카드를 사용 하 여 포함 된 정보에 표시 됩니다. 인스턴스마다 `FlashCardFragment` 에서 호스팅되는 `ViewPager`합니다. 
`FlashCardFragment`뷰 구성 되는 `TextView` 플래시 카드 문제가 되는 텍스트를 표시 하는 합니다. 이 보기를 사용 하는 이벤트 처리기를 구현 하는 한 `Toast` 사용자 플래시 카드 질문을 누르면 답을 표시 합니다. 



### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment 레이아웃 만들기

전에 `FlashCardFragment` 수 구현 하 고, 해당 레이아웃을 정의 해야 합니다. 이 레이아웃은 하나의 조각에 대 한 조각 컨테이너 레이아웃입니다. 새 Android 레이아웃을 추가 **리소스/레이아웃** 호출 **flashcard_layout.axml**합니다. 오픈 **Resources/layout/flashcard_layout.axml** 내용을 다음 코드로 바꿉니다. 

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

이 레이아웃 정의 플래시 카드 조각; 각 조각으로 구성 됩니다는 `TextView` 큰 (100sp) 글꼴을 사용 하 여 수학 문제를 표시 하는 합니다. 이 텍스트 세로 및 가로로 가운데에 표시 됩니다 플래시 카드. 



### <a name="create-the-initial-flashcardfragment-class"></a>초기 FlashCardFragment 클래스 만들기

이라는 새 파일 추가 **FlashCardFragment.cs** 내용을 다음 코드로 바꿉니다.

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

이 코드를 스텁 필수 아웃 `Fragment` 플래시 카드를 표시 하는 데 사용할 정의 합니다. 사실은 `FlashCardFragment` 의 지원 라이브러리 버전에서 파생 됩니다 `Fragment` 에 정의 된 `Android.Support.V4.App.Fragment`합니다. 생성자가 비어 있도록 합니다 `newInstance` 팩터리 메서드는 새로 만드는 데 `FlashCardFragment` 생성자를 대신 합니다. 

합니다 `OnCreateView` 수명 주기 메서드를 만들고 구성 합니다 `TextView`합니다. 조각에 대 한 레이아웃을 확장 하기 `TextView` 배포용를 반환 하 고 `TextView` 호출자에 게 합니다. `LayoutInflater` 및 `ViewGroup` 에 전달 됩니다 `OnCreateView` 레이아웃을 확장할 수 있도록 합니다. `savedInstanceState` 데이터 번들에 포함 되어 있는 `OnCreateView` 를 다시 사용 하 여를 `TextView` 저장된 된 상태에서. 

조각의 보기 호출에 의해 명시적으로 팽창 됩니다 `inflater.Inflate`합니다. 합니다 `container` 인수는 뷰의 부모 및 `false` 플래그 지시 하지만된 보기 보기의 부모에 추가 하지 않는 데이터 (이 추가할 때 `ViewPager` 호출의 어댑터의 `GetItem` 뒷부분에서는이 메서드 연습)입니다. 



### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment에 상태 코드를 추가 합니다.

활동을 같은 조각에는 `Bundle` 저장 하 고 해당 상태를 검색 하는 합니다. **FlashCardPager**이 `Bundle` 질문을 저장 하 고 연결된 된 플래시 카드에 대 한 텍스트에 대답 하는 데 사용 됩니다. **FlashCardFragment.cs**, 다음을 추가 합니다 `Bundle` 키의 맨 위에 `FlashCardFragment` 클래스 정의: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

수정 된 `newInstance` 팩터리 메서드를 만듭니다를 `Bundle` 개체를 사용 하 여 위의 키를 전달 된 질문을 저장 하 고 인스턴스화된 후 조각에서 텍스트를 대답: 

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

조각 수명 주기 메서드를 수정 `OnCreateView` 전달 된 번들에서이 정보를 검색 하 여 질문 텍스트를 로드 합니다 `TextBox`: 

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

`answer` 변수 여기에서 사용 되지 않지만 이벤트 처리기 코드는이 파일에 추가 되 면 나중에 사용할 수는 있습니다. 


## <a name="create-the-adapter"></a>어댑터 만들기

`ViewPager` 사이 위치 하는 어댑터 컨트롤러 개체를 사용 하는 `ViewPager` 및 데이터 원본 (의 ViewPager에서 그림을 참조 하세요 [어댑터](~/android/user-interface/controls/view-pager/index.md#adapter) 문서). 이 데이터에 액세스할 `ViewPager` 에서 파생 된 사용자 지정 어댑터를 제공 해야 `PagerAdapter`합니다. 이 예제에서는 조각이 있으므로 사용을 `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` 에서 파생 된 `PagerAdapter`합니다. 
`FragmentPagerAdapter` 각 페이지를 나타내는 `Fragment` 는 영구적으로 유지 됩니다에 대 한 조각 관리자에서 사용자 페이지에 반환할 수 있습니다. 페이지를 통해 사용자 천공 기와으로 `ViewPager`는 `FragmentPagerAdapter` 데이터 원본에서 정보를 추출 하 고 사용 하 여 만듭니다 `Fragment`에 대 한은 `ViewPager` 표시할 합니다. 

구현 하는 경우는 `FragmentPagerAdapter`, 다음 재정의 해야 합니다.

-   **개수** &ndash; 보기 (페이지) 사용할 수 있는 수를 반환 하는 읽기 전용 속성입니다.

-   **GetItem** &ndash; 지정된 된 페이지를 표시 하려면 조각을 반환 합니다.

이라는 새 파일 추가 **FlashCardDeckAdapter.cs** 내용을 다음 코드로 바꿉니다.

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

이 코드를 스텁 필수 아웃 `FragmentPagerAdapter` 구현 합니다. 다음 섹션에서는 이러한 메서드의 작업 코드를 사용 하 여 바뀝니다. 생성자의 목적은 조각 관리자를 전달 하는 것은 `FlashCardDeckAdapter`의 기본 클래스 생성자입니다. 



### <a name="implement-the-adapter-constructor"></a>구현 하며 어댑터 생성자

앱을 인스턴스화하는 `FlashCardDeckAdapter`, 조각 관리자 및 인스턴스화된에 대 한 참조를 제공 `FlashCardDeck`합니다. 맨 위에 다음 멤버 변수를 추가 합니다 `FlashCardDeckAdapter` 클래스의 **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

코드의 다음 줄을 추가 합니다 `FlashCardDeckAdapter` 생성자: 

```csharp
this.flashCardDeck = flashCards;
```

이 부분은 저장소 코드를 `FlashCardDeck` 인스턴스를 `FlashCardDeckAdapter` 사용 됩니다. 



### <a name="implement-count"></a>구현 개수

`Count` 구현은 비교적 간단한: 플래시 카드 데크의 플래시 카드의 수를 반환 합니다. `Count`를 다음 코드로 바꿉니다. 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


합니다 `NumCards` 속성의 `FlashCardDeck` 데이터 집합의 플래시 카드 (조각 수)의 수를 반환 합니다. 



### <a name="implement-getitem"></a>GetItem 구현

`GetItem` 메서드가 지정된 된 위치와 관련 된 조각 반환 합니다. 때 `GetItem` 라고 플래시 카드 데크의 위치에 대 한 반환을 `FlashCardFragment` 해당 위치의 플래시 카드 문제를 표시 하도록 구성 합니다. `GetItem` 메서드를 다음 코드로 바꿉니다. 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

이 코드는 다음을 수행합니다.

1.  수학 문제 문자열 조회는 `FlashCardDeck` 데크 지정된 된 위치에 대 한 합니다. 

2.  응답 문자열을 조회 합니다 `FlashCardDeck` 데크 지정된 된 위치에 대 한 합니다. 

3.  호출 된 `FlashCardFragment` 팩터리 메서드 `newInstance`플래시 카드 문제와 응답 문자열에 전달 합니다. 

4.  만들고 새 플래시 카드 반환 `Fragment` 해당 위치에 대 한 질문 및 답변 텍스트를 포함 하는 합니다. 

경우는 `ViewPager` 렌더링 합니다 `Fragment` 에서 `position`, 표시를 `TextBox` 에 있는 수학 문제 문자열을 포함 하 `position` 플래시 카드 데크의. 



## <a name="add-the-adapter-to-the-viewpager"></a>ViewPager에 어댑터 추가

이제는 `FlashCardDeckAdapter` 는 구현 것에 추가 하는 `ViewPager`합니다. **MainActivity.cs**, 다음 코드 줄의 끝에 추가 된 `OnCreate` 메서드:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

이 코드를 인스턴스화하는 `FlashCardDeckAdapter`전달는 `SupportFragmentManager` 첫 번째 인수에 합니다. (합니다 `SupportFragmentManager` FragmentActivity의 속성에 대 한 참조를 가져오는 데 사용 되는 `FragmentManager` &ndash; 에 대 한 자세한 내용은 `FragmentManager`, 참조 [조각 관리](~/android/platform/fragments/managing-fragments.md).) 

핵심 구현을 완료 되었습니다. &ndash; 빌드하고 앱을 실행 합니다.
다음 스크린샷에서 왼쪽에 표시 된 것 처럼 화면에 나타나는 플래시 카드 데크의 첫 번째 이미지에 표시 됩니다. 안쪽으로 살짝 밀어 왼쪽에서 더 많은 플래시 카드를 참조 하세요. 그런 다음 플래시 카드 데크를 뒤로 이동 하려면 오른쪽을 살짝 밀어서:

[![호출기 표시기 없이 FlashCardPager 앱의 예제 스크린샷](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>호출기 표시기 추가

이 최소 `ViewPager` 구현 각 플래시 카드 데크를 표시 하지만 데크 내에서 사용자가 알려 주지 않고를 제공 합니다. 다음 단계를 추가 하는 것을 `PagerTabStrip`입니다. `PagerTabStrip` 문제에 대 한 번호 표시 되 고 이전 및 다음 플래시 카드의 힌트를 표시 하 여 탐색 컨텍스트를 제공 하는 사용자에 게 알립니다. 

오픈 **Resources/layout/Main.axml** 추가한는 `PagerTabStrip` 레이아웃:

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

빈 빌드하고 앱을 실행 하는 경우 표시 `PagerTabStrip` 각 플래시 카드의 맨 위에 표시 합니다. 

[![텍스트 없이 PagerTabStrip의 클로즈업](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>제목 표시

각 페이지 탭에 제목에 추가 하려면 구현 합니다 `GetPageTitleFormatted` 어댑터에서 메서드. `ViewPager` 호출 `GetPageTitleFormatted` (구현 됨) 하는 경우 지정된 된 위치에 페이지를 설명 하는 제목 문자열을 가져오려고 합니다. 다음 메서드를 추가 합니다 `FlashCardDeckAdapter` 클래스의 **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

이 코드 플래시 카드 데크의 위치 문제 숫자로 변환합니다. 결과 문자열은 Java로 변환 `String` 에 반환 되는 여 `ViewPager`입니다. 이 새 메서드를 사용 하 여 앱을 실행 하는 경우 각 페이지에서 문제 수를 표시 합니다 `PagerTabStrip`: 

[![각 페이지 위쪽에 표시 되는 문제 수를 사용 하 여 FlashCardPager의 스크린샷](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

플래시 카드 데크의 각 플래시 카드의 맨 위에 있는 표시 되는 문제 수를 보려면 앞뒤로 살짝 수 있습니다. 



## <a name="handle-user-input"></a>사용자 입력 처리

**FlashCardPager** 일련의 조각 기반의 플래시 카드의 표시를 `ViewPager`, 하지만 아직 없는 각 문제에 대 한 답변을 표시 하는 방법입니다. 이 섹션에서는 이벤트 처리기에 추가 되는 `FlashCardFragment` 플래시 카드 문제가 되는 텍스트에 사용자가 누르면 답을 표시 합니다. 

오픈 **FlashCardFragment.cs** 의 끝에 다음 코드를 추가 하 고는 `OnCreateView` 메서드 뷰는 호출자에 게 반환 되기 전에만: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

이렇게 `Click` 이벤트 처리기에서 사용자가 누를 때 표시 되는 알림 답변 표시를 `TextBox`입니다. 합니다 `answer` 변수는 상태 정보에 전달 된 번들에서 읽은 경우 이전 초기화 된 `OnCreateView`합니다. 빌드 앱을 실행 하 고 문제가 되는 텍스트에 대 한 답을 각 플래시 카드 탭: 

[![스크린 샷을의 FlashCardPager 앱 수학 문제를 탭 할 때 알림](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

**FlashCardPager** 이 연습에서 사용 하는 `MainActivity` 에서 파생 된 `FragmentActivity`를 파생할 수도 있습니다 하지만 `MainActivity` 에서 `AppCompatActivity` (하는 지원도 제공 조각 관리에 대 한). 보려는 `AppCompatActivity` 예제를 참조 하세요 [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) 샘플 갤러리에서. 



## <a name="summary"></a>요약

이 연습에서는 기본 빌드하는 방법의 단계별 예제를 제공 `ViewPager`-사용 하 여 앱 기반 `Fragment`s입니다. 플래시 카드 질문과 대답을 포함 하는 예제 데이터 소스를 제공을 `ViewPager` 플래시 카드를 표시 하는 레이아웃 및 `FragmentPagerAdapter` 연결 하는 하위 클래스는 `ViewPager` 데이터 원본에. 지침 플래시 카드를 통해 이동 하는 사용자를 위해 포함 된 추가 하는 방법을 설명 하는 `PagerTabStrip` 각 페이지의 맨 위에 있는 문제 수를 표시 하려면. 마지막으로 이벤트 처리 코드는 사용자가 플래시 카드 문제를 해결할 때 답을 표시에 추가 되었습니다. 



## <a name="related-links"></a>관련 링크

- [FlashCardPager (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
