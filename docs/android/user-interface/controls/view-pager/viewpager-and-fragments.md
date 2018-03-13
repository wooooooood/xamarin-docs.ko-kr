---
title: "조각 사용 하 여 ViewPager"
description: "ViewPager gestural 탐색을 구현할 수 있게 하는 레이아웃 관리자입니다. Gestural 탐색 왼쪽과 오른쪽의 데이터 페이지를 통해 단계로 살짝에 사용자를 수 있습니다. 이 가이드에서는 조각을 사용 하 여 데이터 페이지와 ViewPager swipeable UI를 구현 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 62B6286F-3680-48F3-B91B-453692E457E5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: cd71617cce209ef0127023f69c2b503fee031e43
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="viewpager-with-fragments"></a>조각 사용 하 여 ViewPager

_ViewPager gestural 탐색을 구현할 수 있게 하는 레이아웃 관리자입니다. Gestural 탐색 왼쪽과 오른쪽의 데이터 페이지를 통해 단계로 살짝에 사용자를 수 있습니다. 이 가이드에서는 조각을 사용 하 여 데이터 페이지와 ViewPager swipeable UI를 구현 하는 방법을 설명 합니다._

 
## <a name="overview"></a>개요

`ViewPager` 대개 조각와 함께에서 보다 쉽게의 각 페이지의 수명 주기를 관리할 수 있도록는 `ViewPager`합니다. 이 연습에서는 `ViewPager` 만드는 데 사용 되는 응용 프로그램 호출 **FlashCardPager** 플래시 카드에는 일련의 수학 문제를 표시 하 합니다. 각 플래시 카드 조각으로 구현 됩니다. 사용자 천공 기와 왼쪽 및 오른쪽 flash 카드를 통해 및 해당 응답을 표시 하기 위해 수학 문제를 탭 합니다. 이 앱을 만듭니다는 `Fragment` 어댑터에서 파생 된 각 플래시 카드 및 구현에 대 한 인스턴스 `FragmentPagerAdapter`합니다. [Viewpager 뷰와](~/android/user-interface/controls/view-pager/viewpager-and-views.md), 대부분의 작업에서 수행 된 `MainActivity` 수명 주기 메서드. **FlashCardPager**, 대부분의 작업 수행 됩니다는 `Fragment` 의 수명 주기 메서드 중 하나에 있습니다. 

이 가이드에 조각의 기본 사항 다루지 않습니다 &ndash; 모르는 아직 Xamarin.Android의 조각으로, 참조 [조각](~/android/platform/fragments/index.md) 조각을 사용 하 여 시작할 수 있도록 합니다. 



## <a name="start-an-app-project"></a>응용 프로그램 프로젝트를 시작 합니다.

