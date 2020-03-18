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
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020263"
---
# <a name="fragments-walkthrough-ndash-landscape"></a>조각 연습 &ndash; 가로

[조각 연습 &ndash; 1부](./walkthrough.md)에서는 휴대폰의 작은 화면을 대상으로 하는 Android 앱에서 조각을 만들고 사용하는 방법을 설명했습니다. 이 연습의 다음 단계에서는 태블릿의 추가 가로 공간을 활용하도록 애플리케이션을 수정합니다. 여기에는 항상 재생 목록에 표시되는 하나의 작업이 포함되며(`TitlesFragment`), `PlayQuoteFragment`는 사용자가 선택한 항목에 대한 응답으로 작업에 동적으로 추가됩니다.

[![태블릿에서 실행 되는 앱](./walkthrough-landscape-images/01-tablet-screenshot-sml.png)](./walkthrough-landscape-images/01-tablet-screenshot.png#lightbox)

또한 가로 모드로 실행되는 휴대폰이 이 향상 기능의 이점을 얻게 됩니다.

[![가로 모드로 Android 휴대폰에서 실행되는 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

## <a name="updating-the-app-to-handle-landscape-orientation"></a>가로 방향을 처리하도록 앱 업데이트

다음 수정 사항은 [조각 연습 - 휴대폰](./walkthrough.md)에서 수행된 작업을 기준으로 빌드됩니다.

1. `TitlesFragment` 및 `PlayQuoteFragment`를 모두 표시하도록 대체 레이아웃을 만듭니다
1. 디바이스가 두 조각을 동시에 표시하는지 확인하고 그에 따라 동작을 변경하도록 `TitlesFragment`를 업데이트합니다.
1. 디바이스가 가로 모드일 때 닫히도록 `PlayQuoteActivity`를 업데이트합니다.

## <a name="1-create-an-alternate-layout"></a>1. 대체 레이아웃 만들기

기본 작업이 Android 디바이스에 생성되면 Android가 디바이스 방향에 따라 로드할 레이아웃을 결정합니다. 기본적으로 Android는 **Resources/layout/activity_main.axml** 레이아웃 파일을 제공합니다. 가로 모드로 로드되는 디바이스의 경우 Android가 **Resources/layout-land/activity_main.axml** 레이아웃 파일을 제공합니다. [Android 리소스](/xamarin/android/app-fundamentals/resources-in-android)에 대한 가이드에는 Android가 애플리케이션에 대해 로드할 리소스 파일을 결정하는 방법에 대한 자세한 정보가 포함되어 있습니다.

[대체 레이아웃](/xamarin/android/user-interface/android-designer/alternative-layout-views) 가이드에 설명된 단계에 따라 **가로** 방향을 대상으로 하는 대체 레이아웃을 만듭니다. 그러면 프로젝트에 새로운 레이아웃 리소스 파일인 **Resources/layout/activity_main.axml**이 추가됩니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

[![솔루션 탐색기의 대체 레이아웃](./walkthrough-landscape-images/02-alternate-layout.w157-sml.png)](./walkthrough-landscape-images/02-alternate-layout.w157.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

[![Solution Pad의 대체 레이아웃](./walkthrough-landscape-images/02-alternate-layout.m743-sml.png)](./walkthrough-landscape-images/02-alternate-layout.m743.png#lightbox)

-----

대체 레이아웃을 만든 후, 다음 XML과 일치하도록 **Resources/layout-land/activity_main.axml** 파일의 소스를 편집합니다.

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

작업의 루트 뷰에 리소스 ID `two_fragments_layout`이 제공되고 `fragment` 및 `FrameLayout`이라는 2개의 하위 뷰가 포함됩니다. `fragment`가 정적으로 로드되는 동안 `FrameLayout`은 런타임에 `PlayQuoteFragment`로 대체되는 "자리 표시자" 역할을 수행합니다. `TitlesFragment`에서 새 희곡이 선택될 때마다 `playquote_container`가 `PlayQuoteFragment`의 새 인스턴스로 업데이트됩니다.

각 하위 뷰는 해당 부모의 전체 높이를 차지합니다. 각 하위 뷰의 너비는 `android:layout_weight` 및 `android:layout_width` 특성으로 제어됩니다. 이 예에서 각 하위 뷰는 부모가 제공하는 너비의 50%를 차지합니다. _레이아웃 너비_에 대한 자세한 내용은 [LinearLayout에 대한 Google 문서](https://developer.android.com/guide/topics/ui/layout/linear.html)를 참조하십시오.

## <a name="2-changes-to-titlesfragment"></a>2. TitlesFragment 변경 사항

대체 레이아웃이 생성된 후에는 `TitlesFragment`를 업데이트해야 합니다. 앱이 한 번의 작업에 두 조각을 표시할 경우 `TitlesFragment`가 부모 작업에 `PlayQuoteFragment`를 로드해야 합니다. 그렇지 않으면 `TitlesFragment`가 `PlayQuoteFragment`를 호스트하는 `PlayQuoteActivity`를 시작해야 합니다. 부울 플래그는 `TitlesFragment`가 사용할 동작을 결정하는 데 도움을 줍니다. 이 플래그는 `OnActivityCreated` 메서드로 초기화됩니다.

먼저 `TitlesFragment` 클래스 위에 인스턴스 변수를 추가합니다.

```csharp
bool showingTwoFragments;
```

그런 후 변수를 초기화하기 위해 다음 코드 조각을 `OnActivityCreated`에 추가합니다. 

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

디바이스가 가로 모드로 실행되는 경우 리소스 ID가 `playquote_container`인 `FrameLayout`이 화면에 표시되고, `showingTwoFragments`가 `true`로 초기화됩니다. 디바이스가 세로 모드로 실행되는 경우에는 `playquote_container`가 화면에 표시되지 않고, `showingTwoFragments`가 `false`가 됩니다.

`ShowPlayQuote` 메서드는 조각에서 인용구를 표시하는 방법을 변경하거나 새 작업을 시작해야 합니다.  2개의 조각을 표시할 때 하나의 조각을 로드하도록 `ShowPlayQuote` 메서드를 업데이트하거나 작업을 시작합니다.

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

사용자가 현재 `PlayQuoteFragment`에 표시된 것과 다른 희곡을 선택하면 새로운 `PlayQuoteFragment`가 생성되고 `playquote_container`의 콘텐츠를 `FragmentTransaction`의 컨텍스트 내에서 바꿉니다.

### <a name="complete-code-for-titlesfragment"></a>TitlesFragment에 대한 전체 코드

`TitlesFragment`에 대한 모든 이전 변경 사항을 완료한 후에는 전체 클래스가 다음 코드와 일치해야 합니다.

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

## <a name="3-changes-to-playquoteactivity"></a>3. PlayQuoteActivity 변경 사항

이제 수행해야 할 최종 세부 사항이 남았습니다. 디바이스가 가로 모드일 때는 `PlayQuoteActivity`가 필요하지 않습니다. 디바이스가 가로 모드이면 `PlayQuoteActivity`가 표시되지 않습니다. 자체적으로 닫히도록 `PlayQuoteActivity`의 `OnCreate` 메서드를 업데이트합니다. 이 코드는 `PlayQuoteActivity.OnCreate`의 최종 버전입니다.

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

이렇게 수정하면 디바이스 방향에 대한 확인이 추가됩니다. 가로 모드이면 `PlayQuoteActivity`가 자체적으로 닫힙니다.

## <a name="4-run-the-application"></a>4. 애플리케이션 실행

이러한 변경 사항이 완료되면 앱을 실행하고, 디바이스를 가로 모드로 회전한 후(필요한 경우), 희곡을 선택합니다. 인용구가 희곡 목록과 같은 화면에 표시됩니다.

[![가로 모드로 Android 휴대폰에서 실행되는 앱](./images/intro-screenshot-phone-land-sml.png)](./images/intro-screenshot-phone-land.png#lightbox)

[![Android 태블릿에서 실행되는 앱](./images/intro-screenshot-tablet-sml.png)](./images/intro-screenshot-tablet.png#lightbox)
