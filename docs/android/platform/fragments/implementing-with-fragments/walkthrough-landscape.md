---
title: 조각 연습-2 부
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/26/2018
ms.openlocfilehash: 7ec8ad6ce428107d2255dd07c7e69c9e77780c09
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118242"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>조각 연습 &ndash; 가로

합니다 [조각 연습 &ndash; 1 부](./walkthrough.md) 만들고 조각 휴대폰에서 작은 화면을 대상으로 하는 Android 앱에서 사용 하는 방법을 보여 줍니다. 이 연습의 다음 단계는 태블릿에서 추가 가로 공간을 활용 하기 위해 응용 프로그램을 수정 하는 것 &ndash; 재생 목록을 항상 하는 하나의 활동만 됩니다 (합니다 `TitlesFragment`) 및 `PlayQuoteFragment` 동적으로 추가 됩니다 사용자 선택에 대 한 응답으로 작업 합니다.

[![태블릿에서 실행 되는 앱](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

이러한 향상에서 가로 모드에서 실행 되는 휴대폰도 도움이 됩니다.

[![Android 휴대폰에서 가로 모드에서 실행 중인 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>가로 방향 처리 하도록 앱 업데이트

다음과 같이 수정에서 수행 된 작업을 기반으로 구축 되며는 [조각 연습-휴대폰](./walkthrough.md)

1. 모두 표시 하려면 대체 레이아웃을 만들 합니다 `TitlesFragment` 고 `PlayQuoteFragment`입니다.
1. 업데이트 `TitlesFragment` 장치 조각을 모두를 동시에 표시 되는 경우를 감지 하 여 동작을 적절 하 게 변경 합니다.
1. 업데이트 `PlayQuoteActivity` 장치가 가로 모드에 있을 때을 닫을 수 있습니다.

## <a name="1-create-an-alternate-layout"></a>1. 대체 레이아웃

기본 활동 Android 장치에서 만들어지면 Android는 로드 하는 레이아웃 장치 방향에 따라 결정 합니다. 기본적으로 Android는 제공 된 **Resources/layout/activity_main.axml** 레이아웃 파일입니다. 가로 모드에서 로드 하는 장치에 대 한 Android가 제공 합니다 **Resources/layout-land/activity_main.axml** 레이아웃 파일입니다. 이 가이드에서 [Android 리소스](/xamarin/android/app-fundamentals/resources-in-android) 어떤 리소스를 응용 프로그램에 대 한 로드 파일 Android을 결정 하는 방법에 자세한 정보가 포함 됩니다.

대상으로 하는 대체 레이아웃을 만들 **가로** 에 설명 된 단계를 수행 하 여 방향 합니다 [대체 레이아웃](/xamarin/android/user-interface/android-designer/alternative-layout-views) 가이드입니다. 새 레이아웃 리소스 파일을 프로젝트에 추가 해야이 **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![솔루션 탐색기에서 대체 레이아웃](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Solution Pad의 대체 레이아웃](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

대체 레이아웃을 만든 후 파일의 소스를 편집 **Resources/layout-land/activity_main.axml** 이 XML을 일치 시킬 수 있도록 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/two_fragments_layout"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment android:name="FragmentSample.TitlesFragment"
        android:id="@+id/titles"
        android:layout_weight="1"
        android:layout_width="0px"
        android:layout_height="match_parent" />

    <FrameLayout android:id="@+id/playquote_container"
            android:layout_weight="1"
            android:layout_width="0px"
            android:layout_height="match_parent"
             />
</LinearLayout>
```

활동의 루트 뷰는 리소스 ID 제공 됩니다 `two_fragments_layout` 있고 두 하위 뷰를 `fragment` 및 `FrameLayout`합니다. 하는 동안 합니다 `fragment` 정적으로 로드 되는 `FrameLayout` 는 "자리 표시자로" 하 여 런타임 시 바꿀는 `PlayQuoteFragment`합니다. 새 재생이 선택 될 때마다 합니다 `TitlesFragment`, `playquote_container` 의 새 인스턴스를 사용 하 여 업데이트 됩니다는 `PlayQuoteFragment`합니다.

하위 뷰의 각 부모의 전체 높이 차지 합니다. 각 하위 뷰 너비에 의해 제어 되는 `android:layout_weight` 및 `android:layout_width` 특성입니다. 이 예제에서는 각 하위 뷰 너비의 50%를 차지 합니다 부모를 제공 합니다. 참조 [Google의 문서는 LinearLayout](https://developer.android.com/guide/topics/ui/layout/linear.html) 대 한 자세한 내용은 _레이아웃 가중치_합니다.

## <a name="2-changes-to-titlesfragment"></a>2. TitlesFragment 변경

업데이트 해야 하는 대체 레이아웃을 만든 후 `TitlesFragment`합니다. 때 앱에 표시 되는 2 개의 조각을 하나의 작업, 한 다음 `TitlesFragment` 로드 해야는 `PlayQuoteFragment` 부모 활동입니다. 이 고, 그렇지 `TitlesFragment` 시작 해야 합니다 `PlayQuoteActivity` 호스트는 `PlayQuoteFragment`합니다. 부울 플래그는 데 도움이 됩니다 `TitlesFragment` 사용 해야 하는 동작을 결정 합니다. 이 플래그는 초기화 된 `OnActivityCreated` 메서드.

먼저, 맨 위에 있는 인스턴스 변수를 추가 합니다 `TitlesFragment` 클래스:

```csharp
bool showingTwoFragments;
```

그런 다음 다음 코드 조각을 추가 `OnActivityCreated` 변수를 초기화 합니다. 

```csharp
var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
showingTwoFragments = quoteContainer != null &&
                      quoteContainer.Visibility == ViewStates.Visible;
if (showingTwoFragments)
{
    ListView.ChoiceMode = ChoiceMode.Single;
    ShowPlayQuote(selectedPlayId);
}
```

장치 가로 모드에서 실행 중인 경우 해당 `FrameLayout` 리소스 ID를 사용 하 여 `playquote_container` 화면에 표시 됩니다 있도록 `showingTwoFragments` 로 초기화 됩니다 `true`합니다. 장치가 실행 되 고 있는지 세로 모드에서는 다음 `playquote_container` 되지 화면의 따라서 `showingTwoFragments` 됩니다 `false`합니다.

합니다 `ShowPlayQuote` 메서드는 따옴표를 표시 하는 방법을 변경 해야 합니다. &ndash; 조각 또는 새 작업을 시작 합니다.  업데이트는 `ShowPlayQuote` 메서드 두 조각이 표시 하는 경우 조각 로드를 작업을 시작 해야이 고, 그렇지:

```csharp
void ShowPlayQuote(int playId)
{
    selectedPlayId = playId;
    if (showingTwoFragments)
    {
        ListView.SetItemChecked(selectedPlayId, true);

        var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

        if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
        {
            var container = Activity.FindViewById(Resource.Id.playquote_container);
            var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

            FragmentTransaction ft = FragmentManager.BeginTransaction();
            ft.Replace(Resource.Id.playquote_container, quoteFrag);
            ft.Commit();
        }
    }
    else
    {
        var intent = new Intent(Activity, typeof(PlayQuoteActivity));
        intent.PutExtra("current_play_id", playId);
        StartActivity(intent);
    }
}
```

사용자의 현재 표시 되는 것과에서 다른 play 선정 `PlayQuoteFragment`, 클릭 하 고 새 `PlayQuoteFragment` 만들어지고의 내용을 덮어씁니다를 `playquote_container` 컨텍스트 내에서 `FragmentTransaction`입니다.

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment에 대 한 전체 코드

에 모든 이전 변경 내용을 완료 한 후 `TitlesFragment`, 완전 한 클래스에는이 코드와 일치 해야 합니다.

```csharp
public class TitlesFragment : ListFragment
{
    int selectedPlayId;
    bool showingTwoFragments;

    public override void OnActivityCreated(Bundle savedInstanceState)
    {
        base.OnActivityCreated(savedInstanceState);
        ListAdapter = new ArrayAdapter<string>(Activity, Android.Resource.Layout.SimpleListItemActivated1, Shakespeare.Titles);

        if (savedInstanceState != null)
        {
            selectedPlayId = savedInstanceState.GetInt("current_play_id", 0);
        }


        var quoteContainer = Activity.FindViewById(Resource.Id.playquote_container);
        showingTwoFragments = quoteContainer != null &&
                                quoteContainer.Visibility == ViewStates.Visible;
        if (showingTwoFragments)
        {
            ListView.ChoiceMode = ChoiceMode.Single;
            ShowPlayQuote(selectedPlayId);
        }
    }

    public override void OnSaveInstanceState(Bundle outState)
    {
        base.OnSaveInstanceState(outState);
        outState.PutInt("current_play_id", selectedPlayId);
    }

    public override void OnListItemClick(ListView l, View v, int position, long id)
    {
        ShowPlayQuote(position);
    }

    void ShowPlayQuote(int playId)
    {
        selectedPlayId = playId;
        if (showingTwoFragments)
        {
            ListView.SetItemChecked(selectedPlayId, true);

            var playQuoteFragment = FragmentManager.FindFragmentById(Resource.Id.playquote_container) as PlayQuoteFragment;

            if (playQuoteFragment == null || playQuoteFragment.PlayId != playId)
            {
                var container = Activity.FindViewById(Resource.Id.playquote_container);
                var quoteFrag = PlayQuoteFragment.NewInstance(selectedPlayId);

                FragmentTransaction ft = FragmentManager.BeginTransaction();
                ft.Replace(Resource.Id.playquote_container, quoteFrag);
                ft.AddToBackStack(null);
                ft.SetTransition(FragmentTransit.FragmentFade);
                ft.Commit();
            }
        }
        else
        {
            var intent = new Intent(Activity, typeof(PlayQuoteActivity));
            intent.PutExtra("current_play_id", playId);
            StartActivity(intent);
        }
    }
}
```

## <a name="3-changes-to-playquoteactivity"></a>3. PlayQuoteActivity 변경

하나의 최종 세부 정보가 주의: `PlayQuoteActivity` 장치가 가로 모드에 있을 때는 필요 하지 않습니다. 장치가 가로 모드의 경우는 `PlayQuoteActivity` 표시 되어야 합니다. 업데이트를 `OnCreate` 메서드의 `PlayQuoteActivity` 자체 닫힙니다 되도록 합니다. 이 코드는의 최종 버전 `PlayQuoteActivity.OnCreate`:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    base.OnCreate(savedInstanceState);

    if (Resources.Configuration.Orientation == Android.Content.Res.Orientation.Landscape)
    {
        Finish();
    }

    var playId = Intent.Extras.GetInt("current_play_id", 0);
    var playQuoteFrag = PlayQuoteFragment.NewInstance(playId);
    FragmentManager.BeginTransaction()
                    .Add(Android.Resource.Id.Content, playQuoteFrag)
                    .Commit();
}
```

장치 방향에 대 한 검사를 추가 하는이 수정 합니다. 다음 가로 모드에 있을 경우 `PlayQuoteActivity` 자체 닫힙니다.

## <a name="4-run-the-application"></a>4. 응용 프로그램 실행

이러한 변경 내용은 앱을 실행 완료 되 면 가로 모드로 (필요한 경우) 장치를 회전할 하 고 재생을 선택 합니다. 견적 재생 목록와 동일한 화면에 표시 합니다.

[![Android 휴대폰에서 가로 모드에서 실행 중인 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android 태블릿에서 실행 되는 앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
