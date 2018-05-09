---
title: 연습-2 부 조각 수
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/26/2018
ms.openlocfilehash: 58291388d375a4fd9273c8e0cd46db3799966766
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="fragments-walkthrough-ndash-landscape"></a>연습에 조각의 &ndash; 가로

[조각 연습 &ndash; 1 부](./walkthrough.md) 만들고 조각 휴대폰에서 보다 작은 화면을 대상으로 하는 Android 응용 프로그램에서 사용 하는 방법에 설명 합니다. 이 연습에서는 다음 단계는 태블릿 extra 가로 공간을 활용 하도록 응용 프로그램을 수정 하 &ndash; 재생 목록이 항상 하나의 활동 됩니다 (의 `TitlesFragment`) 및 `PlayQuoteFragment` 동적으로 추가할 수 에 대 한 응답으로 사용자가 선택한 활동:

[![태블릿에서 실행 중인 앱](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

이 향상 된이 기능에서 가로 모드에서 실행 되는 휴대폰도 도움이 됩니다.

[![Android 휴대폰에서 가로 모드에서 실행 중인 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>가로 방향 처리 하도록 응용 프로그램 업데이트

다음과 같이 수정에 수행 된 작업 기반으로 구축 되며는 [조각 연습-전화](./walkthrough.md)

1. 모두 표시 하는 대체 레이아웃을 만듭니다는 `TitlesFragment` 및 `PlayQuoteFragment`합니다.
1. 업데이트 `TitlesFragment` 장치 표시 되는 조각을 모두 동시에 감지 하 여 동작을 적절 하 게 변경 합니다.
1. 업데이트 `PlayQuoteActivity` 를 가로 모드로 장치가 있을 때 닫습니다.

## <a name="1-create-an-alternate-layout"></a>1. 대체 레이아웃 만들기

기본 활동 Android 장치에서 만들어질 때 Android 장치 방향에 따라 로드 레이아웃을 결정 합니다. Android 기본적으로 제공 된 **Resources/layout/activity_main.axml** 레이아웃 파일. 가로 모드에서 로드 하는 장치에 대 한 Android는 제공 된 **Resources/layout-land/activity_main.axml** 레이아웃 파일. 가이드 [Android 리소스](/xamarin/android/app-fundamentals/resources-in-android) 어떤 리소스 파일을 응용 프로그램에 대 한 로드 Android을 결정 하는 방법에 대 한 자세한 정보를 포함 합니다.

대상으로 하는 대체 레이아웃을 만들려면 **가로** 방향에 설명 된 단계에 따라는 [대체 레이아웃](/xamarin/android/user-interface/android-designer/alternative-layout-views) 가이드입니다. 새 레이아웃 리소스 파일을 프로젝트에 추가 해야이 **Resources/layout/activity_main.axml**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![솔루션 탐색기에서 대체 레이아웃](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![솔루션 패드에서 대체 레이아웃](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

대체 레이아웃을 만든 후 파일의 원본을 편집 **Resources/layout-land/activity_main.axml** 이 XML 일치 하도록 합니다.

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

루트 뷰는 활동의 리소스 ID 지정 `two_fragments_layout` 및 두 개의 하위 뷰는 `fragment` 및 `FrameLayout`합니다. 동안는 `fragment` 정적으로 로드 되는 `FrameLayout` 자리 표시자로 사용 "" 하 여 실시간으로 대체 하는 `PlayQuoteFragment`합니다. 새 재생에서 선택 될 때마다는 `TitlesFragment`, `playquote_container` 의 새 인스턴스를 업데이트할 수는 `PlayQuoteFragment`합니다.

각 하위 보기 부모의 전체 높이 차지 합니다. 각 하위 뷰 너비에 의해 제어 되는 `android:layout_weight` 및 `android:layout_width` 특성입니다. 이 예제에서는 각 하위 뷰 너비의 50%를 차지 합니다 부모를 제공 합니다. 참조 [는 LinearLayout에서 Google의 문서](https://developer.android.com/guide/topics/ui/layout/linear.html) 대 한 자세한 내용은 _레이아웃 가중치_합니다.

## <a name="2-changes-to-titlesfragment"></a>2. TitlesFragment에 대 한 변경

업데이트 해야 하는 대체 레이아웃을 만든 후 `TitlesFragment`합니다. 때 응용 프로그램에 표시 되는 두 개의 조각을 하나의 활동 후 `TitlesFragment` 로드 해야는 `PlayQuoteFragment` 부모 활동에에서 있습니다. 그렇지 않으면 `TitlesFragment` 시작 하도록는 `PlayQuoteActivity` 호스트는 `PlayQuoteFragment`합니다. 부울 플래그는 데 도움이 됩니다 `TitlesFragment` 사용 해야 하는 동작을 결정 합니다. 이 플래그에서 초기화 되는 `OnActivityCreated` 메서드.

먼저 맨 위에 있는 인스턴스 변수를 추가 하는 `TitlesFragment` 클래스:

```csharp
bool showingTwoFragments;
```

그런 다음에 다음 코드 조각 추가 `OnActivityCreated` 는 변수를 초기화 합니다. 

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

장치 가로 모드에서 실행 중인 경우 하면 `FrameLayout` 리소스 ID 인 `playquote_container` 화면에 표시 되지 것입니다 하므로 `showingTwoFragments` 로 초기화 됩니다 `true`합니다. 장치가 세로 모드에서 다음 실행 중인 경우 `playquote_container` 됩니다 화면에 따라서 `showingTwoFragments` 됩니다 `false`합니다.

`ShowPlayQuote` 메서드는 따옴표를 표시 하는 방법을 변경 해야 합니다 &ndash; 조각 또는 새 활동을 시작 합니다.  업데이트는 `ShowPlayQuote` 개체 두 조각이 표시 하면 조각에 대 한 활동을 시작 해야 그렇지 않은 경우:

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

사용자가 선택한에 현재 표시 된 것과에서 다른 재생 하는 경우 `PlayQuoteFragment`, 다음 새 `PlayQuoteFragment` 만들어지고의 내용을 덮어씁니다는 `playquote_container` 의 컨텍스트에서 `FragmentTransaction`합니다.

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment에 대 한 전체 코드

에 대 한 모든 이전 변경을 완료 한 후 `TitlesFragment`, 완전 한 클래스에는이 코드와 일치 해야 합니다.

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

## <a name="3-changes-to-playquoteactivity"></a>3. PlayQuoteActivity에 대 한 변경

하나의 최종 세부 정보가 처리: `PlayQuoteActivity` 가로 모드로 장치가 있을 때 필요 하지 않습니다. 장치가 가로 모드 이면는 `PlayQuoteActivity` 노출 되지 않아야 합니다. 업데이트는 `OnCreate` 방식의 `PlayQuoteActivity` 를 자체 닫힙니다. 이 코드는의 최종 버전 `PlayQuoteActivity.OnCreate`:

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

장치 방향에 대 한 검사를 추가 하는이 수정 합니다. 다음 가로 모드에서이 `PlayQuoteActivity` 자체 닫힙니다.

## <a name="4-run-the-application"></a>4. 응용 프로그램 실행

이러한 변경이 완료 되 면 응용 프로그램을 실행 한 후에 가로 모드로 (필요한 경우) 장치를 회전할 하 고 재생을 선택 합니다. 인용 재생 목록으로 동일한 화면에 표시 됩니다.

[![Android 휴대폰에서 가로 모드에서 실행 중인 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android 태블릿에서 실행 중인 앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