라는 새 Android 프로젝트 만들기 **FlashCardPager**합니다. 그런 다음 NuGet 패키지 관리자를 시작 (NuGet 패키지를 설치 하는 방법에 대 한 자세한 내용은 참조 [연습: 프로젝트에 포함 하는 NuGet](https://docs.microsoft.com/visualstudio/mac/nuget-walkthrough)). 찾기 및 설치는 **Xamarin.Android.Support.v4** 에 설명 된 대로 패키지 [Viewpager 뷰와](~/android/user-interface/controls/view-pager/viewpager-and-views.md)합니다. 



## <a name="add-an-example-data-source"></a>예제 데이터 소스 추가

**FlashCardPager**, 데이터 소스를 나타내는 flash 카드는 `FlashCardDeck` 클래스; 원본 공급이이 데이터는 `ViewPager` 항목 콘텐츠로 합니다. `FlashCardDeck` 수학 문제 및 답변의 미리 만들어진 컬렉션을 포함 합니다. `FlashCardDeck` 생성자에 인수가 필요 합니다. 

```csharp
FlashCardDeck flashCards = new FlashCardDeck();
```

컬렉션에서 flash 카드의 `FlashCardDeck` 각 플래시 카드 인덱서가 액세스할 수 있도록 구성 됩니다. 예를 들어 다음 코드 줄 묶음에 네 번째 플래시 카드 문제를 검색합니다. 

```csharp
string problem = flashCardDeck[3].Problem;
```

이 코드 줄 앞에서 설명한 문제가에 대 한 해당 대답을 검색합니다.

```csharp
string answer = flashCardDeck[3].Answer;
```

때문에 구현 세부 사항 `FlashCardDeck` 이해 관련이 없는 `ViewPager`, `FlashCardDeck` 코드가 여기에 나열 되어 있지 합니다.
소스 코드를 `FlashCardDeck` 에 [FlashCardDeck.cs](https://github.com/xamarin/monodroid-samples/blob/master/UserInterface/FlashCardPager/FlashCardPager/FlashCardDeck.cs)합니다.
이 소스 파일을 다운로드 (복사한 새 코드를 붙여 넣습니다. **FlashCardDeck.cs** 파일)를 프로젝트에 추가 합니다.



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
```

이 XML 정의 `ViewPager` 전체 화면을 차지 하는 합니다. 정규화 된 이름을 사용 해야 **android.support.v4.view.ViewPager** 때문에 `ViewPager` 지원 라이브러리에 패키지 됩니다. `ViewPager` 에서만 사용할 수는 [Android 지원 라이브러리 v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/); Android SDK에서 사용할 수 없는 합니다.


## <a name="set-up-viewpager"></a>ViewPager 설정

편집 **MainActivity.cs** 다음 추가 `using` 문:

```csharp
using Android.Support.V4.View;
using Android.Support.V4.App;
```

변경 된 `MainActivity` 클래스 선언에서 파생 되도록 `AppCompatActivity`:

```csharp
public class MainActivity : FragmentActivity
```

`MainActivity` 파생 된`FragmentActivity` (대신 `Activity`) 때문에 `FragmentActivity` 조각의 지원을 관리 하는 방법을 알고 있습니다. `OnCreate` 메서드를 다음 코드로 바꿉니다. 

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

1.  보기에서 설정 하는 **Main.axml** 레이아웃 리소스입니다.

2.  검색에 대 한 참조는 `ViewPager` 레이아웃에서 합니다.

3.  새 인스턴스화합니다 `FlashCardDeck` 데이터 원본으로 합니다.

빌드하고이 코드를 실행 하는 경우 다음 스크린 샷에서 유사한 디스플레이 표시 되어야 합니다. 

[![빈 ViewPager 사용 앱을 FlashCardPager 스크린 샷](viewpager-and-fragments-images/01-initial-screen-sml.png)](viewpager-and-fragments-images/01-initial-screen.png#lightbox)

이 시점에서 `ViewPager` 채우기 사용 되는 조각 되어 있지 때문에 비어 있지는는 `ViewPager`, 데이터에서이 조각이 만들기 위한 어댑터 없는 및 **FlashCardDeck**합니다. 

다음 섹션에서는 `FlashCardFragment` 는 만드는 각 플래시 카드의 기능을 구현 하는 및 `FragmentPagerAdapter` 연결할 만들어집니다는 `ViewPager` 데이터에서 만든 조각에는 `FlashCardDeck`합니다. 



## <a name="create-the-fragment"></a>조각 만들기

각 플래시 카드 라는 UI 조각에서 관리할 `FlashCardFragment`합니다. `FlashCardFragment`보기는 단일 플래시 카드와 포함 된 정보에 표시 됩니다. 각 인스턴스에 `FlashCardFragment` 에서 호스팅되는 `ViewPager`합니다. 
`FlashCardFragment`뷰 구성 됩니다는 `TextView` 플래시 카드 문제가 되는 텍스트를 표시 하는 합니다. 이 보기를 사용 하는 이벤트 처리기를 구현 합니다는 `Toast` 사용자 플래시 카드 질문을 누르면 답변을 표시 합니다. 



### <a name="create-the-flashcardfragment-layout"></a>FlashCardFragment 레이아웃 만들기

하기 전에 `FlashCardFragment` 수 구현 레이아웃을 정의 해야 합니다. 이 레이아웃은 하나의 조각에 대 한 조각 컨테이너 레이아웃입니다. 새 Android 레이아웃에 추가 **리소스/레이아웃** 호출 **flashcard_layout.axml**합니다. 열기 **Resources/layout/flashcard_layout.axml** 해당 내용을 다음 코드로 바꿉니다. 

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

이 레이아웃 단일 플래시 카드 조각; 정의 각 조각은 이루어진는 `TextView` 큰 (100sp) 글꼴을 사용 하 여 수학 문제를 표시 하는 합니다. 이 텍스트 세로 및 가로로 가운데에 표시 됩니다 플래시 카드입니다. 



### <a name="create-the-initial-flashcardfragment-class"></a>초기 FlashCardFragment 클래스 만들기

라는 새 파일 추가 **FlashCardFragment.cs** 해당 내용을 다음 코드로 바꿉니다.

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

이 코드는 필수적이 지 끊고 `Fragment` 플래시 카드 표시 하는 정의 합니다. `FlashCardFragment` 의 지원 라이브러리 버전에서 파생 된 `Fragment` 에 정의 된 `Android.Support.V4.App.Fragment`합니다. 생성자가 비어 있도록는 `newInstance` 팩터리 메서드는 새 데 `FlashCardFragment` 생성자를 대신 합니다. 

`OnCreateView` 수명 주기 메서드를 만들고 구성 합니다.는 `TextView`합니다. 조각에 대 한 레이아웃을 만들고 `TextView` 확장의 반환 및 `TextView` 호출자에 게 있습니다. `LayoutInflater` 및 `ViewGroup` 에 전달 `OnCreateView` 레이아웃을 확장할 수 있도록 합니다. `savedInstanceState` 번들 데이터를 포함 하는 `OnCreateView` 를 다시 사용 하는 `TextView` 저장된 된 상태에서. 

조각에서 보기에 대 한 호출에서 명시적으로 확장 `inflater.Inflate`합니다. `container` 인수는 보기의 부모 및 `false` 플래그 inflater 높여서 뷰 보기의 부모에 추가 하지 않는 것을 지시 (계층이 추가 됩니다. 때 `ViewPager` 호출의 어댑터의 `GetItem` 뒷부분에서는이 메서드 연습)입니다. 



### <a name="add-state-code-to-flashcardfragment"></a>FlashCardFragment에 상태 코드를 추가 합니다.

활동, 같은 조각에는 `Bundle` 를 사용 하 여 저장 하 고 해당 상태를 검색 합니다. **FlashCardPager**이, `Bundle` 질문을 저장 하 고 관련된 플래시 카드에 대 한 텍스트에 답변 하는 데 사용 됩니다. **FlashCardFragment.cs**, 다음 추가 `Bundle` 키의 맨 위에 `FlashCardFragment` 클래스 정의: 

```csharp
private static string FLASH_CARD_QUESTION = "card_question";
private static string FLASH_CARD_ANSWER = "card_answer";
```

수정 된 `newInstance` 팩터리 메서드를 만듭니다는 `Bundle` 개체를 사용 하 여 위 키 전달 된 질문을 저장 하 고 인스턴스화된 후 조각에 텍스트 응답: 

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

조각 수명 주기 메서드를 수정 `OnCreateView` 전달 된 번들에서이 정보를 검색 한 질문 텍스트를 로드 하는 `TextBox`: 

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

`ViewPager` 사이 있는 어댑터 컨트롤러 개체를 사용 하 여는 `ViewPager` 와 데이터 원본 (의 ViewPager에 그림을 참조 하십시오 [어댑터](~/android/user-interface/controls/view-pager/index.md#adapter) 문서). 이 데이터에 액세스 하려면 `ViewPager` 에서 파생 된 사용자 지정 어댑터를 제공 해야 `PagerAdapter`합니다. 이 예에서는 조각을 사용 하므로 사용는 `FragmentPagerAdapter` &ndash; `FragmentPagerAdapter` 에서 파생 된 `PagerAdapter`합니다. 
`FragmentPagerAdapter` 각 페이지를 나타내는 `Fragment` 하는 영구적으로 유지 됩니다에 대 한 조각 관리자에서 사용자 페이지에 반환할 수 있습니다. 페이지를 통해 사용자 천공 기와로 `ViewPager`, `FragmentPagerAdapter` 데이터 원본에서 정보를 추출 하 고 사용 하 여 만듭니다 `Fragment`에 대 한은 `ViewPager` 표시 합니다. 

구현 하는 경우는 `FragmentPagerAdapter`, 다음 재정의 해야 합니다.

-   **Count** &ndash; 보기 (페이지) 사용할 수 있는 수를 반환 하는 읽기 전용 속성입니다.

-   **GetItem** &ndash; 지정된 된 페이지에 대해 표시할 부분을 반환 합니다.

라는 새 파일 추가 **FlashCardDeckAdapter.cs** 해당 내용을 다음 코드로 바꿉니다.

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

이 코드는 필수적이 지 끊고 `FragmentPagerAdapter` 구현 합니다. 다음 섹션에서 이러한 각 방법의 작업 코드로 대체 됩니다. 생성자의 목적은 조각 관리자를 전달 하는 `FlashCardDeckAdapter`의 기본 클래스 생성자입니다. 



### <a name="implement-the-adapter-constructor"></a>구현 하며 어댑터 생성자

응용 프로그램을 인스턴스화하는 `FlashCardDeckAdapter`, 조각 관리자 및 인스턴스화된에 대 한 참조를 제공 `FlashCardDeck`합니다. 맨 위에 다음 멤버 변수 추가 `FlashCardDeckAdapter` 클래스 **FlashCardDeckAdapter.cs**: 

```csharp
public FlashCardDeck flashCardDeck;
```

다음 줄의 코드를 추가 `FlashCardDeckAdapter` 생성자: 

```csharp
this.flashCardDeck = flashCards;
```

이 줄의 코드 저장소는 `FlashCardDeck` 인스턴스가 `FlashCardDeckAdapter` 사용 합니다. 



### <a name="implement-count"></a>구현 개수

`Count` 구현은 비교적 간단: 플래시 카드 묶음에 플래시 카드의 수를 반환 합니다. `Count`를 다음 코드로 바꿉니다. 

```csharp
public override int Count
{
    get { return flashCardDeck.NumCards; }
}
```


`NumCards` 속성 `FlashCardDeck` 데이터 집합의 플래시 카드 (조각 수)의 수를 반환 합니다. 



### <a name="implement-getitem"></a>GetItem 구현

`GetItem` 메서드는 지정된 된 위치와 관련 된 조각을 반환 합니다. 때 `GetItem` 반환 플래시 카드 데크에서 위치에 대해 호출 됩니다는 `FlashCardFragment` 해당 위치의 플래시 카드 문제를 표시 하도록 구성 합니다. `GetItem` 메서드를 다음 코드로 바꿉니다. 

```csharp
public override Android.Support.V4.App.Fragment GetItem(int position)
{
    return (Android.Support.V4.App.Fragment)
        FlashCardFragment.newInstance (
            flashCardDeck[position].Problem, flashCardDeck[position].Answer);
}
```

이 코드는 다음을 수행합니다.

1.  수학 문제 문자열을 찾습니다는 `FlashCardDeck` 데크 지정된 된 위치에 대 한 합니다. 

2.  응답 문자열을 찾습니다는 `FlashCardDeck` 데크 지정된 된 위치에 대 한 합니다. 

3.  호출 된 `FlashCardFragment` 팩터리 메서드 `newInstance`플래시 카드 문제 및 응답 문자열에 전달 합니다. 

4.  만들고 새 플래시 카드 반환 `Fragment` 해당 위치에 대 한 질문 및 대답 텍스트를 포함 하 합니다. 

경우는 `ViewPager` 렌더링는 `Fragment` 에서 `position`, 표시는 `TextBox` 에 있는 수학 문제 문자열이 포함 된 `position` 플래시 카드 묶음에 합니다. 



## <a name="add-the-adapter-to-the-viewpager"></a>어댑터는 ViewPager를 추가

이제는 `FlashCardDeckAdapter` 은 구현 하는 추가 하는 `ViewPager`합니다. **MainActivity.cs**, 코드의 다음 줄의 끝에 추가 `OnCreate` 메서드:

```csharp
FlashCardDeckAdapter adapter =
    new FlashCardDeckAdapter(SupportFragmentManager, flashCards);
viewPager.Adapter = adapter;
```

이 코드를 인스턴스화하는 `FlashCardDeckAdapter`에 전달 하는 `SupportFragmentManager` 첫 번째 인수에 있습니다. (의 `SupportFragmentManager` FragmentActivity의 속성에 대 한 참조를 가져오는 데 사용 됩니다는 `FragmentManager` &ndash; 에 대 한 자세한 내용은 `FragmentManager`, 참조 [관리 조각](~/android/platform/fragments/managing-fragments.md).) 

핵심 구현을 완료 되었습니다. &ndash; 빌드하고 응용 프로그램을 실행 합니다.
다음 스크린샷에서 왼쪽에 표시 된 것 처럼 화면에 나타나는 플래시 카드 묶음의 첫 번째 이미지에 표시 됩니다. 살짝 왼쪽에서 더 많은 flash 카드를 참조 한 다음 오른쪽으로 살짝을 플래시 카드 데크를 뒤로 이동:

[![호출기 표시기 없이 FlashCardPager 앱의 스크린 샷](viewpager-and-fragments-images/02-example-views-sml.png)](viewpager-and-fragments-images/02-example-views.png#lightbox)



## <a name="add-a-pager-indicator"></a>호출기 표시기 추가

이 최소 `ViewPager` 구현은 데크를 각 플래시 카드를 표시 하지만 묶음 내에 사용자가 없는 지표를 제공 합니다. 다음 단계는 추가 하는 `PagerTabStrip`합니다. `PagerTabStrip` 어떤 문제에 대 한 번호 표시 되 고 이전 및 다음 플래시 카드의 힌트를 표시 하 여 탐색 컨텍스트를 제공 하는 사용자에 게 알립니다. 

열기 **Resources/layout/Main.axml** 추가 `PagerTabStrip` 레이아웃에:

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

빈 빌드하고 앱을 실행할 때 나타납니다 `PagerTabStrip` 각 플래시 카드의 맨 위에 표시: 

[![텍스트 없이 PagerTabStrip의 클로즈업](viewpager-and-fragments-images/03-empty-pagetabstrip-sml.png)](viewpager-and-fragments-images/03-empty-pagetabstrip.png#lightbox)



### <a name="display-a-title"></a>제목 표시

각 페이지 탭에 제목을 추가 하려면 구현에서 `GetPageTitleFormatted` 어댑터에서 메서드. `ViewPager` 호출 `GetPageTitleFormatted` (구현) 하는 경우 지정된 된 위치에 페이지를 설명 하는 제목 문자열을 가져올 수 있습니다. 다음 메서드를 추가 `FlashCardDeckAdapter` 클래스 **FlashCardDeckAdapter.cs**: 

```csharp
public override Java.Lang.ICharSequence GetPageTitleFormatted(int position)
{
    return new Java.Lang.String("Problem " + (position + 1));
}
```

이 코드 위치 플래시 카드 묶음에는 문제가 숫자로 변환합니다. 결과 문자열은 Java로 변환 `String` 에 반환 되는 `ViewPager`합니다. 앱이 새 메서드를 실행 하면 각 페이지에 문제 수를 표시 하는 고 `PagerTabStrip`: 

[![각 페이지 위에 표시 된 문제 수와 FlashCardPager의 스크린샷](viewpager-and-fragments-images/04-pagetabstrip-sml.png)](viewpager-and-fragments-images/04-pagetabstrip.png#lightbox)

각 플래시 카드의 맨 위쪽에 표시 되는 플래시 카드 묶음에 문제 수를 확인할 간에 살짝 수 있습니다. 



## <a name="handle-user-input"></a>사용자 입력을 처리

**FlashCardPager** 일련의 조각을 기반 플래시 카드의 수를 표시 한 `ViewPager`, 이지만 각 문제에 대 한 대답을 표시 하는 방법을 아직 포함 되지 않습니다. 이 섹션에서는 이벤트 처리기에 추가 되는 `FlashCardFragment` 플래시 카드 문제가 되는 텍스트에 사용자가 누르면 답변을 표시 하려면. 

열기 **FlashCardFragment.cs** 의 끝에 다음 코드를 추가 하 고는 `OnCreateView` 메서드 뷰는 호출자에 게 반환 하기 바로 전에: 

```csharp
questionBox.Click += delegate
{
    Toast.MakeText(Activity.ApplicationContext,
            "Answer: " + answer, ToastLength.Short).Show();
};
```

이 `Click` 이벤트 처리기는 사용자가 누를 때 표시 되는 알림 메시지에 대 한 대답 표시는 `TextBox`합니다. `answer` 에 전달 된 번들에서 상태 정보를 읽으면 변수 이전 초기화 했습니다 `OnCreateView`합니다. 작성 하 고 앱을 실행 한 후 답변을 참조 하기 위한 각 플래시 카드 문제가 되는 텍스트를 탭: 

[![FlashCardPager 스크린샷 앱 알림 수학 문제 탭이 수행 되는 경우](viewpager-and-fragments-images/05-answer-sml.png)](viewpager-and-fragments-images/05-answer.png#lightbox)

**FlashCardPager** 이 연습에서 사용 하 여는 `MainActivity` 에서 파생 된 `FragmentActivity`, 산출할 수도 있지만 `MainActivity` 에서 `AppCompatActivity` (있는 지원도 제공 조각을 관리 하기 위한)입니다. 보려는 `AppCompatActivity` 예제에서는 참조 [FlashCardPager](https://developer.xamarin.com/samples/monodroid/UserInterface%5CFlashCardPager/) 샘플 갤러리에서 합니다. 



## <a name="summary"></a>요약

이 연습에서는 기본을 작성 하는 방법의 단계별 예제를 제공 `ViewPager`-사용 하 여 앱 기반 `Fragment`s입니다. 플래시 카드 질문 및 답변을 포함 하는 예제 데이터 소스를 표시 한 `ViewPager` 플래시 카드 표시 하는 레이아웃 및 `FragmentPagerAdapter` 연결 하는 하위 클래스는 `ViewPager` 데이터 원본에 합니다. 지침 탐색 플래시 카드 하는 사용자를 위해 제공 된 추가 하는 방법을 설명 하는 `PagerTabStrip` 각 페이지 맨 위에 있는 문제 수를 표시 하려면. 마지막으로, 이벤트 처리 코드는 플래시 카드 문제에 사용자가 누르면 답변을 표시 하려면 추가 되었습니다. 



## <a name="related-links"></a>관련 링크

- [FlashCardPager (샘플)](https://developer.xamarin.com/samples/monodroid/UserInterface/FlashCardPager)
