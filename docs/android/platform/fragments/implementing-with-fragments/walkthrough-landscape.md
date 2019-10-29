---
title: 조각 연습 - 2부
ms.prod: xamarin
ms.topic: tutorial
ms.assetid: 444A894D-5197-4726-934F-79BA80A71CB0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/26/2018
ms.openlocfilehash: 4d9ef88f39914f8fa5e578577ee9f6977c2bc88e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020263"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>조각 연습 &ndash; 가로

[조각 연습 &ndash; 1 부](./walkthrough.md) 에서는 휴대폰에서 더 작은 화면을 대상으로 하는 Android 앱에서 조각을 만들고 사용 하는 방법을 보여 주었습니다. 이 연습의 다음 단계는 태블릿에서 추가 가로 공간을 활용 하도록 응용 프로그램을 수정 하는 것입니다. &ndash;은 항상 재생 목록 (`TitlesFragment`)이 되 고 `PlayQuoteFragment`가 작업에 동적으로 추가 됩니다. 사용자가 선택한 항목에 esponse:

[태블릿에서 실행 되는 앱![](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

가로 모드에서 실행 되는 전화는 다음과 같은 향상 된 기능을 활용 합니다.

[가로 모드로 Android 휴대폰에서 실행 되는![앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>가로 방향을 처리 하도록 앱 업데이트

다음 수정 사항은 [조각 연습-휴대폰](./walkthrough.md) 에서 수행 된 작업을 기반으로 작성 됩니다.

1. `TitlesFragment`와 `PlayQuoteFragment`를 모두 표시 하는 대체 레이아웃을 만듭니다.
1. 장치에서 두 조각이 동시에 표시 되는지 여부를 확인 하 고 그에 따라 동작을 변경 `TitlesFragment`를 업데이트 합니다.
1. 장치가 가로 모드일 때 `PlayQuoteActivity`를 업데이트 합니다.

## <a name="1-create-an-alternate-layout"></a>1. 대체 레이아웃 만들기

Android 장치에서 주 활동을 만들 때 Android에서 장치 방향에 따라 로드할 레이아웃을 결정 합니다. 기본적으로 Android는 **Resources/layout/activity_main. xml** 레이아웃 파일을 제공 합니다. 가로 모드에서 로드 하는 장치의 경우 Android는 **Resources/layout-land/activity_main** 레이아웃 파일을 제공 합니다. Android [리소스](/xamarin/android/app-fundamentals/resources-in-android) 에 대 한 가이드에는 android에서 응용 프로그램에 대해 로드할 리소스 파일을 결정 하는 방법에 대 한 자세한 내용이 포함 되어 있습니다.

[대체](/xamarin/android/user-interface/android-designer/alternative-layout-views) 레이아웃 가이드에 설명 된 단계를 수행 하 여 **가로** 방향을 대상으로 하는 대체 레이아웃을 만듭니다. 그러면 프로젝트에 새 레이아웃 리소스 파일 ( **Resources/layout/activity_main**)이 추가 됩니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[솔루션 탐색기에서 대체 레이아웃![](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[Solution Pad에서 대체 레이아웃![](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

대체 레이아웃을 만든 후이 XML과 일치 하도록 파일 **Resources/layout-land/activity_main** 의 원본을 편집 합니다.

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

활동의 루트 뷰에는 `two_fragments_layout` 리소스 ID가 제공 되며, `fragment` 및 `FrameLayout`라는 두 개의 하위 뷰가 있습니다. `fragment` 정적으로 로드 되는 동안 `FrameLayout`는 런타임에 `PlayQuoteFragment`으로 바뀔 "자리 표시자"로 작동 합니다. `TitlesFragment`에서 새 재생을 선택할 때마다 `playquote_container` 새 `PlayQuoteFragment`인스턴스로 업데이트 됩니다.

각 하위 뷰는 부모의 전체 높이를 차지 합니다. 각 하위 뷰의 너비는 `android:layout_weight` 및 `android:layout_width` 특성에 의해 제어 됩니다. 이 예제에서 각 하위 뷰는 부모에 의해 제공 되는 너비의 50%를 차지 합니다. _레이아웃 가중치_에 대 한 자세한 내용은 [LinearLayout의 Google 문서](https://developer.android.com/guide/topics/ui/layout/linear.html) 를 참조 하세요.

## <a name="2-changes-to-titlesfragment"></a>2. TitlesFragment 변경

대체 레이아웃을 만든 후 `TitlesFragment`를 업데이트 해야 합니다. 앱이 하나의 활동에 두 조각을 표시 하는 경우 `TitlesFragment`는 부모 활동에서 `PlayQuoteFragment`를 로드 해야 합니다. 그렇지 않으면 `PlayQuoteFragment`를 호스팅하는 `PlayQuoteActivity`를 시작 해야 `TitlesFragment`. 부울 플래그는 사용 해야 하는 동작을 결정 하는 데 도움이 `TitlesFragment`. 이 플래그는 `OnActivityCreated` 메서드에서 초기화 됩니다.

먼저 `TitlesFragment` 클래스의 맨 위에 인스턴스 변수를 추가 합니다.

```csharp
bool showingTwoFragments;
```

그런 다음 `OnActivityCreated`에 다음 코드 조각을 추가 하 여 변수를 초기화 합니다. 

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

장치가 가로 모드로 실행 되는 경우 리소스 ID `playquote_container`를 사용 하는 `FrameLayout` 화면에 표시 되므로 `showingTwoFragments` `true`으로 초기화 됩니다. 장치가 세로 모드로 실행 되는 경우에는 `playquote_container` 화면에 있지 않으므로 `showingTwoFragments` `false`됩니다.

`ShowPlayQuote` 메서드는 조각에서 견적 &ndash;를 표시 하거나 새 활동을 시작 하는 방법을 변경 해야 합니다.  두 조각을 표시할 때 조각을 로드 하도록 `ShowPlayQuote` 메서드를 업데이트 합니다. 그렇지 않으면 작업을 시작 해야 합니다.

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

사용자가 현재 `PlayQuoteFragment`에 표시 되는 것과 다른 재생을 선택한 경우에는 새 `PlayQuoteFragment` 만들어지고 `FragmentTransaction`컨텍스트 내에서 `playquote_container` 내용을 바꿉니다.

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment에 대 한 전체 코드

`TitlesFragment`에 대 한 이전 변경 내용을 모두 완료 한 후 전체 클래스는 다음 코드와 일치 해야 합니다.

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

장치를 가로 모드로 사용할 경우에는 `PlayQuoteActivity` 필요 하지 않습니다. 장치가 가로 모드인 경우 `PlayQuoteActivity` 표시 되지 않아야 합니다. `PlayQuoteActivity`의 `OnCreate` 메서드를 업데이트 하 여 자신을 닫습니다. 이 코드는 `PlayQuoteActivity.OnCreate`의 최종 버전입니다.

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

이렇게 수정 하면 장치 방향에 대 한 검사가 추가 됩니다. 가로 모드인 경우 `PlayQuoteActivity` 자체를 닫습니다.

## <a name="4-run-the-application"></a>4. 응용 프로그램을 실행 합니다.

이러한 변경을 완료 한 후에는 앱을 실행 하 고 장치를 가로 모드로 회전 (필요한 경우) 한 다음 재생을 선택 합니다. 따옴표는 재생 목록과 동일한 화면에 표시 되어야 합니다.

[가로 모드로 Android 휴대폰에서 실행 되는![앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[Android 태블릿에서 실행 되는![앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
